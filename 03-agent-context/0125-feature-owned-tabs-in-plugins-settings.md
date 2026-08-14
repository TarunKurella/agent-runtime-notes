---
id: "dsh-note-0125"
title: "Feature-owned tabs in Plugins settings"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-11-plugin-settings-tabs.md"
implementation_evidence: "medium"
target_anchor: "append-only event and session store"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/registry"
aliases:
  - "plugins"
  - "all"
  - "section"
  - "item"
  - "settings.section"
  - "@deepseek-ai/dsh-client-ui-settings-plugins"
  - "settings.plugins.tab"
  - "ui-settings"
  - "settings.plugin.item"
  - "@deepseek-ai/dsh-client-ui-settings-plugin-inventory"
  - "ctx.slots.inject"
  - "ui-settings-plugins"
  - "ui-settings-general"
  - "ui-settings-plugin-inventory"
search_regex: "(?i)(plugins|section|item|settings\\.section|@deepseek\\-ai/dsh\\-client\\-ui\\-settings\\-plugins|settings\\.plugins\\.tab|ui\\-settings|settings\\.plugin\\.item)"
---

# 0125. Feature-owned tabs in Plugins settings — implementation context

## Open this when

Plugin configuration and the read-only Loader inventory each registered a top-level settings.section. They described the same Plugins domain but occupied two navigation rows, split search and configuration into unrelated pages, and gave the Settings shell no principled way to present them together. Combining their components directly would instead make one feature plugin import and own the other feature's data lifecycle.

## Source decision

@deepseek-ai/dsh-client-ui-settings-plugins owns the single settings.section contribution with id plugins. It renders the shared title and compact tab chrome, declares the root-scoped list slot settings.plugins.tab, and projects that ledger's id, order, and locale-following label into its tabs. The slot's canonical type lives in ui-settings, so a tab contributor depends on the Settings domain contract rather than on another feature plugin. The section owner contributes a configurable tab that declares the existing nested settings.plugin.item list.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-11-plugin-settings-tabs.md](../02-notes/implemented/architecture/2026-08-11-plugin-settings-tabs.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-11-plugin-settings-tabs.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-11-plugin-settings-tabs.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-settings/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-settings`. | `named-package-member` |
| [`packages/client/ui-settings/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-settings`. | `named-package-member` |
| [`packages/client/ui-settings-general/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-settings-general`. | `named-package-member` |
| [`packages/client/ui-settings-plugins/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-settings-plugins`. | `named-package-member` |
| [`packages/client/ui-settings-general/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-settings-general`. | `named-package-member` |
| [`packages/client/ui-settings-plugins/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-settings-plugins`. | `named-package-member` |
| [`packages/client/ui-settings-plugin-inventory/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugin-inventory/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-settings-plugin-inventory`. | `named-package-member` |
| [`packages/client/ui-settings-plugin-inventory/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugin-inventory/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-settings-plugin-inventory`. | `named-package-member` |
| [`packages/client/ui-settings`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/ui-settings-general`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/ui-settings-plugins`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/ui-settings-plugin-inventory`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugin-inventory) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `plugins` | `const` | [`apps/cli/src/plugin.ts:63`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts#L63) | `const plugins = after.dsh?.profile?.bundles ?? []` |
| `all` | `const` | [`packages/jobs/jobs-local/src/index.ts:485`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs-local/src/index.ts#L485) | `const all = [...this.store.values()]` |
| `section` | `const` | [`packages/web/tool-web/src/fetch.ts:43`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts#L43) | `const section = row.parentElement as HTMLTableSectionElement` |
| `item` | `const` | [`vendor/schemastery/src/index.ts:414`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/schemastery/src/index.ts#L414) | `const item = schema?.simplify(value[key])` |

### Tests and executable evidence

- [`packages/client/ui-settings-general/tests/host.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/tests/host.client.spec.ts) — A test under the owning area exercises or imports `ui-settings-general`.
- [`packages/client/ui-settings-general/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `ui-settings`. A test under the owning area exercises or imports `ui-settings-general`.
- [`packages/client/ui-settings-plugins/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `ui-settings-plugins`.
- [`packages/client/ui-settings-plugin-inventory/tests/invariant.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugin-inventory/tests/invariant.client.spec.ts) — A test under the owning area exercises or imports `ui-settings-plugin-inventory`.
- [`packages/client/ui-settings-plugin-inventory/tests/browser-plugin.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugin-inventory/tests/browser-plugin.client.spec.tsx) — A test under the owning area exercises or imports `ui-settings-plugin-inventory`.

## How to read the implementation

1. Start with [`packages/client/ui-settings/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/discovery-routing`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `concern/simplification`, `domain/configuration`, `domain/extensions`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/registry`
- Aliases: `plugins`, `all`, `section`, `item`, `settings.section`, `@deepseek-ai/dsh-client-ui-settings-plugins`, `settings.plugins.tab`, `ui-settings`, `settings.plugin.item`, `@deepseek-ai/dsh-client-ui-settings-plugin-inventory`, `ctx.slots.inject`, `ui-settings-plugins`, `ui-settings-general`, `ui-settings-plugin-inventory`
- Regex: `(?i)(plugins|section|item|settings\.section|@deepseek\-ai/dsh\-client\-ui\-settings\-plugins|settings\.plugins\.tab|ui\-settings|settings\.plugin\.item)`

```bash
rg -n --pcre2 "(?i)(plugins|section|item|settings\\.section|@deepseek\\-ai/dsh\\-client\\-ui\\-settings\\-plugins|settings\\.plugins\\.tab|ui\\-settings|settings\\.plugin\\.item)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0217. Versioned GUI welcome onboarding](0217-versioned-gui-welcome-onboarding.md): Shares source implementation: `packages/client/ui-settings`, `packages/client/ui-settings-general/src/index.ts`.
- **`shares-code-with`** — [0213. official DeepSeek first-run credential setup](0213-official-deepseek-first-run-credential-setup.md): Shares source implementation: `packages/client/ui-settings`, `packages/client/ui-settings/src/index.ts`.
- **`shares-code-with`** — [0493. Remove the first-run beta notice](0493-remove-the-first-run-beta-notice.md): Shares source implementation: `packages/client/ui-settings-general`, `packages/client/ui-settings-general/src/index.ts`.
- **`shares-code-with`** — [0349. onboarding takeover chrome moves into the step](0349-onboarding-takeover-chrome-moves-into-the-step.md): Shares source implementation: `packages/client/ui-settings-general/src/index.ts`, `packages/client/ui-settings-general/src/invariant.ts`.
- **`shares-code-with`** — [0287. Plugin configuration in the web settings page](0287-plugin-configuration-in-the-web-settings-page.md): Shares source implementation: `packages/client/ui-settings-plugins/src/index.ts`.
- **`shares-code-with`** — [0510. Client Settings, Locale, and Theme layering](0510-client-settings-locale-and-theme-layering.md): Shares source implementation: `packages/client/ui-settings/src/index.ts`.
- **`same-design-pressure`** — [0054. Client plugin loading --- plain packages, dsh.client plugins, and the two-phase boot](0054-client-plugin-loading-plain-packages-dsh-client-plugins-and-the-two-phas.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/lifecycle`.
- **`same-design-pressure`** — [0126. Repository naming contract and pre-release rename ledger](0126-repository-naming-contract-and-pre-release-rename-ledger.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/lifecycle`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0125-feature-owned-tabs-in-plugins-settings.md`.
