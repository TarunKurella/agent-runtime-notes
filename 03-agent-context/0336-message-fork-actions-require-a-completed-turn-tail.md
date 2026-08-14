---
id: "dsh-note-0336"
title: "Message fork actions require a completed turn tail"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-02-message-fork-actions-require-completed-turn-tail.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/agent-loop"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "running"
  - "turnEnds"
  - "turn/end"
  - "ConversationSnapshot.turnEnds"
  - "aria-disabled"
  - "aria-describedby"
  - "Message fork actions require a completed turn tail"
  - "bug fix"
  - "boundary"
  - "evidence"
  - "human control"
  - "ownership"
  - "recovery"
  - "agent loop"
search_regex: "(?i)(running|turnEnds|turn/end|ConversationSnapshot\\.turnEnds|aria\\-disabled|aria\\-describedby|Message[- ]fork[- ]actions[- ]require[- ]a[- ]completed[- ]turn[- ]tail|bug[- ]fix)"
---

# 0336. Message fork actions require a completed turn tail — implementation context

## Open this when

The Web conversation attached branch to the last assistant node with nonempty text in each turn. A later tool result, interrupted reasoning node, or terminal error did not take ownership because those rows have no content-text IconActions. The branch icon could therefore appear beneath an assistant response while more rows from the same turn remained below it. The Host correctly expanded that message anchor through the containing turn/end, but the placement made the action look like a message-level cut and the child visibly inherited the same-turn suffix.

## Source decision

ConversationSnapshot.turnEnds retains the completed turn boundaries present in the raw event window. The conversation view walks transcript nodes through each boundary and enables branch only when the boundary's last node is a user message, a durable steering message, or a content-bearing assistant message. Open turns have no eligible message, and a later tool result, reasoning-only interruption, turn error, or other transcript node leaves branch unavailable on earlier messages.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-02-message-fork-actions-require-completed-turn-tail.md](../02-notes/implemented/bug-fix/2026-08-02-message-fork-actions-require-completed-turn-tail.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-02-message-fork-actions-require-completed-turn-tail.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-02-message-fork-actions-require-completed-turn-tail.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/agent-loop/src/agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts) | runtime implementation | Defines `turnEnds`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/bash-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts) | package entry point | Defines `running`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/pwsh-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/index.ts) | package entry point | Defines `running`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `running`, a construct named by the note. | `symbol-definition` |
| [`packages/extensions/ui-cordis/src/client/CordisPanel.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/CordisPanel.tsx) | runtime implementation | Defines `running`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/ChatView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx) | runtime implementation | Defines `running`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts) | runtime implementation | Defines `turnEnds`, a construct named by the note. | `symbol-definition` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `running` | `const` | [`packages/client/connection/src/client/fixture.ts:1792`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1792) | `const running = summaryOf(id)?.running === true` |
| `running` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L156) | `const running = useSession(s => s.running)` |
| `turnEnds` | `const` | [`packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts:318`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts#L318) | `const turnEnds = new Map<number, number>()` |
| `turnEnds` | `let` | [`packages/core/agent-loop/src/agent.ts:260`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L260) | `let turnEnds: TurnEndReason \| null = null` |
| `running` | `const` | [`packages/extensions/ui-cordis/src/client/CordisPanel.tsx:154`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/CordisPanel.tsx#L154) | `const running = all.filter(view => visiblePanelStatus(` |
| `running` | `const` | [`packages/shell/bash-local/src/index.ts:257`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts#L257) | `const running = this.ctx.subprocess.spawn(this.spawnSpec(spec, argv, this.config.maxOutputBytes, spec.signal))` |
| `running` | `const` | [`packages/shell/pwsh-local/src/index.ts:286`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/index.ts#L286) | `const running = this.ctx.subprocess.spawn(this.spawnSpec(spec, this.config.maxOutputBytes, spec.signal, argv))` |

### Tests and executable evidence

- [`apps/web/tests/message-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/message-actions.e2e.ts) — A test under the owning area exercises or imports `aria-disabled`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `turnEnds`.
- [`apps/web/tests/goal-multi-turn-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/goal-multi-turn-actions.e2e.ts) — A test under the owning area exercises or imports `aria-disabled`.
- [`packages/core/agent-loop/tests/contract-regressions.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/contract-regressions.spec.ts) — A test under the owning area exercises or imports `turnEnds`.
- [`packages/client/ui-conversation/tests/skeleton.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/skeleton.client.spec.tsx) — A test under the owning area exercises or imports `turnEnds`.
- [`packages/client/ui-conversation/tests/chat-view.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-view.client.spec.tsx) — A test under the owning area exercises or imports `turnEnds`. A test under the owning area exercises or imports `aria-disabled`.
- [`packages/client/ui-conversation/tests/input-bar.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-bar.client.spec.tsx) — A test under the owning area exercises or imports `turnEnds`.
- [`packages/client/ui-conversation/tests/queue-dock.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/queue-dock.client.spec.tsx) — A test under the owning area exercises or imports `turnEnds`.

## How to read the implementation

1. Start with [`packages/core/agent-loop/src/agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/recovery`, `domain/agent-loop`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/tools`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`
- Aliases: `running`, `turnEnds`, `turn/end`, `ConversationSnapshot.turnEnds`, `aria-disabled`, `aria-describedby`, `Message fork actions require a completed turn tail`, `bug fix`, `boundary`, `evidence`, `human control`, `ownership`, `recovery`, `agent loop`
- Regex: `(?i)(running|turnEnds|turn/end|ConversationSnapshot\.turnEnds|aria\-disabled|aria\-describedby|Message[- ]fork[- ]actions[- ]require[- ]a[- ]completed[- ]turn[- ]tail|bug[- ]fix)`

```bash
rg -n --pcre2 "(?i)(running|turnEnds|turn/end|ConversationSnapshot\\.turnEnds|aria\\-disabled|aria\\-describedby|Message[- ]fork[- ]actions[- ]require[- ]a[- ]completed[- ]turn[- ]tail|bug[- ]fix)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0196. Web session fork actions](0196-web-session-fork-actions.md): The source note links to this decision directly.
- **`source-link`** — [0477. User and steering bubbles drop the branch action](0477-user-and-steering-bubbles-drop-the-branch-action.md): The source note links to this decision directly.
- **`shares-code-with`** — [0344. Turn-tail IconActions require a completed turn](0344-turn-tail-iconactions-require-a-completed-turn.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/client/ui-conversation/src/client/chat/ChatView.tsx`.
- **`shares-code-with`** — [0461. Collapse agent-loop events around the observable state machine](0461-collapse-agent-loop-events-around-the-observable-state-machine.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/client/ui-conversation/src/client/chat/ChatView.tsx`.
- **`shares-code-with`** — [0353. Latch wake-ups that land in the cancel-convergence window](0353-latch-wake-ups-that-land-in-the-cancel-convergence-window.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/shell/bash-local/src/index.ts`.
- **`shares-code-with`** — [0270. Steer the whole Web queue with an empty-draft Cmd/Ctrl+Enter](0270-steer-the-whole-web-queue-with-an-empty-draft-cmd-ctrl-enter.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/shell/bash-local/src/index.ts`.
- **`shares-code-with`** — [0113. Client Conversation business-node assembly and keyed Chat snapshots](0113-client-conversation-business-node-assembly-and-keyed-chat-snapshots.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/ChatView.tsx`, `packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts`.
- **`shares-code-with`** — [0600. Web message IconActions and clocks](0600-web-message-iconactions-and-clocks.md): Shares source implementation: `apps/web/tests/message-actions.e2e.ts`, `packages/client/connection/src/client/fixture.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0336-message-fork-actions-require-a-completed-turn-tail.md`.
