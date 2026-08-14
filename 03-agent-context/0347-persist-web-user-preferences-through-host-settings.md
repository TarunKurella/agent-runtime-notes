---
id: "dsh-note-0347"
title: "Persist Web user preferences through Host settings"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-06-host-backed-web-preferences.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "queue"
  - "locale"
  - "sync"
  - "mutate"
  - "preference"
  - "dark"
  - "ThemeRuntime"
  - "theme"
  - "system"
  - "yaml"
  - "set"
  - "localStorage"
  - "locale.preference"
  - "ui-theme.preference"
search_regex: "(?i)(queue|locale|sync|mutate|preference|dark|ThemeRuntime|theme)"
---

# 0347. Persist Web user preferences through Host settings — implementation context

## Open this when

The Web Appearance, Language, and busy-Enter preferences lived in browser localStorage. Browser storage is scoped to an origin, so reopening dsh web on another port selected a different partition and lost choices even though both processes used the same DSH home. These are user-level product preferences; session selection, drafts, disclosure state, and other transient browser state remain page-local. The first theme implementation moved only Appearance to Host settings but awaited its initial RPC before providing ThemeRuntime. A slow or unavailable settings request therefore suspended the assembled page.

## Source decision

The owning Host halves register three schemas: optional locale.preference (zh or en, where absence delegates to the browser), ui-theme.preference (light, dark, or system, default system), and ui-conversation.busyEnter (queue or steer, default queue). The local settings provider stores explicit choices in $DSH_HOME/settings.yaml, which resolves to ~/.dsh/settings.yaml under the default home. The API proxy explicitly exposes all three namespaces beside the other Web settings; registration alone never crosses that configuration boundary.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-06-host-backed-web-preferences.md](../02-notes/implemented/bug-fix/2026-08-06-host-backed-web-preferences.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-06-host-backed-web-preferences.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-06-host-backed-web-preferences.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/locale/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/locale`. | `named-package-member` |
| [`packages/client/locale/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/locale`. | `named-package-member` |
| [`packages/client/locale/src/locales/zh.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/locales/zh.ts) | runtime implementation | Core file in the package named by the note: `packages/client/locale`. | `named-package-member` |
| [`packages/client/locale/src/locales/en.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/locales/en.ts) | runtime implementation | Core file in the package named by the note: `packages/client/locale`. | `named-package-member` |
| [`packages/client/schema-form/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/schema-form/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/schema-form`. | `named-package-member` |
| [`packages/client/locale/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/client/index.ts) | package entry point | Core file in the package named by the note: `packages/client/locale`. Defines `sync`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/schema-form/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/schema-form/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/schema-form`. | `named-package-member` |
| [`packages/client/locale/src/locales/settings.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/locales/settings.ts) | runtime implementation | Core file in the package named by the note: `packages/client/locale`. | `named-package-member` |
| [`packages/client/locale`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/schema-form`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/schema-form) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/agent-loop/src/agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts) | runtime implementation | Defines `system`, a construct named by the note. | `symbol-definition` |
| [`packages/client/hmr/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/client/index.ts) | package entry point | Defines `queue`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `queue` | `let` | [`packages/client/hmr/src/client/index.ts:144`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/client/index.ts#L144) | `let queue: Promise<void> = Promise.resolve()` |
| `locale` | `const` | [`packages/client/locale/src/client/index.ts:357`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/client/index.ts#L357) | `const locale = new LocaleRuntime(ctx, host)` |
| `sync` | `const` | [`packages/client/locale/src/client/index.ts:367`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/client/index.ts#L367) | `const sync = (snapshot: LocaleSnapshot): void => {` |
| `mutate` | `const` | [`packages/client/runtime/src/client/contract/store.ts:220`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/store.ts#L220) | `const mutate = decl.actions[key] as (draft: T, ...params: unknown[]) => void` |
| `preference` | `const` | [`packages/client/ui-theme/src/boot-theme.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/boot-theme.ts#L13) | `const preference = ${JSON.stringify(preference)}` |
| `dark` | `const` | [`packages/client/ui-theme/src/boot-theme.ts:17`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/boot-theme.ts#L17) | `const dark = preference === 'dark' \|\| systemDark` |
| `ThemeRuntime` | `class` | [`packages/client/ui-theme/src/client/index.ts:150`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/client/index.ts#L150) | `export class ThemeRuntime {` |
| `theme` | `const` | [`packages/client/ui-theme/src/client/index.ts:386`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/client/index.ts#L386) | `const theme = new ThemeRuntime(ctx, host)` |
| `system` | `const` | [`packages/core/agent-loop/src/agent.ts:337`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L337) | `const system = renderPrompt(assembly)` |
| `yaml` | `const` | [`packages/skill/skill-filesystem/src/index.ts:917`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts#L917) | `const yaml = raw.slice(start, closing.start)` |
| `set` | `const` | [`packages/test-support/client-runtime/src/sessions.ts:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/sessions.ts#L52) | `const set = listeners.get(key) ?? new Set()` |

### Tests and executable evidence

- [`packages/client/ui-theme/tests/host.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/host.client.spec.ts) — A test under the owning area exercises or imports `preference`. A test under the owning area exercises or imports `dark`.
- [`packages/client/ui-theme/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `ThemeRuntime`. A test under the owning area exercises or imports `preference`.
- [`packages/client/ui-theme/tests/theme.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/theme.client.spec.ts) — A test under the owning area exercises or imports `ThemeRuntime`. A test under the owning area exercises or imports `preference`.
- [`packages/client/ui-theme/tests/invariant.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/invariant.client.spec.ts) — A test under the owning area exercises or imports `ThemeRuntime`.
- [`packages/client/ui-theme/tests/boot-theme.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/boot-theme.client.spec.ts) — A test under the owning area exercises or imports `preference`. A test under the owning area exercises or imports `dark`.
- [`packages/client/ui-theme/tests/settings-store.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/settings-store.client.spec.ts) — A test under the owning area exercises or imports `preference`. A test under the owning area exercises or imports `dark`.
- [`packages/client/ui-theme/tests/appearance-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/appearance-row.client.spec.tsx) — A test under the owning area exercises or imports `preference`. A test under the owning area exercises or imports `dark`.
- [`packages/client/ui-theme/tests/scrollbar-styles.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/scrollbar-styles.client.spec.ts) — A test under the owning area exercises or imports `dark`.

## How to read the implementation

1. Start with [`packages/client/locale/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `queue`, `locale`, `sync`, `mutate`, `preference`, `dark`, `ThemeRuntime`, `theme`, `system`, `yaml`, `set`, `localStorage`, `locale.preference`, `ui-theme.preference`
- Regex: `(?i)(queue|locale|sync|mutate|preference|dark|ThemeRuntime|theme)`

```bash
rg -n --pcre2 "(?i)(queue|locale|sync|mutate|preference|dark|ThemeRuntime|theme)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0227. The Settings language a fresh browser opens in comes from the browser](0227-the-settings-language-a-fresh-browser-opens-in-comes-from-the-browser.md): Shares source implementation: `packages/client/locale/src/client/index.ts`, `packages/client/locale/src/index.ts`.
- **`shares-code-with`** — [0510. Client Settings, Locale, and Theme layering](0510-client-settings-locale-and-theme-layering.md): Shares source implementation: `packages/client/locale/src/client/index.ts`, `packages/client/locale/src/index.ts`.
- **`shares-code-with`** — [0208. Ask-question Web presentation](0208-ask-question-web-presentation.md): Shares source implementation: `packages/client/locale/src/index.ts`, `packages/client/locale/src/invariant.ts`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/concurrency`.
- **`same-design-pressure`** — [0045. Web client architecture --- the client cordis plugin tree, the slot system, and the React-free object layer](0045-web-client-architecture-the-client-cordis-plugin-tree-the-slot-system-an.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/concurrency`.
- **`same-design-pressure`** — [0054. Client plugin loading --- plain packages, dsh.client plugins, and the two-phase boot](0054-client-plugin-loading-plain-packages-dsh-client-plugins-and-the-two-phas.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/concurrency`.
- **`same-design-pressure`** — [0278. Web background-job display](0278-web-background-job-display.md): Shares design concerns: `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0514. Cordis Host/Client Dynamic Plugin Runtime](0514-cordis-host-client-dynamic-plugin-runtime.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0347-persist-web-user-preferences-through-host-settings.md`.
