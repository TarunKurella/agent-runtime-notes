---
id: "dsh-note-0616"
title: "TUI presents a reason for every turn-end kind"
status: "archived"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/bug-fix/2026-07-24-tui-turn-end-stop-reason-notices.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/lifecycle"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "completed"
  - "disposed"
  - "rejected"
  - "interrupted"
  - "aborted"
  - "TurnEndReasonMap"
  - "max-tokens"
  - "turn/end"
  - "packages/ui/tui/src/index.ts"
  - "Turn stopped: the agent was disposed."
  - "Turn ended: <kind>."
  - "agent/disposed"
  - "Agent \"<id>\" was disposed."
  - "errors-and-help"
search_regex: "(?i)(completed|disposed|rejected|interrupted|aborted|TurnEndReasonMap|max\\-tokens|turn/end)"
---

# 0616. TUI presents a reason for every turn-end kind — implementation context

## Open this when

The TUI rendered transcript notices for error, aborted, max-tokens, rejected, and interrupted turn ends, but a disposed turn end and any plugin-added TurnEndReasonMap kind rendered nothing. When such a turn ended --- live or replayed from a persisted log --- the agent stopped working with no visible reason, breaking the product expectation that every stop is explained to the user.

## Source decision

The turn/end case in packages/ui/tui/src/index.ts switches on the reason's discriminant and covers every kind: completed stays silent because the settled assistant message and its Completed timing header already present that outcome; disposed appends Turn stopped: the agent was disposed.; and the merge-extensible default appends Turn ended: . so an unknown plugin-added outcome still names why the agent stopped. All other kinds keep their existing notices.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/bug-fix/2026-07-24-tui-turn-end-stop-reason-notices.md](../02-notes/archived/bug-fix/2026-07-24-tui-turn-end-stop-reason-notices.md)
- Pinned source: [.agents/notes/archived/bug-fix/2026-07-24-tui-turn-end-stop-reason-notices.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/bug-fix/2026-07-24-tui-turn-end-stop-reason-notices.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `disposed`, a construct named by the note. Defines `completed`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/src/process-shutdown.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/process-shutdown.ts) | runtime implementation | Defines `completed`, a construct named by the note. | `symbol-definition` |
| [`packages/e2b/fs-e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts) | package entry point | Defines `completed`, a construct named by the note. | `symbol-definition` |
| [`packages/util/timeout/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/index.ts) | package entry point | Defines `disposed`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Defines `TurnEndReasonMap`, a construct named by the note. | `symbol-definition` |
| [`packages/plan/plan-mode/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/index.ts) | package entry point | Defines `disposed`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Defines `aborted`, a construct named by the note. | `symbol-definition` |
| [`scripts/verify-translation-pairing.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-translation-pairing.ts) | repository automation | Defines `rejected`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/bash-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts) | package entry point | Defines `aborted`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/pwsh-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/index.ts) | package entry point | Defines `aborted`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `rejected`, a construct named by the note. | `symbol-definition` |
| [`packages/mcp/mcp-client/src/connection.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/connection.ts) | runtime implementation | Defines `disposed`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `completed` | `let` | [`apps/cli/src/process-shutdown.ts:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/process-shutdown.ts#L30) | `let completed = false` |
| `disposed` | `let` | [`packages/client/runtime/src/client/slots.ts:376`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/slots.ts#L376) | `let disposed = false` |
| `rejected` | `const` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:426`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L426) | `const rejected = ((): string \| null => {` |
| `interrupted` | `const` | [`packages/client/ui-workflow-run/src/client/workflow-definition.ts:95`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workflow-run/src/client/workflow-definition.ts#L95) | `const interrupted = state.stopReason === undefined` |
| `aborted` | `const` | [`packages/core/agent-loop/src/index.ts:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L98) | `const aborted = Promise.withResolvers<never>()` |
| `aborted` | `let` | [`packages/core/agent-loop/src/tool-calls.ts:138`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/tool-calls.ts#L138) | `let aborted: boolean = signal.aborted` |
| `TurnEndReasonMap` | `interface` | [`packages/core/session/src/types.ts:155`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L155) | `export interface TurnEndReasonMap {` |
| `completed` | `let` | [`packages/e2b/fs-e2b/src/index.ts:258`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L258) | `let completed = false` |
| `completed` | `let` | [`packages/e2b/fs-e2b/src/index.ts:305`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L305) | `let completed = false` |
| `rejected` | `function` | [`packages/feedback/message-feedback/src/index.ts:96`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/message-feedback/src/index.ts#L96) | `function rejected<E extends MessageFeedbackFailure>(error: E): MessageFeedbackRejected<E> {` |
| `rejected` | `let` | [`packages/host/apiproxy/src/api-proxy.ts:1768`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1768) | `let rejected = false` |
| `rejected` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1985`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1985) | `const rejected = (error: unknown): RpcResponse<SettingsNamespaceView> => {` |
| `disposed` | `let` | [`packages/llm/llm/src/index.ts:433`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L433) | `let disposed = false` |
| `completed` | `let` | [`packages/llm/llm/src/index.ts:872`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L872) | `let completed = false` |
| `disposed` | `let` | [`packages/mcp/mcp-client/src/connection.ts:137`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/connection.ts#L137) | `let disposed = false` |
| `disposed` | `let` | [`packages/plan/plan-mode/src/index.ts:200`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/index.ts#L200) | `let disposed = false` |

### Tests and executable evidence

- [`python/sdk/tests/test_client.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_client.py) — A test under the owning area exercises or imports `max-tokens`.
- [`packages/core/session/tests/fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/fork.spec.ts) — A test under the owning area exercises or imports `max-tokens`.
- [`.github/issue-management/policy.test.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/issue-management/policy.test.mjs) — A test under the owning area exercises or imports `Completed`.
- [`packages/jobs/jobs-local/tests/jobs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs-local/tests/jobs.spec.ts) — A test under the owning area exercises or imports `max-tokens`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `max-tokens`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `max-tokens`.
- [`apps/web/tests/max-tokens-notice.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/max-tokens-notice.snapshot.ts) — A test under the owning area exercises or imports `max-tokens`.
- [`packages/core/agent-loop/tests/mock-adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/mock-adapter.ts) — A test under the owning area exercises or imports `max-tokens`.

## How to read the implementation

1. Start with [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) because it has the strongest evidence link to the note.
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
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/lifecycle`, `domain/agent-loop`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/projection`, `mechanism/registry`
- Aliases: `completed`, `disposed`, `rejected`, `interrupted`, `aborted`, `TurnEndReasonMap`, `max-tokens`, `turn/end`, `packages/ui/tui/src/index.ts`, `Turn stopped: the agent was disposed.`, `Turn ended: <kind>.`, `agent/disposed`, `Agent "<id>" was disposed.`, `errors-and-help`
- Regex: `(?i)(completed|disposed|rejected|interrupted|aborted|TurnEndReasonMap|max\-tokens|turn/end)`

```bash
rg -n --pcre2 "(?i)(completed|disposed|rejected|interrupted|aborted|TurnEndReasonMap|max\\-tokens|turn/end)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0353. Latch wake-ups that land in the cancel-convergence window](0353-latch-wake-ups-that-land-in-the-cancel-convergence-window.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0342. Load sessions from the pre-react-loop format](0342-load-sessions-from-the-pre-react-loop-format.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0025. Every LLM request is reconstructable from the session log](0025-every-llm-request-is-reconstructable-from-the-session-log.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0591. Code Mode sub-calls in the trajectory and waterfall views](0591-code-mode-sub-calls-in-the-trajectory-and-waterfall-views.md): Shares source implementation: `packages/plan/plan-mode/src/index.ts`, `packages/shell/bash-local/src/index.ts`.
- **`shares-code-with`** — [0014. Branded IDs everywhere they belong](0014-branded-ids-everywhere-they-belong.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0364. Owned-run finish reason reporting](0364-owned-run-finish-reason-reporting.md): Shares source implementation: `apps/cli/src/process-shutdown.ts`, `packages/e2b/fs-e2b/src/index.ts`.
- **`shares-code-with`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/llm/llm/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0616-tui-presents-a-reason-for-every-turn-end-kind.md`.
