---
id: "dsh-note-0248"
title: "Web turn run time and hover-revealed time chrome"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-03-web-turn-run-time.md"
implementation_evidence: "medium"
target_anchor: "append-only event and session store"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/session-state"
  - "lifecycle/implemented"
aliases:
  - "TurnStatus"
  - "turnTimings"
  - "turn/start"
  - "turn/end"
  - "endTime - startTime"
  - "data-time-hover-root"
  - "MessageIconActions.module.css"
  - ":focus-within"
  - "Web turn run time and hover-revealed time chrome"
  - "feature"
  - "boundary"
  - "evidence"
  - "human control"
  - "lifecycle"
search_regex: "(?i)(TurnStatus|turnTimings|turn/start|turn/end|endTime[- ]\\-[- ]startTime|data\\-time\\-hover\\-root|MessageIconActions\\.module\\.css|:focus\\-within)"
---

# 0248. Web turn run time and hover-revealed time chrome — implementation context

## Open this when

The Web chat shows when a message arrived but not how long the agent worked on it. Long turns give no live progress signal beyond the static activity label, and after the turn settles the wall time is not recoverable from the UI. Meanwhile the always-visible clock row adds visual noise to every message.

## Source decision

Turn wall time uses the existing logged turn/start and turn/end timestamps, with no new session events. The client Session folds each in-window pair into turnTimings; the actions-owning assistant footer renders endTime - startTime as a localized Ran for {duration} label after the turn ends. The running TurnStatus clock uses the latest timing without an end, so reload preserves elapsed time, steering does not reset it, and a retry starts from its own logged boundary. Both readings use the same localized formatter and whole-second floor.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-03-web-turn-run-time.md](../02-notes/implemented/feature/2026-08-03-web-turn-run-time.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-03-web-turn-run-time.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-03-web-turn-run-time.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-conversation/src/client/chat/ChatView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx) | runtime implementation | Defines `TurnStatus`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts) | runtime implementation | Defines `turnTimings`, a construct named by the note. | `symbol-definition` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `turn/start` named by the note. Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `turn/start` named by the note. Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `turn/start` named by the note. Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Contains the exact code literal `turn/start` named by the note. Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.zh.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.zh.md) | package contract and examples | Contains the exact code literal `turn/start` named by the note. | `exact-code-occurrence` |
| [`docs/persistence-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/persistence-catalog.md) | package contract and examples | Contains the exact code literal `turn/start` named by the note. Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/persistence-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/persistence-catalog.zh.md) | package contract and examples | Contains the exact code literal `turn/start` named by the note. Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-persistence-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-persistence-catalog.ts) | repository automation | Contains the exact code literal `turn/start` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `TurnStatus` | `function` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:107`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L107) | `function TurnStatus({ startTime, t }: {` |
| `turnTimings` | `const` | [`packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts:317`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts#L317) | `const turnTimings = new Map<number, { startTime: number; endTime?: number }>()` |

### Tests and executable evidence

- [`scripts/client-bundle-css.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-css.spec.ts) — A test under the owning area exercises or imports `css`.
- [`scripts/publication-payload.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publication-payload.spec.ts) — A test under the owning area exercises or imports `css`.
- [`scripts/client-bundle-purity.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-purity.spec.ts) — A test under the owning area exercises or imports `css`.
- [`apps/web/tests/produced-files.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/produced-files.e2e.ts) — A test under the owning area exercises or imports `css`.
- [`apps/web/tests/sidebar-scrollbar.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/sidebar-scrollbar.e2e.ts) — A test under the owning area exercises or imports `css`.
- [`apps/web/tests/composer-draft-scroll.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/composer-draft-scroll.e2e.ts) — A test under the owning area exercises or imports `css`.
- [`apps/web/tests/composer-tab-geometry.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/composer-tab-geometry.e2e.ts) — A test under the owning area exercises or imports `css`.
- [`apps/web/tests/produced-file-mentions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/produced-file-mentions.e2e.ts) — A test under the owning area exercises or imports `css`.

## How to read the implementation

1. Start with [`packages/client/ui-conversation/src/client/chat/ChatView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** append-only event and session store.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/agent-loop`, `domain/build-release`, `domain/session-state`, `lifecycle/implemented`
- Aliases: `TurnStatus`, `turnTimings`, `turn/start`, `turn/end`, `endTime - startTime`, `data-time-hover-root`, `MessageIconActions.module.css`, `:focus-within`, `Web turn run time and hover-revealed time chrome`, `feature`, `boundary`, `evidence`, `human control`, `lifecycle`
- Regex: `(?i)(TurnStatus|turnTimings|turn/start|turn/end|endTime[- ]\-[- ]startTime|data\-time\-hover\-root|MessageIconActions\.module\.css|:focus\-within)`

```bash
rg -n --pcre2 "(?i)(TurnStatus|turnTimings|turn/start|turn/end|endTime[- ]\\-[- ]startTime|data\\-time\\-hover\\-root|MessageIconActions\\.module\\.css|:focus\\-within)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0618. Question-composer option rows are scroll content, not the slack absorber](0618-question-composer-option-rows-are-scroll-content-not-the-slack-absorber.md): Shares source implementation: `apps/web/tests/produced-files.e2e.ts`, `apps/web/tests/sidebar-scrollbar.e2e.ts`.
- **`shares-code-with`** — [0351. Reader scroll attribution through the observed-top ledger](0351-reader-scroll-attribution-through-the-observed-top-ledger.md): Shares source implementation: `apps/web/tests/composer-draft-scroll.e2e.ts`, `apps/web/tests/sidebar-scrollbar.e2e.ts`.
- **`shares-code-with`** — [0113. Client Conversation business-node assembly and keyed Chat snapshots](0113-client-conversation-business-node-assembly-and-keyed-chat-snapshots.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/ChatView.tsx`, `packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts`.
- **`shares-code-with`** — [0336. Message fork actions require a completed turn tail](0336-message-fork-actions-require-a-completed-turn-tail.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/ChatView.tsx`, `packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts`.
- **`shares-code-with`** — [0344. Turn-tail IconActions require a completed turn](0344-turn-tail-iconactions-require-a-completed-turn.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/ChatView.tsx`, `packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts`.
- **`shares-code-with`** — [0327. The composer's two text layers share one scrollport](0327-the-composer-s-two-text-layers-share-one-scrollport.md): Shares source implementation: `apps/web/tests/composer-draft-scroll.e2e.ts`, `scripts/client-bundle-purity.spec.ts`.
- **`shares-code-with`** — [0340. The conversation column reserves one scrollbar gutter for every view](0340-the-conversation-column-reserves-one-scrollbar-gutter-for-every-view.md): Shares source implementation: `apps/web/tests/composer-tab-geometry.e2e.ts`, `packages/client/ui-conversation/src/client/chat/ChatView.tsx`.
- **`shares-code-with`** — [0368. The overlay composer seat compensates for the bar instead of reserving a gutter](0368-the-overlay-composer-seat-compensates-for-the-bar-instead-of-reserving-a.md): Shares source implementation: `apps/web/tests/composer-tab-geometry.e2e.ts`, `packages/client/ui-conversation/src/client/chat/ChatView.tsx`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0248-web-turn-run-time-and-hover-revealed-time-chrome.md`.
