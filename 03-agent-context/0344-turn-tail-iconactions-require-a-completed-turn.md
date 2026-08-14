---
id: "dsh-note-0344"
title: "Turn-tail IconActions require a completed turn"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-05-turn-tail-actions-require-a-completed-turn.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/evidence"
  - "concern/lifecycle"
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
aliases:
  - "running"
  - "AssistantMarkdown"
  - "turnTimings"
  - "turnEnds"
  - "assistantActionsSeqs"
  - "ConversationSnapshot.turnEnds"
  - "turn/end"
  - "hasContentText"
  - "chat-flow.ts"
  - "apps/web/tests/turn-tail-actions.e2e.ts"
  - "Turn-tail IconActions require a completed turn"
  - "bug fix"
  - "evidence"
  - "lifecycle"
search_regex: "(?i)(running|AssistantMarkdown|turnTimings|turnEnds|assistantActionsSeqs|ConversationSnapshot\\.turnEnds|turn/end|hasContentText)"
---

# 0344. Turn-tail IconActions require a completed turn — implementation context

## Open this when

Assistant IconActions were derived from the finalized transcript alone: the last content-text assistant of each turn owned the row. That quantity is stable only after the turn closes. While a turn is still producing steps, the narration a model writes before a tool call is the last content assistant so far, so it took the row for as long as the tool ran and then lost it to the next step's text. Readers saw copy, branch, and a clock appear under an intermediate sentence, shift the flow by one 28px row, and disappear.

## Source decision

assistantActionsSeqs takes ConversationSnapshot.turnEnds and grants the row only within a turn that has a turn/end in the window. Ownership inside a completed turn is unchanged: its last content-text assistant. A turn still producing steps grants nothing, so its narration never mounts the row, and the seat appears once, under the settled answer, when the turn closes. This is the same completion fact the branch control and the run-time label already use, so the three parts of one row now agree.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-05-turn-tail-actions-require-a-completed-turn.md](../02-notes/implemented/bug-fix/2026-08-05-turn-tail-actions-require-a-completed-turn.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-05-turn-tail-actions-require-a-completed-turn.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-05-turn-tail-actions-require-a-completed-turn.md)
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
| [`packages/client/ui-conversation/src/client/chat/AssistantMarkdown.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/AssistantMarkdown.tsx) | runtime implementation | Defines `AssistantMarkdown`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts) | runtime implementation | Defines `turnEnds`, a construct named by the note. Defines `turnTimings`, a construct named by the note. | `symbol-definition` |
| [`tsconfig.host.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.host.json) | composition and configuration | Contains the exact code literal `apps/web/tests/turn-tail-actions.e2e.ts` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `running` | `const` | [`packages/client/connection/src/client/fixture.ts:1792`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1792) | `const running = summaryOf(id)?.running === true` |
| `AssistantMarkdown` | `const` | [`packages/client/ui-conversation/src/client/chat/AssistantMarkdown.tsx:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/AssistantMarkdown.tsx#L37) | `export const AssistantMarkdown = memo(function AssistantMarkdown({` |
| `running` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L156) | `const running = useSession(s => s.running)` |
| `turnTimings` | `const` | [`packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts:317`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts#L317) | `const turnTimings = new Map<number, { startTime: number; endTime?: number }>()` |
| `turnEnds` | `const` | [`packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts:318`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts#L318) | `const turnEnds = new Map<number, number>()` |
| `turnEnds` | `let` | [`packages/core/agent-loop/src/agent.ts:260`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L260) | `let turnEnds: TurnEndReason \| null = null` |
| `running` | `const` | [`packages/extensions/ui-cordis/src/client/CordisPanel.tsx:154`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/CordisPanel.tsx#L154) | `const running = all.filter(view => visiblePanelStatus(` |
| `running` | `const` | [`packages/shell/bash-local/src/index.ts:257`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts#L257) | `const running = this.ctx.subprocess.spawn(this.spawnSpec(spec, argv, this.config.maxOutputBytes, spec.signal))` |
| `running` | `const` | [`packages/shell/pwsh-local/src/index.ts:286`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/index.ts#L286) | `const running = this.ctx.subprocess.spawn(this.spawnSpec(spec, this.config.maxOutputBytes, spec.signal, argv))` |

### Tests and executable evidence

- [`apps/web/tests/turn-tail-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/turn-tail-actions.e2e.ts) — The source note names this file directly. A test under the owning area exercises or imports `hang`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `hang`.
- [`packages/acp/acp/tests/harness.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/harness.ts) — A test under the owning area exercises or imports `hang`.
- [`apps/web/tests/queue-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/queue-actions.e2e.ts) — A test under the owning area exercises or imports `hang`.
- [`packages/acp/acp/tests/turns.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/turns.spec.ts) — A test under the owning area exercises or imports `hang`.
- [`packages/acp/acp/tests/dispose.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/dispose.spec.ts) — A test under the owning area exercises or imports `hang`.
- [`apps/web/tests/replay-round-trip.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/replay-round-trip.e2e.ts) — A test under the owning area exercises or imports `hang`.
- [`apps/web/tests/live-interactions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/live-interactions.e2e.ts) — A test under the owning area exercises or imports `hang`.

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

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/agent-loop`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/tools`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `running`, `AssistantMarkdown`, `turnTimings`, `turnEnds`, `assistantActionsSeqs`, `ConversationSnapshot.turnEnds`, `turn/end`, `hasContentText`, `chat-flow.ts`, `apps/web/tests/turn-tail-actions.e2e.ts`, `Turn-tail IconActions require a completed turn`, `bug fix`, `evidence`, `lifecycle`
- Regex: `(?i)(running|AssistantMarkdown|turnTimings|turnEnds|assistantActionsSeqs|ConversationSnapshot\.turnEnds|turn/end|hasContentText)`

```bash
rg -n --pcre2 "(?i)(running|AssistantMarkdown|turnTimings|turnEnds|assistantActionsSeqs|ConversationSnapshot\\.turnEnds|turn/end|hasContentText)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0600. Web message IconActions and clocks](0600-web-message-iconactions-and-clocks.md): The source note links to this decision directly.
- **`source-link`** — [0336. Message fork actions require a completed turn tail](0336-message-fork-actions-require-a-completed-turn-tail.md): The source note links to this decision directly.
- **`shares-code-with`** — [0461. Collapse agent-loop events around the observable state machine](0461-collapse-agent-loop-events-around-the-observable-state-machine.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `packages/client/connection/src/client/fixture.ts`.
- **`shares-code-with`** — [0353. Latch wake-ups that land in the cancel-convergence window](0353-latch-wake-ups-that-land-in-the-cancel-convergence-window.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/shell/bash-local/src/index.ts`.
- **`shares-code-with`** — [0270. Steer the whole Web queue with an empty-draft Cmd/Ctrl+Enter](0270-steer-the-whole-web-queue-with-an-empty-draft-cmd-ctrl-enter.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/shell/bash-local/src/index.ts`.
- **`shares-code-with`** — [0242. pwsh tool bash parity](0242-pwsh-tool-bash-parity.md): Shares source implementation: `packages/shell/bash-local/src/index.ts`, `packages/shell/pwsh-local/src/index.ts`.
- **`shares-code-with`** — [0113. Client Conversation business-node assembly and keyed Chat snapshots](0113-client-conversation-business-node-assembly-and-keyed-chat-snapshots.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/ChatView.tsx`, `packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts`.
- **`shares-code-with`** — [0611. Keep the Code Mode result card complete](0611-keep-the-code-mode-result-card-complete.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `packages/client/connection/src/client/fixture.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0344-turn-tail-iconactions-require-a-completed-turn.md`.
