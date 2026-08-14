---
id: "dsh-note-0340"
title: "The conversation column reserves one scrollbar gutter for every view"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-04-composer-tab-gutter-reservation.md"
implementation_evidence: "high"
target_anchor: "append-only event and session store"
tags:
  - "class/bug-fix"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/session-state"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
aliases:
  - "right"
  - "hidden"
  - "stable"
  - "[data-conversation-scroll]"
  - "data-conversation-composer-overlay"
  - ".scrollBody"
  - "scrollbar-gutter: stable"
  - "scrollbar-gutter: auto"
  - "overflow-x: hidden; overflow-y: auto"
  - "overflow-y: auto"
  - "scrollbar-gutter"
  - "::-webkit-scrollbar"
  - "scrollbar-width: thin"
  - "apps/web/tests/composer-tab-geometry.e2e.ts"
search_regex: "(?i)(right|hidden|stable|\\[data\\-conversation\\-scroll\\]|data\\-conversation\\-composer\\-overlay|\\.scrollBody|scrollbar\\-gutter:[- ]stable|scrollbar\\-gutter:[- ]auto)"
---

# 0340. The conversation column reserves one scrollbar gutter for every view — implementation context

## Open this when

The composer seat is one node in one place in the tree, and it was laid out against a different edge depending on which view tab was shown. In Chat it is a sticky CHILD of the column's scroller ([data-conversation-scroll]), so it rides that scroller's content box --- the box a space-consuming scrollbar shortens by the bar's width. A view that declares data-conversation-composer-overlay, which Trajectory does, moves the column's scrolling into the view itself: the branch keyed on that attribute left the scroller overflow: hidden and positioned the seat absolutely, against the padding box, which no scrollbar.

## Source decision

.scrollBody declares scrollbar-gutter: stable for the Chat state, and the overlay branch overrides it with scrollbar-gutter: auto while staying a scroll container on both axes --- overflow-x: hidden; overflow-y: auto. The reservation is Chat's alone: it holds the seat's content box at the same width whether or not the transcript overflows, so the card never jumps as a growing transcript starts to scroll, nor between the hero phase and the first scrolling turn.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-04-composer-tab-gutter-reservation.md](../02-notes/implemented/bug-fix/2026-08-04-composer-tab-gutter-reservation.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-04-composer-tab-gutter-reservation.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-04-composer-tab-gutter-reservation.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/settings/settings/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts) | package entry point | Defines `right`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/acp-snapshot/src/suite.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts) | runtime implementation | Defines `stable`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/DiffBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DiffBlock.tsx) | runtime implementation | Defines `hidden`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/ReadBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ReadBlock.tsx) | runtime implementation | Defines `hidden`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/head-tail-cap.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/head-tail-cap.ts) | runtime implementation | Defines `hidden`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-attachment/src/AttachmentRail.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/AttachmentRail.tsx) | runtime implementation | Defines `right`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-deliverables/src/client/ProducedFiles.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-deliverables/src/client/ProducedFiles.tsx) | runtime implementation | Defines `hidden`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/ChatView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx) | runtime implementation | Defines `right`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/ContextBody.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ContextBody.tsx) | runtime implementation | Defines `hidden`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/sessions/conversation-assembler.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation-assembler.ts) | runtime implementation | Defines `right`, a construct named by the note. | `symbol-definition` |
| [`tsconfig.host.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.host.json) | composition and configuration | Contains the exact code literal `apps/web/tests/composer-tab-geometry.e2e.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `right` | `const` | [`packages/client/runtime/src/client/sessions/conversation-assembler.ts:104`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation-assembler.ts#L104) | `const right = existing[current]` |
| `right` | `const` | [`packages/client/runtime/src/client/sessions/conversation-assembler.ts:647`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation-assembler.ts#L647) | `const right = sorted[added]` |
| `right` | `const` | [`packages/client/ui-attachment/src/AttachmentRail.tsx:85`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/AttachmentRail.tsx#L85) | `const right = el.scrollLeft < el.scrollWidth - el.clientWidth - 1` |
| `right` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:63`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L63) | `const right = Math.min(viewport.right, content.right)` |
| `hidden` | `const` | [`packages/client/ui-conversation/src/client/chat/ContextBody.tsx:94`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ContextBody.tsx#L94) | `const hidden = formRendered ? ['kind', 'form'] : ['kind']` |
| `hidden` | `const` | [`packages/client/ui-deliverables/src/client/ProducedFiles.tsx:114`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-deliverables/src/client/ProducedFiles.tsx#L114) | `const hidden = paths.length - shown.length` |
| `hidden` | `const` | [`packages/client/ui-primitives/src/DiffBlock.tsx:159`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DiffBlock.tsx#L159) | `const hidden = rows.length - maxLines` |
| `hidden` | `const` | [`packages/client/ui-primitives/src/ReadBlock.tsx:107`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ReadBlock.tsx#L107) | `const hidden = lines.length - maxLines` |
| `hidden` | `const` | [`packages/client/ui-primitives/src/head-tail-cap.ts:29`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/head-tail-cap.ts#L29) | `const hidden = total - maxLines` |
| `right` | `const` | [`packages/settings/settings/src/index.ts:153`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts#L153) | `const right = b as Record<string, unknown>` |
| `stable` | `let` | [`packages/test-support/acp-snapshot/src/suite.ts:658`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts#L658) | `let stable = content` |
| `stable` | `const` | [`packages/test-support/acp-snapshot/src/suite.ts:1033`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts#L1033) | `const stable = applyFixtureReplacements(fresh, replacements)` |

### Tests and executable evidence

- [`apps/web/tests/composer-tab-geometry.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/composer-tab-geometry.e2e.ts) — The source note names this file directly. A test under the owning area exercises or imports `data-conversation-composer-overlay`.
- [`scripts/run-oxlint.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-oxlint.spec.ts) — A test under the owning area exercises or imports `auto`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `auto`.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `auto`.
- [`apps/web/tests/web-search-round.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/web-search-round.e2e.ts) — A test under the owning area exercises or imports `auto`.
- [`apps/web/tests/navigation-panes.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/navigation-panes.e2e.ts) — A test under the owning area exercises or imports `auto`.
- [`apps/web/tests/sidebar-scrollbar.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/sidebar-scrollbar.e2e.ts) — A test under the owning area exercises or imports `auto`. A test under the owning area exercises or imports `scrollbar-gutter`.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `auto`.
- Source verification intent: apps/web/tests/composer-tab-geometry.e2e.ts measures the input card's rectangle in both tabs, at a viewport where the card sits at its width cap and one where it shrinks with the column, and asserts the two rectangles are the same rectangle. Only a real engine reports this: jsdom gives every element a zero-sized box and no scrollbar, so a unit spec could assert the declarations exist but not that the two states land in the same place. For the same reason no CSS-text spec accompanies it --- it would restate the declarations without adding a fact the browser lane does not already establish.

## How to read the implementation

1. Start with [`packages/settings/settings/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts) because it has the strongest evidence link to the note.
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

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/compatibility`, `concern/evidence`, `concern/ownership`, `concern/simplification`, `domain/build-release`, `domain/session-state`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`
- Aliases: `right`, `hidden`, `stable`, `[data-conversation-scroll]`, `data-conversation-composer-overlay`, `.scrollBody`, `scrollbar-gutter: stable`, `scrollbar-gutter: auto`, `overflow-x: hidden; overflow-y: auto`, `overflow-y: auto`, `scrollbar-gutter`, `::-webkit-scrollbar`, `scrollbar-width: thin`, `apps/web/tests/composer-tab-geometry.e2e.ts`
- Regex: `(?i)(right|hidden|stable|\[data\-conversation\-scroll\]|data\-conversation\-composer\-overlay|\.scrollBody|scrollbar\-gutter:[- ]stable|scrollbar\-gutter:[- ]auto)`

```bash
rg -n --pcre2 "(?i)(right|hidden|stable|\\[data\\-conversation\\-scroll\\]|data\\-conversation\\-composer\\-overlay|\\.scrollBody|scrollbar\\-gutter:[- ]stable|scrollbar\\-gutter:[- ]auto)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0312. The scrollbar tokens get their consumer, and the workspace list reserves its gutter](0312-the-scrollbar-tokens-get-their-consumer-and-the-workspace-list-reserves.md): The source note links to this decision directly.
- **`source-link`** — [0318. Fixed header, sticky composer inside the transcript scrollport](0318-fixed-header-sticky-composer-inside-the-transcript-scrollport.md): The source note links to this decision directly.
- **`source-link`** — [0327. The composer's two text layers share one scrollport](0327-the-composer-s-two-text-layers-share-one-scrollport.md): The source note links to this decision directly.
- **`source-link`** — [0368. The overlay composer seat compensates for the bar instead of reserving a gutter](0368-the-overlay-composer-seat-compensates-for-the-bar-instead-of-reserving-a.md): The source note links to this decision directly.
- **`shares-code-with`** — [0341. The conversation column scrolls on one axis](0341-the-conversation-column-scrolls-on-one-axis.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/ContextBody.tsx`, `packages/client/ui-deliverables/src/client/ProducedFiles.tsx`.
- **`shares-code-with`** — [0612. A collapsed sidebar retains its control rail](0612-a-collapsed-sidebar-retains-its-control-rail.md): Shares source implementation: `packages/client/ui-attachment/src/AttachmentRail.tsx`, `packages/client/ui-conversation/src/client/chat/ChatView.tsx`.
- **`shares-code-with`** — [0113. Client Conversation business-node assembly and keyed Chat snapshots](0113-client-conversation-business-node-assembly-and-keyed-chat-snapshots.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/ChatView.tsx`.
- **`shares-code-with`** — [0248. Web turn run time and hover-revealed time chrome](0248-web-turn-run-time-and-hover-revealed-time-chrome.md): Shares source implementation: `apps/web/tests/composer-tab-geometry.e2e.ts`, `packages/client/ui-conversation/src/client/chat/ChatView.tsx`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0340-the-conversation-column-reserves-one-scrollbar-gutter-for-every-view.md`.
