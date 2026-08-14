---
id: "dsh-note-0327"
title: "The composer's two text layers share one scrollport"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-31-composer-text-layers-share-one-scrollport.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "ConversationSession"
  - "[data-input-backdrop]"
  - "scrollTop"
  - "[data-input-scroll]"
  - "white-space: pre-wrap"
  - "overflow-y: auto"
  - "setSelectionRange"
  - "ArrowUp"
  - "transform: translateY"
  - "animation-timeline: scroll"
  - "scrollbar-gutter: stable"
  - "scrollbar-width: none"
  - ".card"
  - "overscroll-behavior: contain"
search_regex: "(?i)(ConversationSession|\\[data\\-input\\-backdrop\\]|scrollTop|\\[data\\-input\\-scroll\\]|white\\-space:[- ]pre\\-wrap|overflow\\-y:[- ]auto|setSelectionRange|ArrowUp)"
---

# 0327. The composer's two text layers share one scrollport — implementation context

## Open this when

The composer paints its text in two stacked layers (InputBar): the owns the value, the selection, and the caret but renders its own glyphs color: transparent, and every visible character is painted by the [data-input-backdrop] div beneath it, which also carries the claim-token highlight, the chips, and the ghost hint. That split is what makes chips and highlights possible at all --- a textarea cannot style a range of its own text. The draft box is capped at 14 lines, so past the cap something has to scroll. Two layers with two scroll offsets fail in two stages, and this change is the second one.

## Source decision

One scrolling box, holding both layers. [data-input-scroll] is the composer's only scrollport and carries the 14-line cap. Inside it, the auto-grow stack is as tall as the whole draft --- the hidden mirror div is in normal flow and no longer capped, so it sizes the stack to the full text --- and the backdrop and textarea ride that height absolutely. The textarea is overflow: hidden with no scrollable overflow of its own; it can no longer hold an offset at all. The browser then applies one offset to both layers, in the same frame, on the same compositor.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-31-composer-text-layers-share-one-scrollport.md](../02-notes/implemented/bug-fix/2026-07-31-composer-text-layers-share-one-scrollport.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-31-composer-text-layers-share-one-scrollport.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-31-composer-text-layers-share-one-scrollport.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx) | runtime implementation | Defines `ConversationSession`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ConversationSession` | `function` | [`packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx:138`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx#L138) | `export function ConversationSession({` |

### Tests and executable evidence

- [`apps/web/tests/composer-draft-scroll.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/composer-draft-scroll.e2e.ts) — The source note names this file directly. A test under the owning area exercises or imports `scrollTop`.
- [`packages/client/ui-conversation/tests/input-bar.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-bar.client.spec.tsx) — The source note names this file directly. A test under the owning area exercises or imports `scrollTop`.
- [`scripts/publint-all.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.spec.ts) — A test under the owning area exercises or imports `tsx`.
- [`scripts/coverage-exempt.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/coverage-exempt.spec.ts) — A test under the owning area exercises or imports `tsx`.
- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — A test under the owning area exercises or imports `tsx`.
- [`apps/web/tests/workflow-run.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/workflow-run.e2e.ts) — A test under the owning area exercises or imports `focus`.
- [`scripts/client-bundle-purity.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-purity.spec.ts) — A test under the owning area exercises or imports `tsx`.
- [`apps/web/tests/chat-scroll-fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/chat-scroll-fixture.ts) — A test under the owning area exercises or imports `scroll`.
- Source verification intent: The unit spec in input-bar.spec.tsx asserts what jsdom can see: that one scrolling box contains both the textarea and the backdrop, that the backdrop's text is now the draft and nothing else, and that a late persisted draft reveals its caret without taking focus from another control. jsdom reports scrollHeight === clientHeight for every element and never scrolls one, so the geometry belongs to the browser scenario; the wheel-chaining cases stub the scrollport's metrics rather than the textarea's.

## How to read the implementation

1. Start with [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx) because it has the strongest evidence link to the note.
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

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/projection`
- Aliases: `ConversationSession`, `[data-input-backdrop]`, `scrollTop`, `[data-input-scroll]`, `white-space: pre-wrap`, `overflow-y: auto`, `setSelectionRange`, `ArrowUp`, `transform: translateY`, `animation-timeline: scroll`, `scrollbar-gutter: stable`, `scrollbar-width: none`, `.card`, `overscroll-behavior: contain`
- Regex: `(?i)(ConversationSession|\[data\-input\-backdrop\]|scrollTop|\[data\-input\-scroll\]|white\-space:[- ]pre\-wrap|overflow\-y:[- ]auto|setSelectionRange|ArrowUp)`

```bash
rg -n --pcre2 "(?i)(ConversationSession|\\[data\\-input\\-backdrop\\]|scrollTop|\\[data\\-input\\-scroll\\]|white\\-space:[- ]pre\\-wrap|overflow\\-y:[- ]auto|setSelectionRange|ArrowUp)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0320. The approval takeover shares the composer's text cap](0320-the-approval-takeover-shares-the-composer-s-text-cap.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/InputBar.tsx`, `scripts/coverage-exempt.spec.ts`.
- **`shares-code-with`** — [0499. Keep supported-platform tests semantic](0499-keep-supported-platform-tests-semantic.md): Shares source implementation: `scripts/install-lefthook.spec.ts`, `scripts/publint-all.spec.ts`.
- **`shares-code-with`** — [0622. Web conversation UI polish sweep](0622-web-conversation-ui-polish-sweep.md): Shares source implementation: `scripts/coverage-exempt.spec.ts`, `scripts/publint-all.spec.ts`.
- **`shares-code-with`** — [0351. Reader scroll attribution through the observed-top ledger](0351-reader-scroll-attribution-through-the-observed-top-ledger.md): Shares source implementation: `apps/web/tests/chat-scroll-fixture.ts`, `apps/web/tests/composer-draft-scroll.e2e.ts`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `scripts/install-lefthook.spec.ts`.
- **`shares-code-with`** — [0598. Address pending queue occurrences for edit and removal](0598-address-pending-queue-occurrences-for-edit-and-removal.md): Shares source implementation: `apps/web/tests/workflow-run.e2e.ts`.
- **`shares-code-with`** — [0248. Web turn run time and hover-revealed time chrome](0248-web-turn-run-time-and-hover-revealed-time-chrome.md): Shares source implementation: `apps/web/tests/composer-draft-scroll.e2e.ts`, `scripts/client-bundle-purity.spec.ts`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0327-the-composer-s-two-text-layers-share-one-scrollport.md`.
