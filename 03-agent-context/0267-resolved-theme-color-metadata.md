---
id: "dsh-note-0267"
title: "Resolved theme color metadata"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-06-resolved-theme-color-metadata.md"
implementation_evidence: "medium"
target_anchor: "Rust harness core contract"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "domain/configuration"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
aliases:
  - "ThemePresenter"
  - "ThemeDefinition"
  - "theme_color"
  - "<meta name=\"theme-color\">"
  - "color-scheme"
  - "background-color"
  - "background_color"
  - "prefers-color-scheme"
  - "themeColor"
  - "theme-color"
  - "Resolved theme color metadata"
  - "feature"
  - "boundary"
  - "discovery routing"
search_regex: "(?i)(ThemePresenter|ThemeDefinition|theme_color|<meta[- ]name=\"theme\\-color\">|color\\-scheme|background\\-color|background_color|prefers\\-color\\-scheme)"
---

# 0267. Resolved theme color metadata — implementation context

## Open this when

The web client can resolve its theme independently of the operating-system preference, so a single manifest theme_color or media-qualified static metadata can disagree with an explicit Light or Dark selection. Browser chrome around an installed or ordinary page then need not match the app surface even though the layout presenter already owns the resolved document palette.

## Source decision

The ui-layout ThemePresenter owns one alongside its root color-scheme, dark-palette attribute, and inline token writes. After applying a resolved snapshot's palette and token overrides, the presenter reads the body's computed background-color into the metadata element and inserts that single node into the document head. Subsequent snapshots update the same node, and disposal removes it. The rendered body background remains the color authority. The PWA manifest carries no static theme_color or background_color, and ThemeDefinition gains no second color field that could drift from the token palette.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-06-resolved-theme-color-metadata.md](../02-notes/implemented/feature/2026-08-06-resolved-theme-color-metadata.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-06-resolved-theme-color-metadata.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-06-resolved-theme-color-metadata.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-theme/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/client/index.ts) | package entry point | Defines `ThemeDefinition`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-layout/src/client/theme-presenter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/theme-presenter.ts) | runtime implementation | Defines `ThemePresenter`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ThemePresenter` | `class` | [`packages/client/ui-layout/src/client/theme-presenter.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/theme-presenter.ts#L16) | `export class ThemePresenter {` |
| `ThemeDefinition` | `interface` | [`packages/client/ui-theme/src/client/index.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/client/index.ts#L61) | `export interface ThemeDefinition {` |

### Tests and executable evidence

- [`apps/web/tests/pwa-manifest.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/pwa-manifest.e2e.ts) — A test under the owning area exercises or imports `prefers-color-scheme`.
- [`apps/web/tests/settings-chrome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/settings-chrome.e2e.ts) — A test under the owning area exercises or imports `themeColor`. A test under the owning area exercises or imports `theme-color`.
- [`packages/client/ui-theme/tests/theme.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/theme.client.spec.ts) — A test under the owning area exercises or imports `prefers-color-scheme`.
- [`packages/client/ui-layout/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `theme-color`.
- [`packages/client/ui-theme/tests/boot-theme.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/boot-theme.client.spec.ts) — A test under the owning area exercises or imports `color-scheme`.
- [`packages/client/ui-layout/tests/theme-presenter.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/tests/theme-presenter.client.spec.ts) — A test under the owning area exercises or imports `ThemePresenter`. A test under the owning area exercises or imports `color-scheme`.
- [`packages/client/ui-theme/tests/scrollbar-styles.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/scrollbar-styles.client.spec.ts) — A test under the owning area exercises or imports `background-color`.
- [`packages/client/ui-primitives/tests/fixtures/markdown-dom/code-fences.settled.txt`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/fixtures/markdown-dom/code-fences.settled.txt) — A test under the owning area exercises or imports `background-color`.
- Source verification intent: The presenter unit contract covers light and dark computed colors, node reuse, and disposal. The ui-layout composition test covers initial insertion, event-driven reuse, and fiber cleanup. The Web browser settings scenario drives Light, Dark, System, operating-system changes, and reload through the shipped composition, asserting one metadata element whose content equals the computed body background with no console errors. The metadata change has no rendered accessibility-tree output, so the existing scenario golden remains unchanged.

## How to read the implementation

1. Start with [`packages/client/ui-theme/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/client/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust harness core contract.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `domain/configuration`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`
- Aliases: `ThemePresenter`, `ThemeDefinition`, `theme_color`, `<meta name="theme-color">`, `color-scheme`, `background-color`, `background_color`, `prefers-color-scheme`, `themeColor`, `theme-color`, `Resolved theme color metadata`, `feature`, `boundary`, `discovery routing`
- Regex: `(?i)(ThemePresenter|ThemeDefinition|theme_color|<meta[- ]name="theme\-color">|color\-scheme|background\-color|background_color|prefers\-color\-scheme)`

```bash
rg -n --pcre2 "(?i)(ThemePresenter|ThemeDefinition|theme_color|<meta[- ]name=\"theme\\-color\">|color\\-scheme|background\\-color|background_color|prefers\\-color\\-scheme)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0319. Web details follow the current Session lifecycle](0319-web-details-follow-the-current-session-lifecycle.md): Shares source implementation: `apps/web/tests/settings-chrome.e2e.ts`, `packages/client/ui-layout/tests/apply.client.spec.ts`.
- **`shares-code-with`** — [0628. Web favicon follows the color scheme](0628-web-favicon-follows-the-color-scheme.md): Shares source implementation: `apps/web/tests/pwa-manifest.e2e.ts`, `packages/client/ui-theme/src/client/index.ts`.
- **`shares-code-with`** — [0397. Web styling system --- the token framework and engineering constraints](0397-web-styling-system-the-token-framework-and-engineering-constraints.md): Shares source implementation: `apps/web/tests/pwa-manifest.e2e.ts`, `apps/web/tests/settings-chrome.e2e.ts`.
- **`shares-code-with`** — [0219. Steer a queued Web message into the active turn](0219-steer-a-queued-web-message-into-the-active-turn.md): Shares source implementation: `apps/web/tests/settings-chrome.e2e.ts`.
- **`shares-code-with`** — [0458. Plan-specific collaboration state](0458-plan-specific-collaboration-state.md): Shares source implementation: `packages/client/ui-theme/src/client/index.ts`.
- **`shares-code-with`** — [0312. The scrollbar tokens get their consumer, and the workspace list reserves its gutter](0312-the-scrollbar-tokens-get-their-consumer-and-the-workspace-list-reserves.md): Shares source implementation: `packages/client/ui-theme/tests/scrollbar-styles.client.spec.ts`.
- **`shares-code-with`** — [0624. Web details default closed](0624-web-details-default-closed.md): Shares source implementation: `packages/client/ui-layout/tests/apply.client.spec.ts`.
- **`shares-code-with`** — [0377. Mechanical quality gates over prose guidelines](0377-mechanical-quality-gates-over-prose-guidelines.md): Shares source implementation: `packages/client/ui-theme/tests/scrollbar-styles.client.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0267-resolved-theme-color-metadata.md`.
