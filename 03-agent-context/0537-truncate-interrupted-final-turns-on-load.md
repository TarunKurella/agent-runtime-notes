---
id: "dsh-note-0537"
title: "Truncate interrupted final turns on load"
status: "rejected"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/rejected/simplification/2026-06-20-truncate-interrupted-turns.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/recovery"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/rejected"
  - "mechanism/policy"
aliases:
  - "interrupted"
  - "interruptedTurnClosers"
  - "TurnEndReasonMap"
  - "tool/result"
  - "step/end"
  - "turn/end { kind: 'interrupted' }"
  - "turn/start"
  - "turn/end"
  - "turn/end { interrupted }"
  - "Truncate interrupted final turns on load"
  - "simplification"
  - "boundary"
  - "compatibility"
  - "concurrency"
search_regex: "(?i)(interrupted|interruptedTurnClosers|TurnEndReasonMap|tool/result|step/end|turn/end[- ]\\{[- ]kind:[- ]'interrupted'[- ]\\}|turn/start|turn/end)"
---

# 0537. Truncate interrupted final turns on load — implementation context

## Open this when

The current persistence contract preserves a final turn that was durably written but never closed. On load, interruptedTurnClosers() scans the tail, synthesizes error tool/result events for unanswered tool calls, appends a step/end when a step is open, appends turn/end { kind: 'interrupted' }, and asks the backend to durably commit that repair. The coordinator, JSONL backend, SQLite backend, session event vocabulary, invariants, docs, and tests all model this synthetic close path. This is a lot of machinery to preserve partial work from the last crashed turn. It also invents events that never happened.

## Source decision

On load, keep only the last completed turn. A backend still tolerates and truncates a torn final record, but if the parsed durable prefix ends after an open turn/start, the canonical repair is to drop every event after the previous turn/end. No synthetic tool/result, no synthetic step/end, no turn/end { interrupted }, and no interrupted turn-end reason. This makes the persisted turn boundary simple: a completed turn/end is the checkpoint. Anything after the last checkpoint is crash tail. The next prompt resumes from the last known-valid provider transcript, not from a partially reconstructed final turn.

## Decision status

Rejected design. Do not implement it as written. Reuse its failure analysis, constraints, and reversal condition.

- Raw note: [rejected/simplification/2026-06-20-truncate-interrupted-turns.md](../02-notes/rejected/simplification/2026-06-20-truncate-interrupted-turns.md)
- Pinned source: [.agents/notes/rejected/simplification/2026-06-20-truncate-interrupted-turns.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/rejected/simplification/2026-06-20-truncate-interrupted-turns.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/session/session-persistence/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Defines `TurnEndReasonMap`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/repair.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts) | runtime implementation | Defines `interruptedTurnClosers`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-workflow-run/src/client/workflow-definition.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workflow-run/src/client/workflow-definition.ts) | runtime implementation | Defines `interrupted`, a construct named by the note. | `symbol-definition` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `tool/result` named by the note. Contains the exact code literal `step/end` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) | package contract and examples | Contains the exact code literal `tool/result` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.zh.md) | package contract and examples | Contains the exact code literal `tool/result` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `tool/result` named by the note. Contains the exact code literal `step/end` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `tool/result` named by the note. Contains the exact code literal `step/end` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Contains the exact code literal `tool/result` named by the note. Contains the exact code literal `step/end` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.zh.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `interrupted` | `const` | [`packages/client/ui-workflow-run/src/client/workflow-definition.ts:95`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workflow-run/src/client/workflow-definition.ts#L95) | `const interrupted = state.stopReason === undefined` |
| `interruptedTurnClosers` | `function` | [`packages/core/session/src/repair.ts:27`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts#L27) | `export function interruptedTurnClosers(events: readonly SessionEvent[]): SessionEvent[] {` |
| `TurnEndReasonMap` | `interface` | [`packages/core/session/src/types.ts:155`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L155) | `export interface TurnEndReasonMap {` |

### Tests and executable evidence

- [`packages/core/session/tests/repair.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/repair.spec.ts) — A test under the owning area exercises or imports `interruptedTurnClosers`.
- Source verification intent: TurnEndReasonMap drops the interrupted variant. interruptedTurnClosers() and its tests disappear. The persistence coordinator's repair hook truncates backend-specific torn/open tail state without appending closers. Session persistence docs say load returns the last completed turn, plus no partial final turn. Snapshot and contract tests update together with the behavior they pin. The session format version and recorded fixtures are refreshed; non-current stored logs are rejected per the pre-release format policy, with no migration path.

## How to read the implementation

1. Start with [`packages/session/session-persistence/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/README.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Rejected design. Do not implement it as written. Reuse its failure analysis, constraints, and reversal condition.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/recovery`, `domain/agent-loop`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/rejected`, `mechanism/policy`
- Aliases: `interrupted`, `interruptedTurnClosers`, `TurnEndReasonMap`, `tool/result`, `step/end`, `turn/end { kind: 'interrupted' }`, `turn/start`, `turn/end`, `turn/end { interrupted }`, `Truncate interrupted final turns on load`, `simplification`, `boundary`, `compatibility`, `concurrency`
- Regex: `(?i)(interrupted|interruptedTurnClosers|TurnEndReasonMap|tool/result|step/end|turn/end[- ]\{[- ]kind:[- ]'interrupted'[- ]\}|turn/start|turn/end)`

```bash
rg -n --pcre2 "(?i)(interrupted|interruptedTurnClosers|TurnEndReasonMap|tool/result|step/end|turn/end[- ]\\{[- ]kind:[- ]'interrupted'[- ]\\}|turn/start|turn/end)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0545. Every session event is enclosed in a turn](0545-every-session-event-is-enclosed-in-a-turn.md): The source note links to this decision directly.
- **`source-link`** — [0009. Session persistence as an abstract service over the existing `SessionEvent`](0009-session-persistence-as-an-abstract-service-over-the-existing-sessioneven.md): The source note links to this decision directly.
- **`source-link`** — [0535. Drop durable step boundary events](0535-drop-durable-step-boundary-events.md): The source note links to this decision directly.
- **`shares-code-with`** — [0466. Remove synthetic turns for log-only events](0466-remove-synthetic-turns-for-log-only-events.md): Shares source implementation: `docs/architecture.md`, `docs/config-catalog.md`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/session/src/repair.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0035. Agent-scope runtime design and correctness](0035-agent-scope-runtime-design-and-correctness.md): Shares source implementation: `packages/core/session/src/repair.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): Shares source implementation: `packages/core/session/src/repair.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0113. Client Conversation business-node assembly and keyed Chat snapshots](0113-client-conversation-business-node-assembly-and-keyed-chat-snapshots.md): Shares source implementation: `packages/core/session/src/repair.ts`, `packages/core/session/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0537-truncate-interrupted-final-turns-on-load.md`.
