---
id: "dsh-note-0366"
title: "Preset cards clamp their description instead of sizing the roster"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-11-preset-card-description-clamp.md"
implementation_evidence: "medium"
target_anchor: "append-only event and session store"
tags:
  - "class/bug-fix"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/session-state"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
aliases:
  - "top"
  - "Tooltip"
  - "maxWidth"
  - "title"
  - "description"
  - "broken"
  - "min-height"
  - "grid-auto-rows: 1fr"
  - "scrollHeight > clientHeight"
  - ".cardId"
  - "margin-top: auto"
  - "aria-label"
  - "ResizeObserver"
  - "damaged.expected.md"
search_regex: "(?i)(Tooltip|maxWidth|title|description|broken|min\\-height|grid\\-auto\\-rows:[- ]1fr|scrollHeight[- ]>[- ]clientHeight)"
---

# 0366. Preset cards clamp their description instead of sizing the roster — implementation context

## Open this when

A preset publishes its own description, of any length, and the settings section renders the roster as a card grid. The description had a min-height and no upper bound, while the grid sizes rows with grid-auto-rows: 1fr --- which makes every implicit row the same height, not just the row holding the tall card. One long description therefore set the height of the whole roster: with a 250-character description in the custom group, all four cards measured 421px and the short-description cards filled with blank space.

## Source decision

The description clamps to four lines and offers the rest through the shared Tooltip, attached only while the element actually overflows (scrollHeight > clientHeight, re-measured through a ResizeObserver because the settings pane width follows the window). This mirrors the chat stats line, which clamps to one line on the same measure-then-attach rule. Card height stays derived rather than fixed. With the description bounded, grid-auto-rows: 1fr already equalizes the grid, and a card carrying the broken-preset reason or a revealed path still sizes itself --- a pixel height would clip both.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-11-preset-card-description-clamp.md](../02-notes/implemented/bug-fix/2026-08-11-preset-card-description-clamp.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-11-preset-card-description-clamp.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-11-preset-card-description-clamp.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/web/src/app.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/app.tsx) | runtime implementation | Defines `title`, a construct named by the note. | `symbol-definition` |
| [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) | package entry point | Defines `description`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Defines `description`, a construct named by the note. | `symbol-definition` |
| [`scripts/verify-doc-site-fragments.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-doc-site-fragments.ts) | repository automation | Defines `broken`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `title`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/analyzer.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts) | runtime implementation | Defines `description`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-title/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts) | package entry point | Defines `title`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/Tooltip.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Tooltip.tsx) | runtime implementation | Defines `Tooltip`, a construct named by the note. | `symbol-definition` |
| [`packages/preset/agent-presets/src/discovery.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/discovery.ts) | runtime implementation | Defines `broken`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/HoverCard.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx) | runtime implementation | Defines `top`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx) | runtime implementation | Defines `maxWidth`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `top` | `const` | [`packages/client/ui-primitives/src/HoverCard.tsx:110`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx#L110) | `const top = r.top + h > window.innerHeight - 8 ? window.innerHeight - h - 8 : r.top` |
| `Tooltip` | `function` | [`packages/client/ui-primitives/src/Tooltip.tsx:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Tooltip.tsx#L41) | `export function Tooltip({ label, side = 'right', delayMs = 0, disabled = false, maxWidth, children }: { label: TooltipLabel; side?: TooltipSide; delayMs?: number; disabled?: boolean; maxWidth?: number; children: ReactEle` |
| `maxWidth` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx:243`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx#L243) | `const maxWidth = Math.max(` |
| `title` | `const` | [`packages/client/web/src/app.tsx:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/app.tsx#L32) | `const title = useSessions((state) => {` |
| `description` | `const` | [`packages/core/tools/src/py-types.ts:224`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L224) | `const description = (schema as Record<string, unknown>).description` |
| `description` | `const` | [`packages/core/tools/src/py-types.ts:572`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L572) | `const description = describe(fieldSchema)` |
| `description` | `const` | [`packages/core/tools/src/py-types.ts:804`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L804) | `const description = describe(schema)` |
| `title` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2831`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2831) | `const title = payload.title.trim()` |
| `broken` | `const` | [`packages/preset/agent-presets/src/discovery.ts:153`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/discovery.ts#L153) | `const broken = await isFile(path)` |
| `title` | `const` | [`packages/session/session-title/src/index.ts:595`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts#L595) | `const title = normalizeSessionTitle(candidate.title, this.config.maxTitleBytes)` |
| `title` | `const` | [`packages/session/session-title/src/index.ts:745`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts#L745) | `const title = fallbackSessionTitle(first.text, this.config.fallbackMaxWords, this.config.fallbackMaxBytes)` |
| `title` | `const` | [`packages/session/session-title/src/index.ts:761`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts#L761) | `const title = fallbackSessionTitle(` |
| `description` | `const` | [`packages/skill/skill/src/index.ts:751`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L751) | `const description = skill.description` |
| `description` | `const` | [`packages/typert/generator/src/analyzer.ts:2815`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L2815) | `const description = normalizedDocText(ts.getTextOfJSDocComment(block.comment))` |
| `broken` | `const` | [`scripts/verify-doc-site-fragments.ts:108`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-doc-site-fragments.ts#L108) | `const broken: BrokenSiteFragment[] = []` |

### Tests and executable evidence

- [`apps/web/tests/assembled-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/assembled-boot.ts) — A test under the owning area exercises or imports `ResizeObserver`.
- [`apps/web/tests/models-settings.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/models-settings.e2e.ts) — A test under the owning area exercises or imports `aria-label`.
- [`apps/web/tests/message-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/message-actions.e2e.ts) — A test under the owning area exercises or imports `aria-label`.
- [`apps/web/tests/navigation-panes.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/navigation-panes.e2e.ts) — A test under the owning area exercises or imports `aria-label`.
- [`apps/web/tests/live-interactions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/live-interactions.e2e.ts) — A test under the owning area exercises or imports `bottom`.
- [`apps/web/tests/question-composer.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/question-composer.e2e.ts) — A test under the owning area exercises or imports `bottom`.
- [`apps/web/tests/approval-composer.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/approval-composer.e2e.ts) — A test under the owning area exercises or imports `bottom`. A test under the owning area exercises or imports `aria-label`.
- [`apps/web/tests/sidebar-scrollbar.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/sidebar-scrollbar.e2e.ts) — A test under the owning area exercises or imports `aria-label`.
- Source verification intent: Package tests cover the three measurement outcomes (cut off, fitting, and a runtime without ResizeObserver) and the tooltip width cap. The web e2e goldens replay unchanged except damaged.expected.md, re-recorded for the badge copy.

## How to read the implementation

1. Start with [`packages/client/web/src/app.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/app.tsx) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/configuration`, `domain/session-state`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`
- Aliases: `top`, `Tooltip`, `maxWidth`, `title`, `description`, `broken`, `min-height`, `grid-auto-rows: 1fr`, `scrollHeight > clientHeight`, `.cardId`, `margin-top: auto`, `aria-label`, `ResizeObserver`, `damaged.expected.md`
- Regex: `(?i)(Tooltip|maxWidth|title|description|broken|min\-height|grid\-auto\-rows:[- ]1fr|scrollHeight[- ]>[- ]clientHeight)`

```bash
rg -n --pcre2 "(?i)(Tooltip|maxWidth|title|description|broken|min\\-height|grid\\-auto\\-rows:[- ]1fr|scrollHeight[- ]>[- ]clientHeight)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0594. Fixed `Tool / <name>` header for tool-call cards](0594-fixed-tool-name-header-for-tool-call-cards.md): Shares source implementation: `packages/client/web/src/app.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0237. opening a produced file from the web UI](0237-opening-a-produced-file-from-the-web-ui.md): Shares source implementation: `packages/client/web/src/app.tsx`, `packages/core/tools/src/py-types.ts`.
- **`shares-code-with`** — [0619. Tool-card single-row fields render inline](0619-tool-card-single-row-fields-render-inline.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0478. Copy-only preset authoring, and the way into a preset's files](0478-copy-only-preset-authoring-and-the-way-into-a-preset-s-files.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/skill/skill/src/index.ts`.
- **`shares-code-with`** — [0435. Concrete prose names actors and recorded facts](0435-concrete-prose-names-actors-and-recorded-facts.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/session/session-title/src/index.ts`.
- **`shares-code-with`** — [0337. Todo-first composer context order](0337-todo-first-composer-context-order.md): Shares source implementation: `packages/session/session-title/src/index.ts`, `packages/skill/skill/src/index.ts`.
- **`shares-code-with`** — [0185. Workspace UI Complete Product Flow](0185-workspace-ui-complete-product-flow.md): Shares source implementation: `packages/client/web/src/app.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0204. Independent model and user skill invocation policy](0204-independent-model-and-user-skill-invocation-policy.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/skill/skill/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0366-preset-cards-clamp-their-description-instead-of-sizing-the-roster.md`.
