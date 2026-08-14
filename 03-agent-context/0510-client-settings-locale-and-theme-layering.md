---
id: "dsh-note-0510"
title: "Client Settings, Locale, and Theme layering"
status: "proposed"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/architecture/2026-07-25-client-settings-locale-theme.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/security"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/proposed"
  - "mechanism/generation"
  - "mechanism/registry"
aliases:
  - "active"
  - "section"
  - "locale"
  - "models"
  - "preference"
  - "dark"
  - "theme"
  - "settings"
  - "system"
  - "header"
  - "only"
  - "trigger"
  - "item"
  - "ui-settings-*"
search_regex: "(?i)(active|section|locale|models|preference|dark|theme|settings)"
---

# 0510. Client Settings, Locale, and Theme layering — implementation context

## Open this when

The browser client's existing Settings is written directly inside the Sidebar, and language and theme are applied by component-local state mutating the DOM directly. As a result Settings cannot be extended by independent plugins, preference state has no stable cross-plugin service contract, and the theme registry carries both state and presentation responsibilities.

## Source decision

Collaboration doctrine (how every later module joins Settings): feature owners self-register. The Settings shell is a pure composition surface: it only declares slots and renders the chrome structure --- zero copy, no locale dependency, and neither importing nor enumerating any feature; for a feature to appear in Settings, its own plugin registers into the corresponding slot --- locale registers the Language row, ui-theme registers the Appearance row, ui-settings-models registers the Models top-level panel.

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/architecture/2026-07-25-client-settings-locale-theme.md](../02-notes/proposed/architecture/2026-07-25-client-settings-locale-theme.md)
- Pinned source: [.agents/notes/proposed/architecture/2026-07-25-client-settings-locale-theme.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/architecture/2026-07-25-client-settings-locale-theme.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/locale/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/locale`. | `named-package-member` |
| [`packages/client/ui-theme/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-theme`. Defines `settings`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-layout/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-layout`. | `named-package-member` |
| [`packages/client/locale/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/locale`. | `named-package-member` |
| [`packages/settings/settings/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts) | package entry point | Core file in the package named by the note: `packages/settings/settings`. Defines `section`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/settings/settings/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/settings/settings`. | `named-package-member` |
| [`packages/client/ui-settings/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-settings`. | `named-package-member` |
| [`packages/client/ui-theme/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-theme`. | `named-package-member` |
| [`packages/client/locale/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/client/index.ts) | package entry point | Core file in the package named by the note: `packages/client/locale`. Defines `section`, a construct named by the note. | `exact-code-occurrence, named-package-member, symbol-definition` |
| [`packages/client/ui-layout/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-layout`. | `named-package-member` |
| [`packages/client/ui-theme/src/boot-theme.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/boot-theme.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-theme`. Defines `dark`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/settings/settings/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/settings/settings`. Defines `settings`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `active` | `const` | [`packages/client/locale/src/client/LanguageRow.tsx:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/client/LanguageRow.tsx#L31) | `const active = useStore(s => s.active)` |
| `section` | `const` | [`packages/client/locale/src/client/index.ts:188`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/client/index.ts#L188) | `const section = host.getSnapshot().value` |
| `locale` | `const` | [`packages/client/locale/src/client/index.ts:357`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/client/index.ts#L357) | `const locale = new LocaleRuntime(ctx, host)` |
| `active` | `const` | [`packages/client/ui-settings-general/src/client/SettingsRoot.tsx:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/src/client/SettingsRoot.tsx#L46) | `const active = rows.find(r => r.id === activeId)?.id ?? rows[0]?.id` |
| `models` | `const` | [`packages/client/ui-settings-models/src/client/DeepSeekModelsEditor.tsx:96`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/DeepSeekModelsEditor.tsx#L96) | `const models = modelDrafts(value)` |
| `models` | `const` | [`packages/client/ui-settings-models/src/client/ProviderEditor.tsx:341`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ProviderEditor.tsx#L341) | `const models = modelDrafts(modelsOverridden ? customModels : inheritedModels())` |
| `preference` | `const` | [`packages/client/ui-theme/src/boot-theme.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/boot-theme.ts#L13) | `const preference = ${JSON.stringify(preference)}` |
| `dark` | `const` | [`packages/client/ui-theme/src/boot-theme.ts:17`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/boot-theme.ts#L17) | `const dark = preference === 'dark' \|\| systemDark` |
| `preference` | `const` | [`packages/client/ui-theme/src/client/AppearanceRow.tsx:43`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/client/AppearanceRow.tsx#L43) | `const preference = useStore(s => s.preference)` |
| `section` | `const` | [`packages/client/ui-theme/src/client/index.ts:234`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/client/index.ts#L234) | `const section = this.host.getSnapshot().value` |
| `active` | `const` | [`packages/client/ui-theme/src/client/index.ts:298`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/client/index.ts#L298) | `const active = this.themes.find(t => t.id === resolvedId)` |
| `theme` | `const` | [`packages/client/ui-theme/src/client/index.ts:386`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/client/index.ts#L386) | `const theme = new ThemeRuntime(ctx, host)` |
| `settings` | `const` | [`packages/client/ui-theme/src/index.ts:21`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/index.ts#L21) | `const settings = ctx.get('settings')` |
| `section` | `const` | [`packages/client/ui-theme/src/index.ts:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/index.ts#L23) | `const section = settings.get(THEME_NAMESPACE) as ThemeSettings \| undefined` |
| `system` | `const` | [`packages/core/agent-loop/src/agent.ts:337`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L337) | `const system = renderPrompt(assembly)` |
| `header` | `const` | [`packages/core/session/src/index.ts:259`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L259) | `const header = record?.['header']` |

### Tests and executable evidence

- [`packages/client/locale/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `setLocale`.
- [`packages/client/ui-theme/tests/host.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/host.client.spec.ts) — A test under the owning area exercises or imports `theme`. A test under the owning area exercises or imports `dark`.
- [`packages/client/locale/tests/locale.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/tests/locale.client.spec.ts) — A test under the owning area exercises or imports `setLocale`. Contains the exact code literal `locale/change` named by the note.
- [`packages/client/ui-theme/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `light`. A test under the owning area exercises or imports `dark`.
- [`packages/client/ui-theme/tests/theme.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/theme.client.spec.ts) — A test under the owning area exercises or imports `light`. A test under the owning area exercises or imports `dark`.
- [`packages/client/ui-layout/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `ui-theme`. A test under the owning area exercises or imports `theme`.
- [`packages/client/ui-layout/tests/columns.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/tests/columns.client.spec.ts) — A test under the owning area exercises or imports `preference`.
- [`packages/client/ui-theme/tests/boot-theme.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/boot-theme.client.spec.ts) — A test under the owning area exercises or imports `theme`. A test under the owning area exercises or imports `light`.
- Source verification intent: The Settings shell depends only on the slot ledger, never on any feature implementation; General's item list likewise depends only on the ledger. Adding a settings item = the feature package registering it itself (a section or a general item), with zero shell changes. Locale and Theme writes go only through the setters; ongoing synchronization goes only through the change events. Each feature row's store initializes from the getter and is thereafter updated by its own change event with local re-renders.

## How to read the implementation

1. Start with [`packages/client/locale/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/discovery-routing`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/security`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/proposed`, `mechanism/generation`, `mechanism/registry`
- Aliases: `active`, `section`, `locale`, `models`, `preference`, `dark`, `theme`, `settings`, `system`, `header`, `only`, `trigger`, `item`, `ui-settings-*`
- Regex: `(?i)(active|section|locale|models|preference|dark|theme|settings)`

```bash
rg -n --pcre2 "(?i)(active|section|locale|models|preference|dark|theme|settings)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0080. Full client copy rollout onto the typed locale seat, and the non-translation boundary](0080-full-client-copy-rollout-onto-the-typed-locale-seat-and-the-non-translat.md): The source note links to this decision directly.
- **`source-link`** — [0103. Slot declaration injection and reload lifetimes](0103-slot-declaration-injection-and-reload-lifetimes.md): The source note links to this decision directly.
- **`shares-code-with`** — [0073. user-settings seam (`ctx.settings`) and the file provider](0073-user-settings-seam-ctx-settings-and-the-file-provider.md): Shares source implementation: `packages/client/ui-theme/src/index.ts`, `packages/client/ui-theme/src/invariant.ts`.
- **`shares-code-with`** — [0347. Persist Web user preferences through Host settings](0347-persist-web-user-preferences-through-host-settings.md): Shares source implementation: `packages/client/locale/src/client/index.ts`, `packages/client/locale/src/index.ts`.
- **`shares-code-with`** — [0227. The Settings language a fresh browser opens in comes from the browser](0227-the-settings-language-a-fresh-browser-opens-in-comes-from-the-browser.md): Shares source implementation: `packages/client/locale/src/client/index.ts`, `packages/client/locale/src/index.ts`.
- **`shares-code-with`** — [0086. settings write-path integrity and observer lifecycle](0086-settings-write-path-integrity-and-observer-lifecycle.md): Shares source implementation: `packages/settings/settings/src/index.ts`, `packages/settings/settings/src/types.ts`.
- **`shares-code-with`** — [0355. Broken presets are roster rows, not gaps](0355-broken-presets-are-roster-rows-not-gaps.md): Shares source implementation: `packages/settings/settings/src/index.ts`, `packages/settings/settings/src/types.ts`.
- **`shares-code-with`** — [0208. Ask-question Web presentation](0208-ask-question-web-presentation.md): Shares source implementation: `packages/client/locale/src/index.ts`, `packages/client/locale/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0510-client-settings-locale-and-theme-layering.md`.
