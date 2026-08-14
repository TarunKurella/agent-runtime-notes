---
id: "dsh-note-0364"
title: "Owned-run finish reason reporting"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-11-owned-run-finish-reason.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/streaming"
aliases:
  - "completed"
  - "stopReason"
  - "events"
  - "FinishReason"
  - "SdkProtocolError"
  - "RunResult"
  - "finish_reason"
  - "turn/end"
  - "RunResult.finish_reason"
  - "data.reason.kind"
  - "RunResult.events"
  - "tool-calls"
  - "stop_reason"
  - "max-tokens"
search_regex: "(?i)(completed|stopReason|events|FinishReason|SdkProtocolError|RunResult|finish_reason|turn/end)"
---

# 0364. Owned-run finish reason reporting — implementation context

## Open this when

Python SDK consumers need a concise classification of how an owned activity interval reached idle. Requiring each consumer to scan raw turn/end events duplicates protocol knowledge, while a generic success status loses token-limit and model-error distinctions.

## Source decision

RunResult.finish_reason is the string kind from the last root-session turn/end collected between the submitted message's durable inbox receipt and the next whole-agent idle. It is None when the interval contains no turn/end. A turn/end without a string data.reason.kind raises SdkProtocolError instead of being reported as an interval without a turn ending. The field describes the owned run interval; it does not assign that ending to the submitted prompt. The owned-run boundary decision continues to prohibit prompt-level result attribution.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-11-owned-run-finish-reason.md](../02-notes/implemented/bug-fix/2026-08-11-owned-run-finish-reason.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-11-owned-run-finish-reason.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-11-owned-run-finish-reason.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Defines `FinishReason`, a construct named by the note. | `symbol-definition` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `stopReason`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `completed`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/client/src/api.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/api.ts) | runtime implementation | Defines `events`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/client/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/types.ts) | public types and contract | Defines `RunResult`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/src/process-shutdown.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/process-shutdown.ts) | runtime implementation | Defines `completed`, a construct named by the note. | `symbol-definition` |
| [`packages/e2b/fs-e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts) | package entry point | Defines `completed`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/client/src/client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/client.ts) | runtime implementation | Defines `SdkProtocolError`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Defines `events`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts) | package entry point | Defines `events`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-deepseek/src/sse.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/sse.ts) | runtime implementation | Defines `events`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-pi-ai/src/adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts) | provider/backend adapter | Defines `events`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `completed` | `let` | [`apps/cli/src/process-shutdown.ts:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/process-shutdown.ts#L30) | `let completed = false` |
| `stopReason` | `const` | [`packages/acp/acp/src/index.ts:297`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L297) | `const stopReason = await new Promise<StopReason>((resolve, reject) => {` |
| `events` | `const` | [`packages/core/session/src/index.ts:1098`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L1098) | `const events = session.events` |
| `completed` | `let` | [`packages/e2b/fs-e2b/src/index.ts:258`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L258) | `let completed = false` |
| `completed` | `let` | [`packages/e2b/fs-e2b/src/index.ts:305`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L305) | `let completed = false` |
| `stopReason` | `const` | [`packages/hooks/hook-protocol/src/codec.ts:100`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/codec.ts#L100) | `const stopReason = str(parsed, 'stopReason')` |
| `events` | `const` | [`packages/llm/llm-deepseek/src/sse.ts:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/sse.ts#L32) | `const events = stream` |
| `events` | `const` | [`packages/llm/llm-pi-ai/src/adapter.ts:313`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts#L313) | `const events = snapshot.models.streamSimple(model, context, {` |
| `completed` | `let` | [`packages/llm/llm/src/index.ts:872`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L872) | `let completed = false` |
| `FinishReason` | `type` | [`packages/llm/llm/src/types.ts:125`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L125) | `export type FinishReason = FinishReasonMap[keyof FinishReasonMap]` |
| `events` | `const` | [`packages/sdk/client/src/api.ts:150`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/api.ts#L150) | `const events: SessionEvent[] = []` |
| `SdkProtocolError` | `class` | [`packages/sdk/client/src/client.ts:59`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/client.ts#L59) | `export class SdkProtocolError extends Error {` |
| `RunResult` | `interface` | [`packages/sdk/client/src/types.ts:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/types.ts#L62) | `export interface RunResult {` |
| `stopReason` | `const` | [`packages/subagent/subagent-in-process-driver/src/index.ts:225`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-in-process-driver/src/index.ts#L225) | `const stopReason: SubagentStopReason = cancelled && recorded !== 'completed' ? 'aborted' : recorded` |
| `RunResult` | `interface` | [`packages/test-support/acp-snapshot/src/harness.ts:126`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/harness.ts#L126) | `export interface RunResult {` |
| `completed` | `const` | [`packages/typert/generator/src/analyzer.ts:1440`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L1440) | `const completed = new Map<ts.Type, TypeNodeId>()` |

### Tests and executable evidence

- [`python/sdk/tests/test_client.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_client.py) — A test under the owning area exercises or imports `finish_reason`. A test under the owning area exercises or imports `None`.
- [`scripts/translation-prompt.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-prompt.spec.ts) — A test under the owning area exercises or imports `None`.
- [`python/sdk/tests/test_smoke_model.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_smoke_model.py) — A test under the owning area exercises or imports `None`.
- [`apps/web/tests/schedule-after.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/schedule-after.e2e.ts) — A test under the owning area exercises or imports `tool-calls`.
- [`packages/acp/acp/tests/edges.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/edges.spec.ts) — A test under the owning area exercises or imports `stopReason`.
- [`packages/acp/acp/tests/turns.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/turns.spec.ts) — A test under the owning area exercises or imports `stopReason`.
- [`packages/acp/acp/tests/bridge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/bridge.spec.ts) — A test under the owning area exercises or imports `stopReason`.
- [`apps/web/tests/complex-history.perf.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/complex-history.perf.ts) — A test under the owning area exercises or imports `tool-calls`.
- Source verification intent: Python SDK tests cover selection of the last turn ending, an interval without a turn ending, and rejection of a malformed turn-ending reason. The SDK README documents the field's values, None case, failure behavior, and run-level scope.

## How to read the implementation

1. Start with [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/schema-types`, `domain/agent-loop`, `domain/build-release`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/streaming`
- Aliases: `completed`, `stopReason`, `events`, `FinishReason`, `SdkProtocolError`, `RunResult`, `finish_reason`, `turn/end`, `RunResult.finish_reason`, `data.reason.kind`, `RunResult.events`, `tool-calls`, `stop_reason`, `max-tokens`
- Regex: `(?i)(completed|stopReason|events|FinishReason|SdkProtocolError|RunResult|finish_reason|turn/end)`

```bash
rg -n --pcre2 "(?i)(completed|stopReason|events|FinishReason|SdkProtocolError|RunResult|finish_reason|turn/end)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0084. Follow-up enqueue and owned run boundaries](0084-follow-up-enqueue-and-owned-run-boundaries.md): The source note links to this decision directly.
- **`shares-code-with`** — [0308. Recursive Python SDK session notifications](0308-recursive-python-sdk-session-notifications.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0651. Drop unconsumed assembled LLM convenience surfaces](0651-drop-unconsumed-assembled-llm-convenience-surfaces.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/e2b/fs-e2b/src/index.ts`.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0326. The browser conversation is a log-ordered human transcript](0326-the-browser-conversation-is-a-log-ordered-human-transcript.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0498. Per-session snapshot replay for nested agents](0498-per-session-snapshot-replay-for-nested-agents.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/llm/llm/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0364-owned-run-finish-reason-reporting.md`.
