---
id: "dsh-note-0323"
title: "Hover popup pointer grace"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-30-hover-popup-pointer-grace.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
aliases:
  - "disabled"
  - "HoverCard"
  - "Menu"
  - "POINTER_GRACE_MS"
  - "usePointerGrace"
  - "onClose"
  - "pointer-events: none"
  - "closeOnPointerLeave"
  - "..."
  - "packages/client/ui-primitives/tests/hover-card.client.spec.tsx"
  - "tests/atoms.spec.tsx"
  - "apps/web/tests/workspace-management.e2e.ts"
  - "Hover popup pointer grace"
  - "bug fix"
search_regex: "(?i)(disabled|HoverCard|Menu|POINTER_GRACE_MS|usePointerGrace|onClose|pointer\\-events:[- ]none|closeOnPointerLeave)"
---

# 0323. Hover popup pointer grace — implementation context

## Open this when

Both popups the workspace browser rows raise floated out of reach of the pointer. HoverCard closed on the first pointerleave from its anchor and rendered its card pointer-events: none, but the card sits 8px off the anchor's right edge, so every path to it crossed ground belonging to neither and killed the card before it arrived --- the full workspace path and session title it exists to show could be read only in passing. The row action menus passed closeOnPointerLeave, whose handler sat on the portaled list: aiming back at the ...

## Source decision

usePointerGrace (packages/client/ui-primitives/src/pointer-grace.ts) owns one cancelable delayed close, shared by both atoms, with POINTER_GRACE_MS at 200. Leaving arms the close; coming back cancels it. Transit through an anchor-to-popup gap is therefore survivable, while a pointer that has genuinely moved on still dismisses the popup. HoverCard arms the grace on leave instead of closing, and its card no longer sets pointer-events: none, so resting on the card holds it open.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-30-hover-popup-pointer-grace.md](../02-notes/implemented/bug-fix/2026-07-30-hover-popup-pointer-grace.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-30-hover-popup-pointer-grace.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-30-hover-popup-pointer-grace.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-primitives/src/pointer-grace.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/pointer-grace.ts) | runtime implementation | The source note names this file directly. Defines `usePointerGrace`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/client/ui-primitives/src/Menu.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Menu.tsx) | runtime implementation | Defines `Menu`, a construct named by the note. | `symbol-definition` |
| [`packages/e2b/subprocess-e2b/src/process.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/process.ts) | runtime implementation | Defines `onClose`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/HoverCard.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx) | runtime implementation | Defines `HoverCard`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-settings-plugins/src/client/BashCard.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/client/BashCard.tsx) | runtime implementation | Defines `disabled`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-settings-models/src/client/ProviderEditor.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ProviderEditor.tsx) | provider/backend adapter | Defines `disabled`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx) | runtime implementation | Defines `disabled`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-settings-plugins/src/client/WebSearchCard.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/client/WebSearchCard.tsx) | runtime implementation | Defines `disabled`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-settings-models/src/client/CustomProviderCard.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/CustomProviderCard.tsx) | provider/backend adapter | Defines `disabled`, a construct named by the note. | `symbol-definition` |
| [`tsconfig.host.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.host.json) | composition and configuration | Contains the exact code literal `apps/web/tests/workspace-management.e2e.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `disabled` | `const` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:133`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L133) | `const disabled = removed \|\| inert \|\| !live \|\| blocked !== undefined \|\| parentOffline` |
| `HoverCard` | `function` | [`packages/client/ui-primitives/src/HoverCard.tsx:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx#L31) | `export function HoverCard({` |
| `Menu` | `function` | [`packages/client/ui-primitives/src/Menu.tsx:90`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Menu.tsx#L90) | `export function Menu({ open, anchor, items, selectedId, selectedIds, onSelect, onClose, align = 'start', side = 'bottom', portal = false, closeOnPointerLeave = false, dense = false, compact = false, getAnchorRect, footer` |
| `POINTER_GRACE_MS` | `const` | [`packages/client/ui-primitives/src/pointer-grace.ts:14`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/pointer-grace.ts#L14) | `export const POINTER_GRACE_MS = 200` |
| `usePointerGrace` | `function` | [`packages/client/ui-primitives/src/pointer-grace.ts:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/pointer-grace.ts#L31) | `export function usePointerGrace(close: () => void): PointerGrace {` |
| `disabled` | `const` | [`packages/client/ui-settings-models/src/client/CustomProviderCard.tsx:95`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/CustomProviderCard.tsx#L95) | `const disabled = props.readOnly \|\| busy` |
| `disabled` | `const` | [`packages/client/ui-settings-models/src/client/ProviderEditor.tsx:162`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ProviderEditor.tsx#L162) | `const disabled = props.readOnly \|\| busy` |
| `disabled` | `const` | [`packages/client/ui-settings-plugins/src/client/BashCard.tsx:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/client/BashCard.tsx#L23) | `const disabled = !state.writable` |
| `disabled` | `const` | [`packages/client/ui-settings-plugins/src/client/WebSearchCard.tsx:27`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/client/WebSearchCard.tsx#L27) | `const disabled = !state.writable` |
| `onClose` | `const` | [`packages/e2b/subprocess-e2b/src/process.ts:465`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/process.ts#L465) | `const onClose = (): void => { cleanup(); resolve() }` |

### Tests and executable evidence

- [`apps/web/tests/workspace-management.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/workspace-management.e2e.ts) — The source note names this file directly. A test under the owning area exercises or imports `pointerleave`.
- [`packages/client/ui-primitives/tests/hover-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/hover-card.client.spec.tsx) — The source note names this file directly. A test under the owning area exercises or imports `HoverCard`.
- [`packages/client/ui-primitives/tests/atoms.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/atoms.client.spec.tsx) — A test under the owning area exercises or imports `closeOnPointerLeave`. A test under the owning area exercises or imports `POINTER_GRACE_MS`.
- [`packages/client/ui-sidebar/tests/pointer-scrollbars.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-sidebar/tests/pointer-scrollbars.client.spec.tsx) — A test under the owning area exercises or imports `pointerleave`.
- Source verification intent: packages/client/ui-primitives/tests/hover-card.client.spec.tsx and tests/atoms.spec.tsx pin the grace boundary, cancel-on-return, no-second-dwell, disarm-on-owner-close, and the no-arming-while-closed case. The reachability gestures themselves --- hovering onto the card, and moving between an open list and its trigger --- are pinned in the real browser by apps/web/tests/workspace-management.e2e.ts, since they depend on hit testing and layout that jsdom does not model.

## How to read the implementation

1. Start with [`packages/client/ui-primitives/src/pointer-grace.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/pointer-grace.ts) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`
- Aliases: `disabled`, `HoverCard`, `Menu`, `POINTER_GRACE_MS`, `usePointerGrace`, `onClose`, `pointer-events: none`, `closeOnPointerLeave`, `...`, `packages/client/ui-primitives/tests/hover-card.client.spec.tsx`, `tests/atoms.spec.tsx`, `apps/web/tests/workspace-management.e2e.ts`, `Hover popup pointer grace`, `bug fix`
- Regex: `(?i)(disabled|HoverCard|Menu|POINTER_GRACE_MS|usePointerGrace|onClose|pointer\-events:[- ]none|closeOnPointerLeave)`

```bash
rg -n --pcre2 "(?i)(disabled|HoverCard|Menu|POINTER_GRACE_MS|usePointerGrace|onClose|pointer\\-events:[- ]none|closeOnPointerLeave)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0220. Web read card frontend --- the read tool's line window renders line-numbered and highlighted](0220-web-read-card-frontend-the-read-tool-s-line-window-renders-line-numbered.md): Shares source implementation: `packages/client/ui-primitives/src/HoverCard.tsx`.
- **`shares-code-with`** — [0213. official DeepSeek first-run credential setup](0213-official-deepseek-first-run-credential-setup.md): Shares source implementation: `packages/client/ui-settings-models/src/client/ProviderEditor.tsx`.
- **`shares-code-with`** — [0199. Workspace Registration Deletion](0199-workspace-registration-deletion.md): Shares source implementation: `packages/client/ui-primitives/src/Menu.tsx`.
- **`shares-code-with`** — [0608. Hover cards copy their primary value on activation](0608-hover-cards-copy-their-primary-value-on-activation.md): Shares source implementation: `packages/client/ui-primitives/src/HoverCard.tsx`.
- **`shares-code-with`** — [0320. The approval takeover shares the composer's text cap](0320-the-approval-takeover-shares-the-composer-s-text-cap.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/InputBar.tsx`.
- **`shares-code-with`** — [0319. Web details follow the current Session lifecycle](0319-web-details-follow-the-current-session-lifecycle.md): Shares source implementation: `apps/web/tests/workspace-management.e2e.ts`.
- **`shares-code-with`** — [0327. The composer's two text layers share one scrollport](0327-the-composer-s-two-text-layers-share-one-scrollport.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/InputBar.tsx`.
- **`shares-code-with`** — [0333. Web stop preserves pending Queue](0333-web-stop-preserves-pending-queue.md): Shares source implementation: `packages/client/ui-primitives/src/pointer-grace.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0323-hover-popup-pointer-grace.md`.
