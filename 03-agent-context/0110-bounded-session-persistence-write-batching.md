---
id: "dsh-note-0110"
title: "Bounded session persistence write batching"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-08-bounded-session-persistence-write-batching.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/extensions"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/streaming"
aliases:
  - "seq"
  - "time"
  - "SESSION_FORMAT_VERSION"
  - "writeBatchMaxDelayMs"
  - "SCHEMA_VERSION"
  - "PersistenceCoordinator"
  - "SessionWriteBehind"
  - "assistant/chunk"
  - "goal-multi-turn-actions"
  - "permission-policy-context"
  - "appendBatch"
  - "session/flush"
  - "Bounded session persistence write batching"
  - "architecture"
search_regex: "(?i)(time|SESSION_FORMAT_VERSION|writeBatchMaxDelayMs|SCHEMA_VERSION|PersistenceCoordinator|SessionWriteBehind|assistant/chunk|goal\\-multi\\-turn\\-actions)"
---

# 0110. Bounded session persistence write batching — implementation context

## Open this when

Streaming responses can emit many assistant/chunk events in a short interval. The persistence coordinator previously scheduled a backend append as soon as an idle queue received one event. Events arriving while that append was active shared a follow-up batch, but a fast backend could still produce many small durable appends. Each JSONL append creates and syncs a Zstandard frame or raw suffix, while each SQLite append opens and commits a transaction and increments the session revision.

## Source decision

The first-party JSONL and SQLite plugins expose writeBatchMaxDelayMs, a positive integer no greater than Node's timer limit. Its default is 200. Each plugin resolves the value at load and passes it to PersistenceCoordinator; the coordinator remains the single owner of batching behavior. Each live Session receives a package-private SessionWriteBehind. When its pending queue changes from empty to non-empty, the controller starts one fixed window. Later events join that batch without resetting the deadline: this is bounded coalescing, not debounce.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-08-bounded-session-persistence-write-batching.md](../02-notes/implemented/architecture/2026-08-08-bounded-session-persistence-write-batching.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-08-bounded-session-persistence-write-batching.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-08-bounded-session-persistence-write-batching.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Defines `seq`, a construct named by the note. Defines `time`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Defines `SESSION_FORMAT_VERSION`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/repair.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts) | runtime implementation | Defines `seq`, a construct named by the note. Defines `time`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/chunk-rows.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts) | runtime implementation | Defines `time`, a construct named by the note. | `symbol-definition` |
| [`packages/schedule/schedule/src/domain.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/domain.ts) | runtime implementation | Defines `time`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `seq`, a construct named by the note. | `symbol-definition` |
| [`packages/workflow/tool-workflow/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/invariant.ts) | runtime contract checks | Defines `seq`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-persistence-jsonl/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/index.ts) | package entry point | Defines `writeBatchMaxDelayMs`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-persistence/src/coordinator.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/coordinator.ts) | runtime implementation | Defines `PersistenceCoordinator`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-persistence-sqlite/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-sqlite/src/index.ts) | package entry point | Defines `writeBatchMaxDelayMs`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-persistence/src/write-behind.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/write-behind.ts) | runtime implementation | Defines `SessionWriteBehind`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-persistence-sqlite/src/schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-sqlite/src/schema.ts) | runtime implementation | Defines `SCHEMA_VERSION`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `seq` | `const` | [`packages/client/connection/src/client/fixture.ts:362`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L362) | `const seq = events.length` |
| `time` | `let` | [`packages/core/session/src/chunk-rows.ts:284`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts#L284) | `let time = value.time0 as number` |
| `time` | `let` | [`packages/core/session/src/chunk-rows.ts:296`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts#L296) | `let time = row.time0` |
| `seq` | `const` | [`packages/core/session/src/index.ts:233`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L233) | `const seq = event['seq']` |
| `time` | `const` | [`packages/core/session/src/index.ts:234`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L234) | `const time = event['time']` |
| `seq` | `let` | [`packages/core/session/src/repair.ts:85`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts#L85) | `let seq = last.seq + 1` |
| `time` | `const` | [`packages/core/session/src/repair.ts:86`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts#L86) | `const time = last.time` |
| `SESSION_FORMAT_VERSION` | `const` | [`packages/core/session/src/types.ts:56`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L56) | `export const SESSION_FORMAT_VERSION = 0` |
| `time` | `const` | [`packages/schedule/schedule/src/domain.ts:277`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/domain.ts#L277) | `const time = timeMatch?.groups` |
| `writeBatchMaxDelayMs` | `const` | [`packages/session/session-persistence-jsonl/src/index.ts:155`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/index.ts#L155) | `const writeBatchMaxDelayMs = config.writeBatchMaxDelayMs` |
| `writeBatchMaxDelayMs` | `const` | [`packages/session/session-persistence-sqlite/src/index.ts:129`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-sqlite/src/index.ts#L129) | `const writeBatchMaxDelayMs = config.writeBatchMaxDelayMs` |
| `SCHEMA_VERSION` | `const` | [`packages/session/session-persistence-sqlite/src/schema.ts:20`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-sqlite/src/schema.ts#L20) | `export const SCHEMA_VERSION = 15` |
| `PersistenceCoordinator` | `class` | [`packages/session/session-persistence/src/coordinator.ts:588`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/coordinator.ts#L588) | `export class PersistenceCoordinator<TornMarker = unknown> {` |
| `SessionWriteBehind` | `class` | [`packages/session/session-persistence/src/write-behind.ts:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/write-behind.ts#L22) | `export class SessionWriteBehind {` |
| `seq` | `const` | [`packages/workflow/tool-workflow/src/invariant.ts:91`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/invariant.ts#L91) | `const seq = memberSeq(data.seq, fail)` |
| `seq` | `const` | [`packages/workflow/tool-workflow/src/invariant.ts:103`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/invariant.ts#L103) | `const seq = memberSeq(data.seq, fail)` |

### Tests and executable evidence

- [`apps/web/tests/snapshots/goal-multi-turn-actions/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/goal-multi-turn-actions/session.jsonl) — The source note names this file directly.
- [`apps/web/tests/snapshots/permission-policy-context/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/permission-policy-context/session.jsonl) — The source note names this file directly.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `SESSION_FORMAT_VERSION`.
- [`apps/web/tests/goal-multi-turn-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/goal-multi-turn-actions.e2e.ts) — A test under the owning area exercises or imports `goal-multi-turn-actions`.
- [`apps/web/tests/permission-policy-context.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/permission-policy-context.e2e.ts) — A test under the owning area exercises or imports `permission-policy-context`.
- [`packages/host/apiproxy/tests/api-proxy-cold.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-cold.spec.ts) — A test under the owning area exercises or imports `appendBatch`.
- [`packages/session/session-persistence-jsonl/tests/jsonl.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/tests/jsonl.spec.ts) — A test under the owning area exercises or imports `writeBatchMaxDelayMs`.
- [`packages/session/session-persistence/tests/persistence.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/tests/persistence.spec.ts) — A test under the owning area exercises or imports `PersistenceCoordinator`. A test under the owning area exercises or imports `appendBatch`.
- Source verification intent: The controller tests use a fake clock to prove the fixed, non-resetting 200 ms window; immediate and shared flush barriers; events admitted during a barrier; an over-budget tail behind an active write; ordered failure retention; paused automatic retry; and explicit retry of an overlapping background failure. Coordinator tests run the controller through Session notifications, retirement, collision reclamation, and teardown. The JSONL and SQLite suites retain their storage-format, transaction, recovery, and shared persistence-contract coverage.

## How to read the implementation

1. Start with [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/extensions`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/policy`, `mechanism/streaming`
- Aliases: `seq`, `time`, `SESSION_FORMAT_VERSION`, `writeBatchMaxDelayMs`, `SCHEMA_VERSION`, `PersistenceCoordinator`, `SessionWriteBehind`, `assistant/chunk`, `goal-multi-turn-actions`, `permission-policy-context`, `appendBatch`, `session/flush`, `Bounded session persistence write batching`, `architecture`
- Regex: `(?i)(time|SESSION_FORMAT_VERSION|writeBatchMaxDelayMs|SCHEMA_VERSION|PersistenceCoordinator|SessionWriteBehind|assistant/chunk|goal\-multi\-turn\-actions)`

```bash
rg -n --pcre2 "(?i)(time|SESSION_FORMAT_VERSION|writeBatchMaxDelayMs|SCHEMA_VERSION|PersistenceCoordinator|SessionWriteBehind|assistant/chunk|goal\\-multi\\-turn\\-actions)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0013. Shared persistence write coordinator](0013-shared-persistence-write-coordinator.md): The source note links to this decision directly.
- **`source-link`** — [0065. Make packed chunk rows the default JSONL layout](0065-make-packed-chunk-rows-the-default-jsonl-layout.md): The source note links to this decision directly.
- **`source-link`** — [0460. Collapse live persistence into one flush controller](0460-collapse-live-persistence-into-one-flush-controller.md): The source note links to this decision directly.
- **`source-link`** — [0533. Persist assembled assistant messages, not stream chunks](0533-persist-assembled-assistant-messages-not-stream-chunks.md): The source note links to this decision directly.
- **`shares-code-with`** — [0600. Web message IconActions and clocks](0600-web-message-iconactions-and-clocks.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/core/session/src/chunk-rows.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/repair.ts`.
- **`shares-code-with`** — [0012. Session surface --- an ordered projection over the event log](0012-session-surface-an-ordered-projection-over-the-event-log.md): Shares source implementation: `packages/core/session/src/chunk-rows.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0085. the end-seed log boundary](0085-the-end-seed-log-boundary.md): Shares source implementation: `packages/core/session/src/chunk-rows.ts`, `packages/core/session/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0110-bounded-session-persistence-write-batching.md`.
