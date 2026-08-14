---
id: "dsh-note-0250"
title: "The sidebar's scrollbars follow the pointer"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-04-pointer-revealed-sidebar-scrollbars.md"
implementation_evidence: "high"
target_anchor: "append-only event and session store"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "SidebarRoot"
  - "quietBars"
  - "--dsh-scrollbar-thumb"
  - "--dsh-scrollbar-thumb-hover"
  - "SCROLLBAR_LINGER_MS = 2000"
  - "scrollbar-gutter: stable"
  - "scrollbar-color"
  - "ui-theme/tests/scrollbar-styles.spec.ts"
  - "transition-delay"
  - "scrollbar-width: none"
  - "::-webkit-scrollbar"
  - "packages/client/ui-sidebar/tests/pointer-scrollbars.client.spec.tsx"
  - "relatedTarget"
  - "packages/client/ui-sidebar/tests/scrollbar-quiet-styles.client.spec.ts"
search_regex: "(?i)(SidebarRoot|quietBars|\\-\\-dsh\\-scrollbar\\-thumb|\\-\\-dsh\\-scrollbar\\-thumb\\-hover|SCROLLBAR_LINGER_MS[- ]=[- ]2000|scrollbar\\-gutter:[- ]stable|scrollbar\\-color|ui\\-theme/tests/scrollbar\\-styles\\.spec\\.ts)"
---

# 0250. The sidebar's scrollbars follow the pointer — implementation context

## Open this when

The sidebar's session list overflows after a handful of sessions, and from that point its scrollbar is drawn permanently --- in a column that is at rest most of the time, next to rows whose own chrome only appears on hover. It is the one piece of always-on furniture in the sidebar, and nothing about it is actionable until someone reaches for it. The product ask (2026-08-04) is to draw it only while the pointer is over the sidebar, with a short tail so it does not blink out on the way past.

## Source decision

SidebarRoot tracks the pointer over the whole column and carries a quietBars class whenever it is outside. The rule that class selects rebinds ui-theme's indirection pair --- --dsh-scrollbar-thumb and --dsh-scrollbar-thumb-hover --- to transparent, so every scroll region nested under the column draws no thumb. The session list is the only one today; a future one inherits the behavior rather than opting into it. The tail is SCROLLBAR_LINGER_MS = 2000: leaving arms a timer, entering cancels a pending one, and only the timer firing puts the class back.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-04-pointer-revealed-sidebar-scrollbars.md](../02-notes/implemented/feature/2026-08-04-pointer-revealed-sidebar-scrollbars.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-04-pointer-revealed-sidebar-scrollbars.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-04-pointer-revealed-sidebar-scrollbars.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-sidebar/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-sidebar/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-sidebar`. | `named-package-member` |
| [`packages/client/ui-sidebar/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-sidebar/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-sidebar`. | `named-package-member` |
| [`packages/client/ui-sidebar/src/client/SidebarRoot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-sidebar/src/client/SidebarRoot.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-sidebar`. Defines `SidebarRoot`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-sidebar`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-sidebar) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/ui-sidebar/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-sidebar/README.md) | package contract and examples | Core file in the package named by the note: `packages/client/ui-sidebar`. | `named-package-member` |
| [`packages/client/ui-sidebar/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-sidebar/package.json) | composition and configuration | Core file in the package named by the note: `packages/client/ui-sidebar`. | `named-package-member` |
| [`tsconfig.host.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.host.json) | composition and configuration | Contains the exact code literal `apps/web/tests/sidebar-scrollbar.e2e.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `SidebarRoot` | `function` | [`packages/client/ui-sidebar/src/client/SidebarRoot.tsx:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-sidebar/src/client/SidebarRoot.tsx#L44) | `export function SidebarRoot({` |

### Tests and executable evidence

- [`apps/web/tests/sidebar-scrollbar.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/sidebar-scrollbar.e2e.ts) — The source note names this file directly.
- [`packages/client/ui-sidebar/tests/pointer-scrollbars.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-sidebar/tests/pointer-scrollbars.client.spec.tsx) — The source note names this file directly. A test under the owning area exercises or imports `SidebarRoot`.
- [`packages/client/ui-sidebar/tests/scrollbar-quiet-styles.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-sidebar/tests/scrollbar-quiet-styles.client.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `SidebarRoot`.
- [`packages/client/ui-sidebar/tests/sidebar-root.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-sidebar/tests/sidebar-root.client.spec.tsx) — A test under the owning area exercises or imports `SidebarRoot`.
- [`packages/client/ui-sidebar/tests/sidebar-styles.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-sidebar/tests/sidebar-styles.client.spec.ts) — A test under the owning area exercises or imports `SidebarRoot`.
- Source verification intent: packages/client/ui-sidebar/tests/pointer-scrollbars.client.spec.tsx drives the class through the transitions with fake timers: revealed on entry, still revealed one millisecond before the linger closes, quiet one millisecond after, and cancelled by a return within the window. Two more cover the geometric leave: a pointermove landing outside the column's box hides the bars without any DOM leave (the settings-panel shape), and one landing back inside cancels a pending hide.

## How to read the implementation

1. Start with [`packages/client/ui-sidebar/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-sidebar/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/ownership`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `SidebarRoot`, `quietBars`, `--dsh-scrollbar-thumb`, `--dsh-scrollbar-thumb-hover`, `SCROLLBAR_LINGER_MS = 2000`, `scrollbar-gutter: stable`, `scrollbar-color`, `ui-theme/tests/scrollbar-styles.spec.ts`, `transition-delay`, `scrollbar-width: none`, `::-webkit-scrollbar`, `packages/client/ui-sidebar/tests/pointer-scrollbars.client.spec.tsx`, `relatedTarget`, `packages/client/ui-sidebar/tests/scrollbar-quiet-styles.client.spec.ts`
- Regex: `(?i)(SidebarRoot|quietBars|\-\-dsh\-scrollbar\-thumb|\-\-dsh\-scrollbar\-thumb\-hover|SCROLLBAR_LINGER_MS[- ]=[- ]2000|scrollbar\-gutter:[- ]stable|scrollbar\-color|ui\-theme/tests/scrollbar\-styles\.spec\.ts)`

```bash
rg -n --pcre2 "(?i)(SidebarRoot|quietBars|\\-\\-dsh\\-scrollbar\\-thumb|\\-\\-dsh\\-scrollbar\\-thumb\\-hover|SCROLLBAR_LINGER_MS[- ]=[- ]2000|scrollbar\\-gutter:[- ]stable|scrollbar\\-color|ui\\-theme/tests/scrollbar\\-styles\\.spec\\.ts)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0312. The scrollbar tokens get their consumer, and the workspace list reserves its gutter](0312-the-scrollbar-tokens-get-their-consumer-and-the-workspace-list-reserves.md): The source note links to this decision directly.
- **`same-design-pressure`** — [0151. Parallel tool-call execution by per-call safety](0151-parallel-tool-call-execution-by-per-call-safety.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.
- **`same-design-pressure`** — [0126. Repository naming contract and pre-release rename ledger](0126-repository-naming-contract-and-pre-release-rename-ledger.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.
- **`same-design-pressure`** — [0070. A capability-discriminated directory-picker seam for the web-GUI host](0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.
- **`same-design-pressure`** — [0032. Tool output spill policy](0032-tool-output-spill-policy.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.
- **`same-design-pressure`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.
- **`same-design-pressure`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0250-the-sidebar-s-scrollbars-follow-the-pointer.md`.
