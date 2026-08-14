---
id: "dsh-note-0486"
title: "Remove the steering interjection caption"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-08-10-web-remove-steering-interjection-caption.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/simplification"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
aliases:
  - "UserMessageNode"
  - "SteeringMessageNode"
  - "UserStyleBubble"
  - "PendingSteeringBubble"
  - "UserMessageNodeView"
  - "message.steering"
  - ".steeringMark"
  - "agent/inbox/spliced"
  - "data-pending-steering"
  - "packages/client/ui-conversation"
  - "steering/mid-steer"
  - "steering/settled"
  - "plan-review/approved"
  - "Remove the steering interjection caption"
search_regex: "(?i)(UserMessageNode|SteeringMessageNode|UserStyleBubble|PendingSteeringBubble|UserMessageNodeView|message\\.steering|\\.steeringMark|agent/inbox/spliced)"
---

# 0486. Remove the steering interjection caption — implementation context

## Open this when

The context-source and steer marks decision captioned every durable and pending steering bubble with 插话 / Interjection so the transcript could say which right-aligned bubble interrupted a running turn. The caption repeats what the flow already shows: a steering bubble sits mid-turn, between the assistant content it interrupted, while a turn-opening prompt sits at a turn boundary.

## Source decision

Steering renders exactly as a user bubble. UserStyleBubble has no steering flag, the message.steering locale key and the .steeringMark style are deleted, and PendingSteeringBubble and UserMessageNodeView pass only content and actions. A mid-turn steer is recognizable by its position inside the running turn's flow, and by nothing else. The runtime distinction is untouched.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-08-10-web-remove-steering-interjection-caption.md](../02-notes/implemented/simplification/2026-08-10-web-remove-steering-interjection-caption.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-08-10-web-remove-steering-interjection-caption.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-08-10-web-remove-steering-interjection-caption.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-conversation/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/client/ui-conversation`. | `named-directory-member` |
| [`packages/client/ui-conversation/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/client/ui-conversation`. | `named-directory-member` |
| [`packages/client/ui-conversation/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/client/ui-conversation`. Contains the exact code literal `packages/client/ui-conversation` named by the note. | `exact-code-occurrence, named-directory-member` |
| [`packages/client/ui-conversation/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/client/ui-conversation`. | `named-directory-member` |
| [`packages/client/ui-conversation/src/client/chat/MessageItem.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/MessageItem.tsx) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client/ui-conversation`. Defines `UserStyleBubble`, a construct named by the note. | `named-directory-member, symbol-definition` |
| [`packages/client/ui-conversation`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/client/runtime/src/client/sessions/conversation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts) | runtime implementation | Defines `SteeringMessageNode`, a construct named by the note. Defines `UserMessageNode`, a construct named by the note. | `symbol-definition` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `packages/client/ui-conversation` named by the note. | `exact-code-occurrence` |
| [`tsconfig.client.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.client.json) | composition and configuration | Contains the exact code literal `packages/client/ui-conversation` named by the note. | `exact-code-occurrence` |
| [`docs/module-graph.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/module-graph.md) | package contract and examples | Contains the exact code literal `packages/client/ui-conversation` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `agent/inbox/spliced` named by the note. | `exact-code-occurrence` |
| [`docs/module-graph.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/module-graph.zh.md) | package contract and examples | Contains the exact code literal `packages/client/ui-conversation` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `UserMessageNode` | `interface` | [`packages/client/runtime/src/client/sessions/conversation.ts:76`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts#L76) | `export interface UserMessageNode {` |
| `SteeringMessageNode` | `interface` | [`packages/client/runtime/src/client/sessions/conversation.ts:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts#L121) | `export interface SteeringMessageNode {` |
| `UserStyleBubble` | `function` | [`packages/client/ui-conversation/src/client/chat/MessageItem.tsx:179`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/MessageItem.tsx#L179) | `function UserStyleBubble({` |
| `PendingSteeringBubble` | `function` | [`packages/client/ui-conversation/src/client/chat/MessageItem.tsx:213`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/MessageItem.tsx#L213) | `export function PendingSteeringBubble({ content, loadImage, t }: {` |
| `UserMessageNodeView` | `const` | [`packages/client/ui-conversation/src/client/chat/MessageItem.tsx:238`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/MessageItem.tsx#L238) | `export const UserMessageNodeView = memo(function UserMessageNodeView({` |

### Tests and executable evidence

- [`apps/web/tests/steering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/steering.e2e.ts) — A test under the owning area exercises or imports `Interjection`. A test under the owning area exercises or imports `steering`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `steering`.
- [`packages/core/agent-loop/tests/resume.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/resume.spec.ts) — A test under the owning area exercises or imports `steering`.
- [`packages/core/agent-loop/tests/cancel.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/cancel.spec.ts) — A test under the owning area exercises or imports `steering`.
- [`apps/web/tests/snapshots/steering/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/steering/session.jsonl) — A test under the owning area exercises or imports `Interjection`.
- [`packages/core/agent-loop/tests/interception.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/interception.spec.ts) — A test under the owning area exercises or imports `steering`.
- [`apps/web/tests/snapshots/steering/settled.expected.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/steering/settled.expected.md) — A test under the owning area exercises or imports `Interjection`.
- [`apps/web/tests/snapshots/steer-all/settled.expected.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/steer-all/settled.expected.md) — A test under the owning area exercises or imports `Interjection`.
- Source verification intent: packages/client/ui-conversation jsdom coverage pins the plain bubble: the pending hand-off test locates pending bubbles by data-pending-steering and asserts the single-bubble hand-off without any caption, and the MessageItem steering arm asserts copy-without-branch on an uncaptioned bubble. The keyless assembled-Web goldens (steering/mid-steer, steering/settled, plan-review/approved) replay the unchanged session fixtures with no caption text.

## How to read the implementation

1. Start with [`packages/client/ui-conversation/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/README.md) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/simplification`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/projection`
- Aliases: `UserMessageNode`, `SteeringMessageNode`, `UserStyleBubble`, `PendingSteeringBubble`, `UserMessageNodeView`, `message.steering`, `.steeringMark`, `agent/inbox/spliced`, `data-pending-steering`, `packages/client/ui-conversation`, `steering/mid-steer`, `steering/settled`, `plan-review/approved`, `Remove the steering interjection caption`
- Regex: `(?i)(UserMessageNode|SteeringMessageNode|UserStyleBubble|PendingSteeringBubble|UserMessageNodeView|message\.steering|\.steeringMark|agent/inbox/spliced)`

```bash
rg -n --pcre2 "(?i)(UserMessageNode|SteeringMessageNode|UserStyleBubble|PendingSteeringBubble|UserMessageNodeView|message\\.steering|\\.steeringMark|agent/inbox/spliced)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0675. Web UI drops steer entry and interjection chrome](0675-web-ui-drops-steer-entry-and-interjection-chrome.md): The source note links to this decision directly.
- **`source-link`** — [0252. Web transcript marks context source, recall, and steering](0252-web-transcript-marks-context-source-recall-and-steering.md): The source note links to this decision directly.
- **`shares-code-with`** — [0330. Fork anchor floors to an event seq](0330-fork-anchor-floors-to-an-event-seq.md): Shares source implementation: `packages/client/ui-conversation`, `packages/client/ui-conversation/README.md`.
- **`shares-code-with`** — [0253. Web turn and window latency/throughput metrics](0253-web-turn-and-window-latency-throughput-metrics.md): Shares source implementation: `packages/client/ui-conversation`, `packages/client/ui-conversation/README.md`.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0076. Projected token usage and context occupancy](0076-projected-token-usage-and-context-occupancy.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0292. Web surface for message feedback](0292-web-surface-for-message-feedback.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0177. Safe assistant Markdown in the Web conversation](0177-safe-assistant-markdown-in-the-web-conversation.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0486-remove-the-steering-interjection-caption.md`.
