# Event Sourcing And Durability Deep Dive

This guide explains the pinned deepseek-harness design for making a long-running agent inspectable, resumable, and safe enough to recover after failure. It is a reading guide for the exact source copies in this repository, not a claim that every choice is universally correct.

Start with [0003: event-sourced sessions](../03-agent-context/0003-event-sourced-sessions-with-derived-message-history.md), then keep the distinction below explicit throughout the code:

> A fact can be accepted into the live session log before it is durable on storage. A successful durability barrier makes a stronger claim about recorded intent, but it does not make an external side effect exactly once.

## The Three Layers

| Layer | Owner | Question it answers | Primary code |
|---|---|---|---|
| Event log | `Session` | What immutable facts occurred, in order? | `packages/core/session/src/index.ts`, `types.ts` |
| Derived state | Session folds and projections | What should the model or UI see now? | `surface.ts`, `repair.ts`, `session-projection/` |
| Durable storage | Persistence backend plus coordinator | Which facts survive process loss? | `session-persistence/`, `session-persistence-jsonl/`, `session-persistence-sqlite/` |

The architecture deliberately does not make the derived model-message history the source of truth. The log is the source; messages, visible transcript nodes, token estimates, indexes, and UI projections are derived consumers. That prevents a summary, a cached transcript, or a renderer-specific view from becoming the only surviving record of what happened.

## Core Invariants

### 1. The session is append-only

A `SessionEvent` has a closed event type plus `seq`, `time`, and `data`. The next sequence number is the current log length. The constructor validates seeded history and live append checks preserve contiguous sequence numbers. There is no valid persisted state with a duplicate or skipped sequence number.

Read in this order:

1. `packages/core/session/src/types.ts` for the event vocabulary and event envelope.
2. `packages/core/session/src/index.ts` for `Session.append()`, immutable event snapshots, and the `seq = log.length` rule.
3. `packages/core/session/tests/session.spec.ts` and `properties.spec.ts` for behavioral evidence.

Why it matters: sequence continuity turns the log into an addressable ledger. A projection can cite the source events that produced an assembled message. A persistence backend can reject a batch that would create holes. A reader can tell exactly where a torn append stopped.

### 2. Raw transport evidence and model-visible messages are different facts

Assistant stream chunks can be recorded for trace and usage fidelity. The assembled `assistant/message` event is authoritative for normal message derivation and cites the earlier source chunk sequences. A raw chunk without a surface operation does not automatically become a model-visible message.

This is why the design can keep exact streaming evidence without asking every consumer to reassemble an arbitrary partial stream. See:

- `packages/core/session/src/surface.ts`
- `packages/core/session/src/chunk-rows.ts`
- `packages/llm/token-meter/src/index.ts`
- [0533: persist assembled assistant messages](../03-agent-context/0533-persist-assembled-assistant-messages-not-stream-chunks.md)

The practical rule is: retain raw evidence when it is needed for audit, replay, or metering; make one validated assembled event the normal semantic boundary.

### 3. Projections are replaceable; evidence is not

The transcript surface is an ordered projection over the log. Token breakdowns, session lists, titles, search indexes, and UI state are also projections. They may be rebuilt, cached, or changed without rewriting earlier facts.

Compaction works through this separation. It changes the *derived context surface* by shadowing a range behind a checkpoint and summary; it does not silently rewrite the event ledger. See [0133: compaction seam](../03-agent-context/0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md), `packages/compaction/compaction/src/checkpoint.ts`, and `packages/compaction/compaction-basic/src/region.ts`.

## One Turn, End To End

```mermaid
sequenceDiagram
  participant Loop as Agent loop
  participant S as Session log
  participant P as Persistence coordinator
  participant B as Durable backend
  participant M as Model adapter or tool

  Loop->>S: append request / step facts
  S-->>P: session/event (post-commit, non-blocking)
  P-->>B: bounded write-behind batch
  Loop->>S: flush before model request
  S->>P: awaited session/flush
  P->>B: drain all earlier writes
  B-->>P: durable append complete
  P-->>S: flush resolves
  Loop->>M: adapter request starts
  M-->>S: chunks, assembled message, tool calls/results
  Loop->>S: flush at the next semantic boundary
```

The first `append()` is the live fact boundary. It validates the event, assigns `seq` and `time`, updates the private log, and publishes a post-commit `session/event` notification. Listener errors do not roll back an accepted fact. The hot path therefore does not wait on filesystem I/O.

The later `SessionStore.flush()` is the durability boundary. It dispatches awaited `session/flush` listeners through the session store. The persistence coordinator listens there and drains the write-behind queue. Calling the raw event name directly would bypass the store-owned carrier and is intentionally not the supported API.

Relevant code:

- `packages/core/session/src/index.ts`: append, publication, `deriveMessages()`, and `flush()`.
- `packages/session/session-persistence/src/coordinator.ts`: live-session adoption, serialized writes, repair, and flush wiring.
- `packages/session/session-persistence/src/write-behind.ts`: bounded batching and drain semantics.
- [0110: bounded persistence batching](../03-agent-context/0110-bounded-session-persistence-write-batching.md).

## What A Flush Guarantees

For a live persisted session, a successful flush means the persistence listeners have drained the events that were accepted before that barrier according to the configured backend's append contract. In the JSONL backend, this includes its durable append procedure; in another backend the physical mechanism can differ.

Flush does **not** mean:

- the model will finish the request;
- a remote API or shell command has completed;
- an external side effect happened exactly once;
- another process can safely write the same session without a separate coordination protocol.

That distinction is central. Durability records the order of intent and observed outcome. Exactly-once external effects require an idempotency key, a transactional resource under the same commit protocol, or an explicit reconciliation workflow. The harness explicitly avoids claiming that a filesystem or remote side effect becomes exactly once merely because the matching tool call was checkpointed.

## Why Checkpoints Are Semantic, Not Periodic

The persistence backend can batch ordinary event appends for throughput. The checkpoint policy decides when batching is not acceptable:

1. Before the model adapter receives a request, the request prefix must be durable.
2. Before a top-level tool body can cause an external side effect, the recorded tool call must be durable.
3. At the agent's next-step boundary, the preceding response and ordered tool results must be durable before another request is constructed.

Read `packages/session/session-checkpoint-policy/src/index.ts` and [0328: checkpointing](../03-agent-context/0328-compaction-checkpoints-use-an-english-engineering-register.md).

The failure behavior is intentionally fail-closed:

- If the request checkpoint rejects, the adapter is not invoked.
- If the top-level tool checkpoint rejects, its body is not entered.
- If cancellation lands while the tool checkpoint is pending, the model receives the canonical `TOOL_ABORTED_BEFORE_DISPATCH` outcome rather than a false claim that execution began.
- Nested tool dispatches reuse the outer model-visible call's checkpoint; the outer boundary owns the durable execution intent.

This is a good pattern for agent harnesses because it avoids a misleading middle state: a tool body should not run while the durable record still says nothing about the call.

## Write-Behind Without Lost Ordering

The coordinator owns the difficult part of asynchronous persistence: it keeps one serialized write path per session identity and validates each append batch against the stored cursor. A batch is accepted only when its event sequence continues the stored log exactly.

The important design choices are:

- **Lazy materialization:** session metadata can exist without creating an artifact. The first real append materializes durable storage.
- **Contiguous batches:** persistence does not accept arbitrary event subsets or reorder live events.
- **Per-session serialization:** append, flush, live adoption, load, and repair do not interleave into conflicting storage writes for one session.
- **Bounded coalescing:** normal appends may be grouped for throughput, but `flush()` drains immediately instead of waiting for the coalescing delay.
- **Dispose quiescence:** shutdown drains active write controllers instead of treating process teardown as an implicit success condition.

The source to read is `packages/session/session-persistence/src/coordinator.ts`, especially the append path, its cursor checks, `session/event` listener, and `session/flush` listener. Then read `write-behind.ts` and its tests.

## Physical Durability: JSONL, Compression, And Torn Writes

`session-persistence-jsonl` stores a session header followed by an append-only event log. Its default physical representation uses independently decodable Zstandard frames. The first frame is constrained to the header record, and event rows can be packed without changing the logical event stream.

Important details:

- The header carries a format version. An unsupported future version is rejected before trying to validate it as the current shape; "newer format" is not mislabeled as generic corruption.
- JSONL packing can combine consecutive delta chunks into storage rows. The reader is layout-blind and reconstructs the same logical events.
- The reader tracks committed byte boundaries. A trailing incomplete JSONL record or torn compressed frame is not treated as a valid new event.
- Recovery can preserve complete logical events recovered from a partially written final frame, truncate the torn tail, then append the repaired tail and closers. The repair contract is deliberately not described as one universal atomic filesystem transaction.
- The backend uses a revision-stable read loop so a file changing between `stat` and `readFile` is retried rather than interpreted as a stable log.

Read:

1. `packages/session/session-persistence-jsonl/src/format.ts`
2. `packages/session/session-persistence-jsonl/src/zstd.ts`
3. `packages/session/session-persistence-jsonl/src/index.ts`
4. `packages/session/session-persistence-jsonl/tests/jsonl.spec.ts` and `zstd.spec.ts`
5. [0048: Zstandard JSONL](../03-agent-context/0048-zstandard-jsonl-session-logs.md) and [0100: large restore](../03-agent-context/0100-large-session-jsonl-restore-pipeline.md)

SQLite is also included as a persistence backend example in `packages/session/session-persistence-sqlite/`. Its indexed `readFrom()` capability illustrates why a backend with native sequence addressing can avoid scanning an entire sequential artifact just to load a suffix.

## Recovery: Preserve Truth, Then Close The Transcript

A process can die after a tool call is recorded but before its result is known. On recovery, auto-retrying is unsafe: the external action may already have occurred. The session repair routine therefore appends a synthetic result with code `TOOL_OUTCOME_UNKNOWN`, followed by necessary `step/end` and `turn/end` closers for an interrupted turn.

This gives the next model request an honest transcript:

- the tool was intended to run;
- the system cannot prove whether its external effect completed;
- read-only or idempotent work may be retried after inspection;
- side-effecting work requires verification or user confirmation.

The repair does not rewrite history to make the turn look clean. It continues the sequence after the final real event and tags the turn as interrupted. Read `packages/core/session/src/repair.ts`, `packages/core/session/tests/repair.spec.ts`, `packages/session/session-checkpoint-policy/tests/crash-recovery.e2e.ts`, and [0537: interrupted final turns](../03-agent-context/0537-truncate-interrupted-final-turns-on-load.md).

## Compaction Is Not Deletion Of The Ledger

Long sessions eventually exceed a model context budget. Compaction creates a durable checkpoint and a replacement summary for a selected derived-history region. The model's next request can use the compacted representation, while raw events remain available for recovery, replay, audit, and selective recall.

The practical distinction is:

| Concern | Preserve | May change |
|---|---|---|
| Event log | Ordered immutable events and their source sequence numbers | Nothing in place |
| Model context | Recent context plus compacted summary/checkpoint | Which older surface nodes are visible |
| Human/UI projection | Traceability to original evidence | Presentation and grouping |
| Token metering | Evidence needed to explain occupancy and usage | Cached estimates or derived breakdowns |

Read `packages/compaction/compaction/src/index.ts`, `checkpoint.ts`, `tool-pairing.ts`, `packages/compaction/compaction-basic/src/summarizer.ts`, and `region.ts`. Then read [0033: overflow recovery](../03-agent-context/0033-after-call-compaction-pressure-and-context-overflow-recovery.md), [0215: manual compaction lock](../03-agent-context/0215-queued-manual-compaction-with-one-durable-lock.md), and [0518: recallable checkpoints](../03-agent-context/0518-recallable-compaction-index-checkpoints-a-state-checkpoint-and-in-sessio.md).

## Compatibility And Versioning

Durable data outlives a running binary. The implementation therefore treats format and event compatibility as runtime safety concerns, not a best-effort parsing detail.

- Header format version is checked before current-version structural validation.
- Event sequence contiguity is validated on load and append.
- Unknown non-ignorable event types fail load rather than being silently skipped and changing semantics.
- Migration support belongs in an explicit preparation/load boundary, where the system can identify and transform a known legacy form before exposing a live session.
- Session projections must tolerate rebuilding from the authoritative log rather than persisting a second mutable source of truth.

See [0122: log versioning](../03-agent-context/0122-session-log-versioning-one-integer-an-upgrade-chain-and-a-per-event-igno.md), `packages/core/session/src/known-event-types.ts`, `packages/core/session/src/preparation.ts`, and `packages/session/session-persistence/src/preparations.ts`.

## Failure Schedule To Test In A New Harness

Do not stop at append/replay unit tests. The minimum serious test matrix is:

1. Append valid events, restart, and derive the same model history.
2. Reject a duplicate or skipped sequence before storage mutation.
3. Crash after an in-memory append but before flush: prove the result matches the documented write-behind window.
4. Crash after a model request checkpoint but before the first streamed response chunk: resume without losing the request record.
5. Crash after a side-effecting tool call checkpoint but before its result: produce an unknown outcome, never an automatic replay.
6. Leave a partial physical append: recover the committed prefix and repair the tail deterministically.
7. Interrupt a turn with unmatched tool calls: add closers and preserve original source sequence references.
8. Compact an old history region: verify the new request surface shrinks while the audit log remains queryable.
9. Load a newer unsupported format: fail explicitly rather than interpreting it with current rules.
10. Race append and flush: preserve ordered, contiguous durable sequence numbers.

The included tests are useful reading material, even though this repository is a source mirror rather than a standalone runnable checkout:

- `packages/core/session/tests/session.spec.ts`
- `packages/core/session/tests/repair.spec.ts`
- `packages/session/session-persistence/tests/coordinator-contract.ts`
- `packages/session/session-persistence/tests/write-behind.spec.ts`
- `packages/session/session-persistence-jsonl/tests/jsonl.spec.ts`
- `packages/session/session-checkpoint-policy/tests/crash-recovery.e2e.ts`
- `packages/compaction/compaction-basic/tests/compaction-loop-repro.spec.ts`

## Porting Checklist

For a Rust or another-language harness, preserve these contracts rather than copying TypeScript package boundaries:

- Use a typed immutable event envelope with a monotonic session-local sequence number.
- Keep acceptance of an event separate from asynchronous physical persistence.
- Make the one durability barrier explicit and awaitable.
- Serialize persistence effects per session and validate contiguous appends at that boundary.
- Record externally visible execution intent before dispatching a side effect.
- Model unknown outcome as a first-class recovery result.
- Retain raw evidence, but define one assembled semantic event for normal derivation.
- Derive context, UI, and indexes from the event log; allow compaction to change views without erasing facts.
- Version the durable format and fail safely on unsupported input.
- Test process loss and partial writes as first-class behaviors, not incidental error handling.

## Note Index

- [0003: event-sourced sessions](../03-agent-context/0003-event-sourced-sessions-with-derived-message-history.md)
- [0009: persistence service](../03-agent-context/0009-session-persistence-as-an-abstract-service-over-the-existing-sessioneven.md)
- [0013: shared write coordinator](../03-agent-context/0013-shared-persistence-write-coordinator.md)
- [0025: reconstructable model requests](../03-agent-context/0025-every-llm-request-is-reconstructable-from-the-session-log.md)
- [0041: replay token meter](../03-agent-context/0041-replay-token-meter-service.md)
- [0110: bounded write batching](../03-agent-context/0110-bounded-session-persistence-write-batching.md)
- [0446: no mutable session summary](../03-agent-context/0446-drop-the-mutable-session-summary.md)
- [0447: trace facts become load-bearing events](../03-agent-context/0447-fold-trace-only-session-facts-into-load-bearing-events.md)
- [0454: simplified log representation](../03-agent-context/0454-simplify-session-log-representation.md)
- [0460: one flush controller](../03-agent-context/0460-collapse-live-persistence-into-one-flush-controller.md)
- [0511: projections and lifecycle logging](../03-agent-context/0511-session-projections-and-command-lifecycle-logging.md)
- [0677: JSONL durable artifact](../03-agent-context/0677-use-session-jsonl-as-the-only-snapshot-session-log-artifact.md)

The linked notes remain the authority on design status and rationale. This guide is intentionally specific to the pinned source commit.
