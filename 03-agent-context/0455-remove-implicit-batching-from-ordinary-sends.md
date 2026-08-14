---
id: "dsh-note-0455"
title: "Remove implicit batching from ordinary sends"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-07-17-one-send-one-turn.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/simplification"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
aliases:
  - "inject"
  - "send"
  - "cancel"
  - "MessageId"
  - "Agent.send"
  - "turn/start"
  - "turn/end"
  - "agent/inbox/inserted { message }"
  - "Inbox.replace"
  - "Inbox.remove"
  - "agent/pre-step"
  - "user/message"
  - "whenIdle"
  - "Remove implicit batching from ordinary sends"
search_regex: "(?i)(inject|send|cancel|MessageId|Agent\\.send|turn/start|turn/end|agent/inbox/inserted[- ]\\{[- ]message[- ]\\})"
---

# 0455. Remove implicit batching from ordinary sends — implementation context

## Open this when

Suppose a caller submits message A and then message B with two Agent.send() calls. Implicit batching can put A and B in one turn simply because both are waiting when the driver reads its queue. The caller made two calls, but the loop silently turns them into one unit of work. That grouping depends on timing rather than caller intent. Calls from one synchronous stack, neighboring microtasks, event listeners, and model callbacks could be grouped differently even though every caller used the same API. This grouping changes behavior, not just the number of model calls.

## Source decision

Each successful send() creates one independent FIFO queue item. If that item runs, it is the only ordinary message in its turn. An item can be dropped before it starts, so the precise guarantee is at most one turn rather than exactly one; two sends are never silently combined. Before inserting a message, send() checks the agent state and accepts an already identified, deeply frozen value. The durable splice and agent/inbox/inserted { message } retain its MessageId; the pending message remains addressable through Inbox.replace() and Inbox.remove() until the driver claims or discards it.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-07-17-one-send-one-turn.md](../02-notes/implemented/simplification/2026-07-17-one-send-one-turn.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-07-17-one-send-one-turn.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-07-17-one-send-one-turn.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm/src/brand.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts) | runtime implementation | Defines `MessageId`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/registry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts) | runtime implementation | Defines `inject`, a construct named by the note. | `symbol-definition` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `inject`, a construct named by the note. | `symbol-definition` |
| [`scripts/smoke-python-runtime.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/smoke-python-runtime.py) | repository automation | Defines `send`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/invariant.ts) | runtime contract checks | Defines `inject`, a construct named by the note. | `symbol-definition` |
| [`packages/client/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts) | package entry point | Defines `inject`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts) | package entry point | Defines `inject`, a construct named by the note. | `symbol-definition` |
| [`packages/lsp/lsp-stdio/src/instance.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/instance.ts) | runtime implementation | Defines `send`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/pointer-grace.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/pointer-grace.ts) | runtime implementation | Defines `cancel`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/websocket-downlink.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/websocket-downlink.ts) | runtime implementation | Defines `send`, a construct named by the note. | `symbol-definition` |
| [`packages/host/directory-picker-native/src/win32-dialog-worker.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-native/src/win32-dialog-worker.ts) | runtime implementation | Defines `send`, a construct named by the note. | `symbol-definition` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `turn/start` named by the note. Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `inject` | `const` | [`packages/acp/acp/src/index.ts:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L44) | `export const inject = ['agents']` |
| `send` | `function` | [`packages/client/connection/src/websocket-downlink.ts:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/websocket-downlink.ts#L23) | `function send(socket: WebSocket, frame: RpcRequest<Frame>): Promise<void> {` |
| `inject` | `const` | [`packages/client/hmr/src/index.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts#L28) | `export const inject = ['clientModules', 'webServer']` |
| `cancel` | `const` | [`packages/client/ui-primitives/src/pointer-grace.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/pointer-grace.ts#L36) | `const cancel = useCallback(() => {` |
| `inject` | `const` | [`packages/fs/fs/src/invariant.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/invariant.ts#L12) | `export const inject = ['invariants']` |
| `send` | `const` | [`packages/host/directory-picker-native/src/win32-dialog-worker.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-native/src/win32-dialog-worker.ts#L28) | `const send = process.send.bind(process)` |
| `MessageId` | `type` | [`packages/llm/llm/src/brand.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts#L16) | `export type MessageId = Branded<'MessageId'>` |
| `MessageId` | `function` | [`packages/llm/llm/src/brand.ts:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts#L23) | `export function MessageId(id: string): MessageId {` |
| `send` | `const` | [`packages/lsp/lsp-stdio/src/instance.ts:206`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/instance.ts#L206) | `const send = this.connection.request(requestMethod(operation), params)` |
| `inject` | `const` | [`packages/sdk/server/src/index.ts:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts#L22) | `export const inject = ['agents']` |
| `send` | `def` | [`scripts/smoke-python-runtime.py:699`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/smoke-python-runtime.py#L699) | `def send(self, message: dict[str, object]) -> None:` |
| `inject` | `const` | [`vendor/cordis/src/registry.ts:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts#L46) | `const inject = (value[symbols.metadata] ??= {}).inject ??= Object.create(null)` |

### Tests and executable evidence

- [`apps/web/tests/steering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/steering.e2e.ts) — A test under the owning area exercises or imports `steer`.
- [`apps/web/tests/workflow-run.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/workflow-run.e2e.ts) — A test under the owning area exercises or imports `whenIdle`.
- [`apps/web/tests/schedule-after.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/schedule-after.e2e.ts) — A test under the owning area exercises or imports `whenIdle`.
- [`apps/web/tests/settings-chrome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/settings-chrome.e2e.ts) — A test under the owning area exercises or imports `steer`.
- [`packages/core/agent/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/agent.spec.ts) — A test under the owning area exercises or imports `steer`. A test under the owning area exercises or imports `whenIdle`.
- [`packages/e2b/e2b/tests/composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/tests/composition.e2e.ts) — A test under the owning area exercises or imports `steer`. A test under the owning area exercises or imports `whenIdle`.
- [`apps/web/tests/minimal-preset.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/minimal-preset.snapshot.ts) — A test under the owning area exercises or imports `whenIdle`.
- [`packages/llm/llm-retry/tests/retry.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/tests/retry.spec.ts) — A test under the owning area exercises or imports `whenIdle`.
- Source verification intent: Unit and property tests submit sends from the same stack, neighboring microtasks, different producers, and reentrant callbacks; every message gets its own FIFO-ordered turn. A built-stdio test submits two lines and observes two model requests and two turn boundaries. Delayed and rejected first-turn checkpoints keep the next turn waiting and prove that its request sees the preceding assistant result.

## How to read the implementation

1. Start with [`packages/llm/llm/src/brand.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/simplification`, `domain/agent-loop`, `domain/build-release`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`
- Aliases: `inject`, `send`, `cancel`, `MessageId`, `Agent.send`, `turn/start`, `turn/end`, `agent/inbox/inserted { message }`, `Inbox.replace`, `Inbox.remove`, `agent/pre-step`, `user/message`, `whenIdle`, `Remove implicit batching from ordinary sends`
- Regex: `(?i)(inject|send|cancel|MessageId|Agent\.send|turn/start|turn/end|agent/inbox/inserted[- ]\{[- ]message[- ]\})`

```bash
rg -n --pcre2 "(?i)(inject|send|cancel|MessageId|Agent\\.send|turn/start|turn/end|agent/inbox/inserted[- ]\\{[- ]message[- ]\\})" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0088. Claim inbox input before one pre-step decision](0088-claim-inbox-input-before-one-pre-step-decision.md): The source note links to this decision directly.
- **`shares-code-with`** — [0179. Web todo display --- snapshot side-effect channel + two render surfaces](0179-web-todo-display-snapshot-side-effect-channel-two-render-surfaces.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0656. Fold the stdio UI helper into the stdio app](0656-fold-the-stdio-ui-helper-into-the-stdio-app.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0058. Separate context injection from turn execution](0058-separate-context-injection-from-turn-execution.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/fs/fs/src/invariant.ts`.
- **`shares-code-with`** — [0573. TUI banner brand gradient](0573-tui-banner-brand-gradient.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0103. Slot declaration injection and reload lifetimes](0103-slot-declaration-injection-and-reload-lifetimes.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0200. Continuable subagents](0200-continuable-subagents.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/llm/llm/src/brand.ts`.
- **`shares-code-with`** — [0084. Follow-up enqueue and owned run boundaries](0084-follow-up-enqueue-and-owned-run-boundaries.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/llm/llm/src/brand.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0455-remove-implicit-batching-from-ordinary-sends.md`.
