---
id: "dsh-note-0368"
title: "The overlay composer seat compensates for the bar instead of reserving a gutter"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-12-composer-overlay-seat-width-compensation.md"
implementation_evidence: "high"
target_anchor: "optional UI or client layer"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "right"
  - "scrollbar-gutter: auto"
  - "--dsh-scrollbar-width"
  - "::-webkit-scrollbar"
  - "scrollbar-width: thin"
  - "apps/web/tests/composer-tab-geometry.e2e.ts"
  - "scrollbar-gutter: stable"
  - "The overlay composer seat compensates for the bar instead of reserving a gutter"
  - "bug fix"
  - "boundary"
  - "evidence"
  - "ownership"
  - "build release"
  - "testing"
search_regex: "(?i)(right|scrollbar\\-gutter:[- ]auto|\\-\\-dsh\\-scrollbar\\-width|::\\-webkit\\-scrollbar|scrollbar\\-width:[- ]thin|apps/web/tests/composer\\-tab\\-geometry\\.e2e\\.ts|scrollbar\\-gutter:[- ]stable|bug[- ]fix)"
---

# 0368. The overlay composer seat compensates for the bar instead of reserving a gutter — implementation context

## Open this when

The composer-tab gutter reservation made the column's scroller reserve a scrollbar gutter unconditionally, so the composer seat measured the same width in Chat and in a view with a composer overlay. The cost was paid by every overlay view: the view's content column ended 8px short of the column's right edge, because the scroller reserved a gutter for a bar it never draws --- the trajectory ledger owns its own scrollers and the outer box never scrolls.

## Source decision

The reservation now belongs to Chat alone. The overlay branch declares scrollbar-gutter: auto, so the view's content spans the full column; the overlay composer seat (absolutely positioned against the padding box) gives back the bar's width with right: var(--dsh-scrollbar-width), so the input card still measures the same width as Chat's seat and does not move between tabs. The compensation value is not a literal: ui-theme's scrollbar.css defines --dsh-scrollbar-width (8px on the WebKit path) beside the ::-webkit-scrollbar rule it mirrors, and the seat reads that variable.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-12-composer-overlay-seat-width-compensation.md](../02-notes/implemented/bug-fix/2026-08-12-composer-overlay-seat-width-compensation.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-12-composer-overlay-seat-width-compensation.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-12-composer-overlay-seat-width-compensation.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/settings/settings/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts) | package entry point | Defines `right`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-attachment/src/AttachmentRail.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/AttachmentRail.tsx) | runtime implementation | Defines `right`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/ChatView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx) | runtime implementation | Defines `right`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/sessions/conversation-assembler.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation-assembler.ts) | runtime implementation | Defines `right`, a construct named by the note. | `symbol-definition` |
| [`tsconfig.host.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.host.json) | composition and configuration | Contains the exact code literal `apps/web/tests/composer-tab-geometry.e2e.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `right` | `const` | [`packages/client/runtime/src/client/sessions/conversation-assembler.ts:104`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation-assembler.ts#L104) | `const right = existing[current]` |
| `right` | `const` | [`packages/client/runtime/src/client/sessions/conversation-assembler.ts:647`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation-assembler.ts#L647) | `const right = sorted[added]` |
| `right` | `const` | [`packages/client/ui-attachment/src/AttachmentRail.tsx:85`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/AttachmentRail.tsx#L85) | `const right = el.scrollLeft < el.scrollWidth - el.clientWidth - 1` |
| `right` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:63`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L63) | `const right = Math.min(viewport.right, content.right)` |
| `right` | `const` | [`packages/settings/settings/src/index.ts:153`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts#L153) | `const right = b as Record<string, unknown>` |

### Tests and executable evidence

- [`apps/web/tests/composer-tab-geometry.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/composer-tab-geometry.e2e.ts) — The source note names this file directly. A test under the owning area exercises or imports `auto`.
- [`scripts/run-oxlint.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-oxlint.spec.ts) — A test under the owning area exercises or imports `auto`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `auto`.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `auto`.
- [`apps/web/tests/web-search-round.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/web-search-round.e2e.ts) — A test under the owning area exercises or imports `auto`.
- [`apps/web/tests/navigation-panes.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/navigation-panes.e2e.ts) — A test under the owning area exercises or imports `auto`.
- [`apps/web/tests/sidebar-scrollbar.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/sidebar-scrollbar.e2e.ts) — A test under the owning area exercises or imports `auto`.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `auto`.
- Source verification intent: apps/web/tests/composer-tab-geometry.e2e.ts still asserts the card holds its position across tabs and now also asserts the split: Chat's scroller keeps scrollbar-gutter: stable and a nonzero band, while the overlay branch resolves auto with a zero band. The control cascade changed with the mechanism: it now drops the seat's right compensation (instead of dropping a gutter Chat never had on that branch) and measures the same 4px shift, proving the equal rectangles are not a tab switch that never reached layout. The committed golden records both states.

## How to read the implementation

1. Start with [`packages/settings/settings/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** optional UI or client layer.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/evidence`, `concern/ownership`, `domain/build-release`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `right`, `scrollbar-gutter: auto`, `--dsh-scrollbar-width`, `::-webkit-scrollbar`, `scrollbar-width: thin`, `apps/web/tests/composer-tab-geometry.e2e.ts`, `scrollbar-gutter: stable`, `The overlay composer seat compensates for the bar instead of reserving a gutter`, `bug fix`, `boundary`, `evidence`, `ownership`, `build release`, `testing`
- Regex: `(?i)(right|scrollbar\-gutter:[- ]auto|\-\-dsh\-scrollbar\-width|::\-webkit\-scrollbar|scrollbar\-width:[- ]thin|apps/web/tests/composer\-tab\-geometry\.e2e\.ts|scrollbar\-gutter:[- ]stable|bug[- ]fix)`

```bash
rg -n --pcre2 "(?i)(right|scrollbar\\-gutter:[- ]auto|\\-\\-dsh\\-scrollbar\\-width|::\\-webkit\\-scrollbar|scrollbar\\-width:[- ]thin|apps/web/tests/composer\\-tab\\-geometry\\.e2e\\.ts|scrollbar\\-gutter:[- ]stable|bug[- ]fix)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0340. The conversation column reserves one scrollbar gutter for every view](0340-the-conversation-column-reserves-one-scrollbar-gutter-for-every-view.md): The source note links to this decision directly.
- **`shares-code-with`** — [0612. A collapsed sidebar retains its control rail](0612-a-collapsed-sidebar-retains-its-control-rail.md): Shares source implementation: `packages/client/ui-attachment/src/AttachmentRail.tsx`, `packages/client/ui-conversation/src/client/chat/ChatView.tsx`.
- **`shares-code-with`** — [0684. Adopt execa for hand-rolled test subprocess plumbing](0684-adopt-execa-for-hand-rolled-test-subprocess-plumbing.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `apps/web/tests/smoke-real.e2e.ts`.
- **`shares-code-with`** — [0351. Reader scroll attribution through the observed-top ledger](0351-reader-scroll-attribution-through-the-observed-top-ledger.md): Shares source implementation: `apps/web/tests/web-search-round.e2e.ts`, `packages/client/ui-conversation/src/client/chat/ChatView.tsx`.
- **`shares-code-with`** — [0433. Standardize Chinese contract terminology on 约定](0433-standardize-chinese-contract-terminology-on-contract.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `apps/web/tests/smoke-real.e2e.ts`.
- **`shares-code-with`** — [0568. Startup slogans replace the configured TUI welcome line](0568-startup-slogans-replace-the-configured-tui-welcome-line.md): Shares source implementation: `apps/web/tests/navigation-panes.e2e.ts`, `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0611. Keep the Code Mode result card complete](0611-keep-the-code-mode-result-card-complete.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `apps/web/tests/smoke-real.e2e.ts`.
- **`shares-code-with`** — [0429. Browser GIFs preserve one evidence chain](0429-browser-gifs-preserve-one-evidence-chain.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `apps/web/tests/smoke-real.e2e.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0368-the-overlay-composer-seat-compensates-for-the-bar-instead-of-reserving-a.md`.
