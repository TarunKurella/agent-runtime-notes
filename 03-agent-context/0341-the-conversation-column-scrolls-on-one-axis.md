---
id: "dsh-note-0341"
title: "The conversation column scrolls on one axis"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-04-conversation-column-one-axis-scroll.md"
implementation_evidence: "high"
target_anchor: "optional UI or client layer"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "hidden"
  - "visible"
  - ".heroGlow"
  - "1051/776"
  - "[data-conversation-scroll]"
  - "overflow-y: auto"
  - ".scrollBody"
  - "overflow-x: hidden"
  - "stdDeviation=\"50"
  - "overflow-x: auto"
  - ".centerCol { overflow: hidden }"
  - "scrollWidth === clientWidth"
  - "scrollLeft"
  - "The conversation column scrolls on one axis"
search_regex: "(?i)(hidden|visible|\\.heroGlow|1051/776|\\[data\\-conversation\\-scroll\\]|overflow\\-y:[- ]auto|\\.scrollBody|overflow\\-x:[- ]hidden)"
---

# 0341. The conversation column scrolls on one axis — implementation context

## Open this when

Narrowing the center column --- by the window or by the sidebar drag --- put a horizontal scrollbar under the whole conversation column on the hero. The bleeding element is the hero's decorative backdrop ellipse: .heroGlow is sized 1051/776 of the hero box so its blur scales in userSpace with the input card, which means it reaches past the column whenever the column is narrower than the glow. That bleed is by construction and stays. What made it user-visible is the scroll container it sits in.

## Source decision

.scrollBody declares overflow-x: hidden. The column states that it is a one-axis scroller instead of leaving the second axis to be derived. Clipping does not change. overflow-y: auto had already made the box a scroll container that clips both axes, so the declaration withdraws only the scrollbar and the user gesture; the glow keeps its bleed, its blur radius, and the same painted extent, and the column keeps its vertical scroll. Nothing in the composer chain moves.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-04-conversation-column-one-axis-scroll.md](../02-notes/implemented/bug-fix/2026-08-04-conversation-column-one-axis-scroll.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-04-conversation-column-one-axis-scroll.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-04-conversation-column-one-axis-scroll.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `visible`, a construct named by the note. | `symbol-definition` |
| [`packages/skill/tool-skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts) | package entry point | Defines `visible`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `visible`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/JsonTree.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/JsonTree.tsx) | runtime implementation | Defines `visible`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/DiffBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DiffBlock.tsx) | runtime implementation | Defines `hidden`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/ReadBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ReadBlock.tsx) | runtime implementation | Defines `hidden`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/head-tail-cap.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/head-tail-cap.ts) | runtime implementation | Defines `hidden`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-deliverables/src/client/ProducedFiles.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-deliverables/src/client/ProducedFiles.tsx) | runtime implementation | Defines `hidden`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/ContextBody.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ContextBody.tsx) | runtime implementation | Defines `hidden`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `hidden` | `const` | [`packages/client/ui-conversation/src/client/chat/ContextBody.tsx:94`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ContextBody.tsx#L94) | `const hidden = formRendered ? ['kind', 'form'] : ['kind']` |
| `hidden` | `const` | [`packages/client/ui-deliverables/src/client/ProducedFiles.tsx:114`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-deliverables/src/client/ProducedFiles.tsx#L114) | `const hidden = paths.length - shown.length` |
| `hidden` | `const` | [`packages/client/ui-primitives/src/DiffBlock.tsx:159`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DiffBlock.tsx#L159) | `const hidden = rows.length - maxLines` |
| `visible` | `const` | [`packages/client/ui-primitives/src/JsonTree.tsx:139`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/JsonTree.tsx#L139) | `const visible = entries.slice(0, limit)` |
| `hidden` | `const` | [`packages/client/ui-primitives/src/ReadBlock.tsx:107`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ReadBlock.tsx#L107) | `const hidden = lines.length - maxLines` |
| `hidden` | `const` | [`packages/client/ui-primitives/src/head-tail-cap.ts:29`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/head-tail-cap.ts#L29) | `const hidden = total - maxLines` |
| `visible` | `const` | [`packages/core/tools/src/index.ts:1166`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1166) | `const visible = new Map<string, ToolDefinition>()` |
| `visible` | `const` | [`packages/core/tools/src/index.ts:1380`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1380) | `const visible = this.get(name, agent)` |
| `visible` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2052`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2052) | `const visible = await listVisibleSessionSummaries(signal)` |
| `visible` | `const` | [`packages/skill/tool-skill/src/index.ts:362`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts#L362) | `const visible = new Set(agent.session.surface.nodes)` |

### Tests and executable evidence

- [`apps/web/tests/conversation-column-overflow.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/conversation-column-overflow.e2e.ts) — The source note names this file directly. A test under the owning area exercises or imports `auto`.
- [`scripts/run-oxlint.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-oxlint.spec.ts) — A test under the owning area exercises or imports `auto`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `auto`.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `auto`.
- [`apps/web/tests/web-search-round.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/web-search-round.e2e.ts) — A test under the owning area exercises or imports `auto`.
- [`apps/web/tests/navigation-panes.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/navigation-panes.e2e.ts) — A test under the owning area exercises or imports `auto`.
- [`apps/web/tests/lifecycle-chrome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/lifecycle-chrome.e2e.ts) — A test under the owning area exercises or imports `scrollLeft`.
- [`apps/web/tests/sidebar-scrollbar.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/sidebar-scrollbar.e2e.ts) — A test under the owning area exercises or imports `auto`.
- Source verification intent: apps/web/tests/conversation-column-overflow.e2e.ts sweeps viewport widths bracketing the glow and, at each stop, wheels horizontally over the column and reads scrollLeft. The committed golden records the relation per stop; the widest stop is the control where the glow does not bleed at all. Two guards keep the scenario honest. The vacuity guard asserts the glow still reaches past the column at the narrow stops, so the claim cannot pass by the symptom having disappeared for an unrelated reason.

## How to read the implementation

1. Start with [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `domain/build-release`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `hidden`, `visible`, `.heroGlow`, `1051/776`, `[data-conversation-scroll]`, `overflow-y: auto`, `.scrollBody`, `overflow-x: hidden`, `stdDeviation="50`, `overflow-x: auto`, `.centerCol { overflow: hidden }`, `scrollWidth === clientWidth`, `scrollLeft`, `The conversation column scrolls on one axis`
- Regex: `(?i)(hidden|visible|\.heroGlow|1051/776|\[data\-conversation\-scroll\]|overflow\-y:[- ]auto|\.scrollBody|overflow\-x:[- ]hidden)`

```bash
rg -n --pcre2 "(?i)(hidden|visible|\\.heroGlow|1051/776|\\[data\\-conversation\\-scroll\\]|overflow\\-y:[- ]auto|\\.scrollBody|overflow\\-x:[- ]hidden)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0340. The conversation column reserves one scrollbar gutter for every view](0340-the-conversation-column-reserves-one-scrollbar-gutter-for-every-view.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/ContextBody.tsx`, `packages/client/ui-deliverables/src/client/ProducedFiles.tsx`.
- **`shares-code-with`** — [0593. Dim-gray pulse for the running prompt glyph](0593-dim-gray-pulse-for-the-running-prompt-glyph.md): Shares source implementation: `packages/client/ui-primitives/src/JsonTree.tsx`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0603. /details command for transcript detail state](0603-details-command-for-transcript-detail-state.md): Shares source implementation: `packages/client/ui-primitives/src/DiffBlock.tsx`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0127. Trajectory assembly from registered Conversation Contexts](0127-trajectory-assembly-from-registered-conversation-contexts.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0204. Independent model and user skill invocation policy](0204-independent-model-and-user-skill-invocation-policy.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/skill/tool-skill/src/index.ts`.
- **`shares-code-with`** — [0085. the end-seed log boundary](0085-the-end-seed-log-boundary.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/skill/tool-skill/src/index.ts`.
- **`shares-code-with`** — [0608. Hover cards copy their primary value on activation](0608-hover-cards-copy-their-primary-value-on-activation.md): Shares source implementation: `packages/client/ui-primitives/src/DiffBlock.tsx`, `packages/client/ui-primitives/src/JsonTree.tsx`.
- **`shares-code-with`** — [0625. Hero stays visible while a blank session opens](0625-hero-stays-visible-while-a-blank-session-opens.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/skill/tool-skill/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0341-the-conversation-column-scrolls-on-one-axis.md`.
