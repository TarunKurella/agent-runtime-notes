---
id: "dsh-note-0359"
title: "Pre-Plugin Theme Bootstrap"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-10-pre-plugin-theme-bootstrap.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "dark"
  - "system"
  - "color-scheme"
  - "body[data-ds-dark-theme]"
  - "dshClient.immediately"
  - "ctx.webServer.tapIndex"
  - "httpServer"
  - "ui-theme.preference"
  - "prefers-color-scheme"
  - "matchMedia"
  - "document.documentElement.style.colorScheme"
  - "colorScheme"
  - "apps/web/index.html"
  - "Pre-Plugin Theme Bootstrap"
search_regex: "(?i)(dark|system|color\\-scheme|body\\[data\\-ds\\-dark\\-theme\\]|dshClient\\.immediately|ctx\\.webServer\\.tapIndex|httpServer|ui\\-theme\\.preference)"
---

# 0359. Pre-Plugin Theme Bootstrap — implementation context

## Open this when

The web shell renders Loading plugins… before the browser-side plugin tree activates. The theme tokens are already loaded with the shell styles, but color-scheme and body[data-ds-dark-theme] are not written until ui-theme's ThemeRuntime and ui-layout's ThemePresenter activate; with a persisted dark preference, the loading page therefore renders first with the light palette and then switches to dark. dshClient.immediately only includes the bundle in first-stage prefetching; it does not cause the plugin to execute before HTML parsing or the shell's initial render.

## Source decision

ui-theme's host half transforms each index HTML document through ctx.webServer.tapIndex(), inserting a synchronous inline script immediately after the opening tag. The transform registers under an optional httpServer injection, so compositions without that service still activate ui-theme and install no transform. When the HTML parser executes the script, the body exists, but the shell's module script and React root have not yet run. The host half registers the ui-theme.preference settings section when a settings provider exists.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-10-pre-plugin-theme-bootstrap.md](../02-notes/implemented/bug-fix/2026-08-10-pre-plugin-theme-bootstrap.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-10-pre-plugin-theme-bootstrap.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-10-pre-plugin-theme-bootstrap.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/web/index.html`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/index.html) | supporting file | The source note names this file directly. | `named-file` |
| [`packages/core/agent-loop/src/agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts) | runtime implementation | Defines `system`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-theme/src/boot-theme.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/boot-theme.ts) | runtime implementation | Defines `dark`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-title-llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title-llm/src/index.ts) | package entry point | Defines `system`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/acp-snapshot/src/suite.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts) | runtime implementation | Defines `system`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `dark` | `const` | [`packages/client/ui-theme/src/boot-theme.ts:17`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/boot-theme.ts#L17) | `const dark = preference === 'dark' \|\| systemDark` |
| `system` | `const` | [`packages/core/agent-loop/src/agent.ts:337`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L337) | `const system = renderPrompt(assembly)` |
| `system` | `const` | [`packages/session/session-title-llm/src/index.ts:250`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title-llm/src/index.ts#L250) | `const system = systemPrompt(config)` |
| `system` | `const` | [`packages/test-support/acp-snapshot/src/suite.ts:409`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts#L409) | `const system = (header as { system?: unknown }).system` |

### Tests and executable evidence

- [`scripts/test-invariants.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-invariants.ts) — A test under the owning area exercises or imports `immediately`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `immediately`.
- [`scripts/test-fixture-cleanup.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-fixture-cleanup.ts) — A test under the owning area exercises or imports `immediately`.
- [`apps/web/tests/assembled-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/assembled-boot.ts) — A test under the owning area exercises or imports `immediately`.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `immediately`. A test under the owning area exercises or imports `light`.
- [`apps/web/tests/pwa-manifest.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/pwa-manifest.e2e.ts) — A test under the owning area exercises or imports `prefers-color-scheme`. A test under the owning area exercises or imports `light`.
- [`apps/web/tests/built-boot.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/built-boot.snapshot.ts) — A test under the owning area exercises or imports `immediately`.
- [`apps/web/tests/settings-chrome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/settings-chrome.e2e.ts) — A test under the owning area exercises or imports `colorScheme`. A test under the owning area exercises or imports `light`.
- Source verification intent: ui-theme's unit tests cover activation without either optional Host service, the script position, Host-setting precedence, the OS preference, missing matchMedia, input without a body, live settings reads, and disposal of the Host registrations with the plugin fiber. A Chromium scenario for the real web composition selects the durable dark preference, holds the plugin bundle request open to keep the loading page observable, then asserts that the index response produces a dark background, the body attribute, and the root element's color-scheme.

## How to read the implementation

1. Start with [`apps/web/index.html`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/index.html) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/projection`, `mechanism/registry`
- Aliases: `dark`, `system`, `color-scheme`, `body[data-ds-dark-theme]`, `dshClient.immediately`, `ctx.webServer.tapIndex`, `httpServer`, `ui-theme.preference`, `prefers-color-scheme`, `matchMedia`, `document.documentElement.style.colorScheme`, `colorScheme`, `apps/web/index.html`, `Pre-Plugin Theme Bootstrap`
- Regex: `(?i)(dark|system|color\-scheme|body\[data\-ds\-dark\-theme\]|dshClient\.immediately|ctx\.webServer\.tapIndex|httpServer|ui\-theme\.preference)`

```bash
rg -n --pcre2 "(?i)(dark|system|color\\-scheme|body\\[data\\-ds\\-dark\\-theme\\]|dshClient\\.immediately|ctx\\.webServer\\.tapIndex|httpServer|ui\\-theme\\.preference)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0347. Persist Web user preferences through Host settings](0347-persist-web-user-preferences-through-host-settings.md): The source note links to this decision directly.
- **`shares-code-with`** — [0433. Standardize Chinese contract terminology on 约定](0433-standardize-chinese-contract-terminology-on-contract.md): Shares source implementation: `apps/web/tests/assembled-boot.ts`, `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0684. Adopt execa for hand-rolled test subprocess plumbing](0684-adopt-execa-for-hand-rolled-test-subprocess-plumbing.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `apps/web/tests/smoke-real.e2e.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0611. Keep the Code Mode result card complete](0611-keep-the-code-mode-result-card-complete.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `apps/web/tests/smoke-real.e2e.ts`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`.
- **`shares-code-with`** — [0429. Browser GIFs preserve one evidence chain](0429-browser-gifs-preserve-one-evidence-chain.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `apps/web/tests/smoke-real.e2e.ts`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `scripts/test-fixture-cleanup.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0359-pre-plugin-theme-bootstrap.md`.
