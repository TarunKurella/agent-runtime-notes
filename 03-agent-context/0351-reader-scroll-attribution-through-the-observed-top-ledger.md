---
id: "dsh-note-0351"
title: "Reader scroll attribution through the observed-top ledger"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-06-reader-scroll-attribution-observed-top-ledger.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/streaming"
aliases:
  - "FOLLOW_THRESHOLD"
  - "observedTopRef"
  - "scrollTop"
  - "packages/client/ui-conversation/tests/chat-view.client.spec.tsx"
  - "readerScroll"
  - "apps/web/tests/chat-scroll-contract.e2e.ts"
  - "Input.synthesizeScrollGesture"
  - "synthesizeScrollGesture"
  - "Input.dispatchTouchEvent"
  - "dispatchTouchEvent"
  - "--hide-scrollbars"
  - "Reader scroll attribution through the observed-top ledger"
  - "bug fix"
  - "boundary"
search_regex: "(?i)(FOLLOW_THRESHOLD|observedTopRef|scrollTop|readerScroll|apps/web/tests/chat\\-scroll\\-contract\\.e2e\\.ts|Input\\.synthesizeScrollGesture|synthesizeScrollGesture|Input\\.dispatchTouchEvent)"
---

# 0351. Reader scroll attribution through the observed-top ledger — implementation context

## Open this when

ChatView's bottom-follow recognized only wheel/trackpad gestures as reader input: while pinned to the floor, a scroll event without matching wheel movement was treated as programmatic and snapped back. Touch panning, native-scrollbar dragging, and keyboard paging therefore could not leave the bottom of a streaming transcript --- on a phone the tail was effectively locked. That wheel-only input classification was a deliberate deferral in the sticky-composer note, which rejected a general input state machine "for this narrow fix" and left every other scroll source outside the model.

## Source decision

Reader input is no longer identified by device. ChatView keeps an observed-top ledger (observedTopRef): the last scrollTop either delivered on the main thread or written by the component, recorded synchronously at every programmatic write site --- bottom follow, open restore, prepend anchoring, resize follow, and scroll delivery itself. When a scroll event arrives, a position that deviates from min(ledger, floor) by more than half a pixel is reader input; a position on the ledger (a delayed programmatic delivery) or exactly on the shrunken floor (a browser clamp after content shrank) preserves the current.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-06-reader-scroll-attribution-observed-top-ledger.md](../02-notes/implemented/bug-fix/2026-08-06-reader-scroll-attribution-observed-top-ledger.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-06-reader-scroll-attribution-observed-top-ledger.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-06-reader-scroll-attribution-observed-top-ledger.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-conversation/src/client/chat/ChatView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx) | runtime implementation | Defines `observedTopRef`, a construct named by the note. Defines `FOLLOW_THRESHOLD`, a construct named by the note. | `symbol-definition` |
| [`tsconfig.host.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.host.json) | composition and configuration | Contains the exact code literal `apps/web/tests/chat-scroll-contract.e2e.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `FOLLOW_THRESHOLD` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L24) | `const FOLLOW_THRESHOLD = 24` |
| `observedTopRef` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:174`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L174) | `const observedTopRef = useRef(0)` |

### Tests and executable evidence

- [`apps/web/tests/chat-scroll-contract.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/chat-scroll-contract.e2e.ts) — The source note names this file directly. A test under the owning area exercises or imports `scrollTop`.
- [`packages/client/ui-conversation/tests/chat-view.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-view.client.spec.tsx) — The source note names this file directly. A test under the owning area exercises or imports `scrollTop`.
- [`apps/web/tests/chat-scroll-fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/chat-scroll-fixture.ts) — A test under the owning area exercises or imports `scroll`.
- [`apps/web/tests/web-search-round.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/web-search-round.e2e.ts) — A test under the owning area exercises or imports `scroll`.
- [`apps/web/tests/sidebar-scrollbar.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/sidebar-scrollbar.e2e.ts) — A test under the owning area exercises or imports `scrollTop`. A test under the owning area exercises or imports `scroll`.
- [`apps/web/tests/approval-composer.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/approval-composer.e2e.ts) — A test under the owning area exercises or imports `min`. A test under the owning area exercises or imports `scroll`.
- [`apps/web/tests/question-composer.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/question-composer.e2e.ts) — A test under the owning area exercises or imports `scroll`.
- [`apps/web/tests/stats-paged-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/stats-paged-history.e2e.ts) — A test under the owning area exercises or imports `scroll`.
- Source verification intent: Unit specs in packages/client/ui-conversation/tests/chat-view.client.spec.tsx pin the ledger contract directly: a readerScroll helper delivers a position the component never wrote, programmatic deliveries land on the ledger, and the stream-finalization shrink clamp keeps following. Two scenarios in apps/web/tests/chat-scroll-contract.e2e.ts extend the browser e2e lane: keyboard paging over a settled transcript and a touch-style momentum fling against paced streaming, both red under the wheel-only implementation and green under the ledger.

## How to read the implementation

1. Start with [`packages/client/ui-conversation/src/client/chat/ChatView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/streaming`
- Aliases: `FOLLOW_THRESHOLD`, `observedTopRef`, `scrollTop`, `packages/client/ui-conversation/tests/chat-view.client.spec.tsx`, `readerScroll`, `apps/web/tests/chat-scroll-contract.e2e.ts`, `Input.synthesizeScrollGesture`, `synthesizeScrollGesture`, `Input.dispatchTouchEvent`, `dispatchTouchEvent`, `--hide-scrollbars`, `Reader scroll attribution through the observed-top ledger`, `bug fix`, `boundary`
- Regex: `(?i)(FOLLOW_THRESHOLD|observedTopRef|scrollTop|readerScroll|apps/web/tests/chat\-scroll\-contract\.e2e\.ts|Input\.synthesizeScrollGesture|synthesizeScrollGesture|Input\.dispatchTouchEvent)`

```bash
rg -n --pcre2 "(?i)(FOLLOW_THRESHOLD|observedTopRef|scrollTop|readerScroll|apps/web/tests/chat\\-scroll\\-contract\\.e2e\\.ts|Input\\.synthesizeScrollGesture|synthesizeScrollGesture|Input\\.dispatchTouchEvent)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0318. Fixed header, sticky composer inside the transcript scrollport](0318-fixed-header-sticky-composer-inside-the-transcript-scrollport.md): The source note links to this decision directly.
- **`source-link`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): The source note links to this decision directly.
- **`shares-code-with`** — [0618. Question-composer option rows are scroll content, not the slack absorber](0618-question-composer-option-rows-are-scroll-content-not-the-slack-absorber.md): Shares source implementation: `apps/web/tests/approval-composer.e2e.ts`, `apps/web/tests/chat-scroll-contract.e2e.ts`.
- **`shares-code-with`** — [0248. Web turn run time and hover-revealed time chrome](0248-web-turn-run-time-and-hover-revealed-time-chrome.md): Shares source implementation: `apps/web/tests/composer-draft-scroll.e2e.ts`, `apps/web/tests/sidebar-scrollbar.e2e.ts`.
- **`shares-code-with`** — [0327. The composer's two text layers share one scrollport](0327-the-composer-s-two-text-layers-share-one-scrollport.md): Shares source implementation: `apps/web/tests/chat-scroll-fixture.ts`, `apps/web/tests/composer-draft-scroll.e2e.ts`.
- **`shares-code-with`** — [0113. Client Conversation business-node assembly and keyed Chat snapshots](0113-client-conversation-business-node-assembly-and-keyed-chat-snapshots.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/ChatView.tsx`.
- **`shares-code-with`** — [0368. The overlay composer seat compensates for the bar instead of reserving a gutter](0368-the-overlay-composer-seat-compensates-for-the-bar-instead-of-reserving-a.md): Shares source implementation: `apps/web/tests/web-search-round.e2e.ts`, `packages/client/ui-conversation/src/client/chat/ChatView.tsx`.
- **`shares-code-with`** — [0247. Web search source card scrolls instead of collapsing](0247-web-search-source-card-scrolls-instead-of-collapsing.md): Shares source implementation: `apps/web/tests/web-search-round.e2e.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0351-reader-scroll-attribution-through-the-observed-top-ledger.md`.
