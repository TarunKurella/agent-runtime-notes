---
id: "dsh-note-0397"
title: "Web styling system --- the token framework and engineering constraints"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-19-web-styling-system.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/session-state"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/generation"
aliases:
  - "composes"
  - "--bg-*"
  - "--text-*"
  - "web-ui/src/style/global.css"
  - "--dsw-*"
  - "packages/client/ui-theme/src/styles/"
  - "body[data-ds-dark-theme]"
  - "--accent: #3964fe"
  - "--bg-sidebar"
  - "--bubble-bg"
  - "[data-theme='dark']"
  - ".module.css"
  - "className"
  - ".scrollable"
search_regex: "(?i)(composes|\\-\\-bg\\-\\*|\\-\\-text\\-\\*|web\\-ui/src/style/global\\.css|\\-\\-dsw\\-\\*|packages/client/ui\\-theme/src/styles/|body\\[data\\-ds\\-dark\\-theme\\]|\\-\\-accent:[- ]\\#3964fe)"
---

# 0397. Web styling system --- the token framework and engineering constraints — implementation context

## Open this when

The GUI has no designer supply; styles are written by an agent and reviewed. Without a machine-checkable token system and coding rules, colors/radii/motion drift as literals across components, and dark mode grows into conditional branches scattered inside components.

## Source decision

The source note does not isolate a compact implementation decision; read it as a whole before changing code.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-19-web-styling-system.md](../02-notes/implemented/process/2026-07-19-web-styling-system.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-19-web-styling-system.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-19-web-styling-system.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/web-styling.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/web-styling.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `packages/client/ui-theme/src/styles/` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/client/ui-theme/src/styles`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/styles) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`scripts/gen-config-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-config-catalog.ts) | repository automation | Defines `composes`, a construct named by the note. | `symbol-definition` |
| [`docs/web-styling.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/web-styling.zh.md) | package contract and examples | Contains the exact code literal `packages/client/ui-theme/src/styles/` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `composes` | `const` | [`scripts/gen-config-catalog.ts:429`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-config-catalog.ts#L429) | `const composes: string[] = []` |

### Tests and executable evidence

- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `composes`.
- [`scripts/gen-doc-graphs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.spec.ts) — A test under the owning area exercises or imports `var`.
- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — A test under the owning area exercises or imports `composes`.
- [`apps/web/tests/pwa-manifest.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/pwa-manifest.e2e.ts) — A test under the owning area exercises or imports `prefers-color-scheme`.
- [`apps/cli/tests/windows-shell.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/windows-shell.spec.ts) — A test under the owning area exercises or imports `composes`.
- [`apps/web/tests/settings-chrome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/settings-chrome.e2e.ts) — A test under the owning area exercises or imports `rgba`.
- [`apps/web/tests/built-boot.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/built-boot.snapshot.ts) — A test under the owning area exercises or imports `composes`.
- [`apps/web/tests/search-card.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/search-card.snapshot.ts) — A test under the owning area exercises or imports `composes`.

## How to read the implementation

1. Start with [`docs/web-styling.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/web-styling.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/evidence`, `concern/ownership`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/session-state`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/generation`
- Aliases: `composes`, `--bg-*`, `--text-*`, `web-ui/src/style/global.css`, `--dsw-*`, `packages/client/ui-theme/src/styles/`, `body[data-ds-dark-theme]`, `--accent: #3964fe`, `--bg-sidebar`, `--bubble-bg`, `[data-theme='dark']`, `.module.css`, `className`, `.scrollable`
- Regex: `(?i)(composes|\-\-bg\-\*|\-\-text\-\*|web\-ui/src/style/global\.css|\-\-dsw\-\*|packages/client/ui\-theme/src/styles/|body\[data\-ds\-dark\-theme\]|\-\-accent:[- ]\#3964fe)`

```bash
rg -n --pcre2 "(?i)(composes|\\-\\-bg\\-\\*|\\-\\-text\\-\\*|web\\-ui/src/style/global\\.css|\\-\\-dsw\\-\\*|packages/client/ui\\-theme/src/styles/|body\\[data\\-ds\\-dark\\-theme\\]|\\-\\-accent:[- ]\\#3964fe)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0684. Adopt execa for hand-rolled test subprocess plumbing](0684-adopt-execa-for-hand-rolled-test-subprocess-plumbing.md): Shares source implementation: `apps/cli/tests/built-bin.e2e.ts`, `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0429. Browser GIFs preserve one evidence chain](0429-browser-gifs-preserve-one-evidence-chain.md): Shares source implementation: `apps/cli/tests/built-bin.e2e.ts`, `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0267. Resolved theme color metadata](0267-resolved-theme-color-metadata.md): Shares source implementation: `apps/web/tests/pwa-manifest.e2e.ts`, `apps/web/tests/settings-chrome.e2e.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0166. The dsh CLI and personal config overlays from the Harness home](0166-the-dsh-cli-and-personal-config-overlays-from-the-harness-home.md): Shares source implementation: `apps/cli/tests/built-bin.e2e.ts`.
- **`shares-code-with`** — [0219. Steer a queued Web message into the active turn](0219-steer-a-queued-web-message-into-the-active-turn.md): Shares source implementation: `apps/web/tests/settings-chrome.e2e.ts`.
- **`shares-code-with`** — [0456. Remove the stdio and Echo agents](0456-remove-the-stdio-and-echo-agents.md): Shares source implementation: `apps/cli/tests/built-bin.e2e.ts`.
- **`shares-code-with`** — [0568. Startup slogans replace the configured TUI welcome line](0568-startup-slogans-replace-the-configured-tui-welcome-line.md): Shares source implementation: `apps/web/tests/scaffold.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0397-web-styling-system-the-token-framework-and-engineering-constraints.md`.
