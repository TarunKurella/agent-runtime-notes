---
id: "dsh-note-0227"
title: "The Settings language a fresh browser opens in comes from the browser"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-31-browser-derived-initial-locale.md"
implementation_evidence: "high"
target_anchor: "QuickJS-to-Rust capability registry"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "FALLBACK_LOCALE"
  - "LocaleRuntime"
  - "resolveInitialLocale"
  - "detectBrowserLocale"
  - "locale"
  - "language"
  - "preference"
  - "window"
  - "usePinnedBrowserLanguages"
  - "Config"
  - "dsh.locale"
  - "navigator.languages"
  - "packages/client/locale/src/client/index.ts"
  - "locale.preference"
search_regex: "(?i)(FALLBACK_LOCALE|LocaleRuntime|resolveInitialLocale|detectBrowserLocale|locale|language|preference|window)"
---

# 0227. The Settings language a fresh browser opens in comes from the browser — implementation context

## Open this when

The Settings Language row opened every first visit in Chinese: LocaleRuntime read dsh.locale from localStorage and fell straight back to zh when nothing was stored. The browser already states which languages its user reads --- navigator.languages is that statement --- and the app ignored it, so an English reader met a Chinese product and had to find a Chinese-labelled settings row to escape it. The fallback was doing two jobs at once: the last resort for an unresolvable locale, and the answer for every user who had simply never chosen.

## Source decision

The provisional locale resolves through the browser, then FALLBACK_LOCALE; an explicit Host preference replaces it live. resolveInitialLocale() in packages/client/locale/src/client/index.ts runs at service construction and expresses the browser/fallback order. The nonblocking settings lifecycle then applies optional locale.preference from $DSH_HOME/settings.yaml; absence leaves the browser-derived value active. Browser matching is on the primary subtag, over the ordered list. detectBrowserLocale() walks [...(navigator.languages ??

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-31-browser-derived-initial-locale.md](../02-notes/implemented/feature/2026-07-31-browser-derived-initial-locale.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-31-browser-derived-initial-locale.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-31-browser-derived-initial-locale.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/locale/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/client/index.ts) | package entry point | The source note names this file directly. Core file in the package named by the note: `packages/client/locale`. | `named-file, named-package-member, symbol-definition` |
| [`packages/client/locale/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/locale`. | `named-package-member` |
| [`packages/client/locale/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/locale`. | `named-package-member` |
| [`packages/client/locale/src/locales/zh.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/locales/zh.ts) | runtime implementation | Core file in the package named by the note: `packages/client/locale`. | `named-package-member` |
| [`packages/client/locale/src/locales/en.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/locales/en.ts) | runtime implementation | Core file in the package named by the note: `packages/client/locale`. | `named-package-member` |
| [`packages/client/locale/src/locales/settings.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/locales/settings.ts) | runtime implementation | Core file in the package named by the note: `packages/client/locale`. | `named-package-member` |
| [`packages/test-support/client-runtime/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/client-runtime`. | `named-package-member` |
| [`packages/test-support/client-runtime/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/test-support/client-runtime`. | `named-package-member` |
| [`packages/test-support/client-runtime/src/locale-env.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/locale-env.ts) | runtime implementation | Core file in the package named by the note: `packages/test-support/client-runtime`. Defines `usePinnedBrowserLanguages`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/locale`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/test-support/client-runtime`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `Config`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `FALLBACK_LOCALE` | `const` | [`packages/client/locale/src/client/index.ts:90`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/client/index.ts#L90) | `export const FALLBACK_LOCALE: LocaleId = 'zh'` |
| `LocaleRuntime` | `class` | [`packages/client/locale/src/client/index.ts:114`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/client/index.ts#L114) | `export class LocaleRuntime {` |
| `resolveInitialLocale` | `function` | [`packages/client/locale/src/client/index.ts:319`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/client/index.ts#L319) | `function resolveInitialLocale(): LocaleId {` |
| `detectBrowserLocale` | `function` | [`packages/client/locale/src/client/index.ts:332`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/client/index.ts#L332) | `function detectBrowserLocale(): LocaleId \| undefined {` |
| `locale` | `const` | [`packages/client/locale/src/client/index.ts:357`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/client/index.ts#L357) | `const locale = new LocaleRuntime(ctx, host)` |
| `language` | `const` | [`packages/client/ui-primitives/src/markdown/render.tsx:301`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/render.tsx#L301) | `const language = node.lang ?? undefined` |
| `preference` | `const` | [`packages/client/ui-theme/src/boot-theme.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/boot-theme.ts#L13) | `const preference = ${JSON.stringify(preference)}` |
| `window` | `const` | [`packages/fs/tool-fs/src/read.ts:147`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts#L147) | `const window = await buildWindow(` |
| `usePinnedBrowserLanguages` | `function` | [`packages/test-support/client-runtime/src/locale-env.ts:17`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/locale-env.ts#L17) | `export function usePinnedBrowserLanguages(primary: string, ...rest: string[]): void {` |
| `Config` | `interface` | [`vendor/hmr/src/index.ts:553`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L553) | `export interface Config extends ChokidarOptions {` |

### Tests and executable evidence

- [`apps/web/tests/support.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/support.ts) — The source note names this file directly.
- [`packages/client/locale/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `LocaleRuntime`. A test under the owning area exercises or imports `navigator`.
- [`packages/client/locale/tests/locale.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/tests/locale.client.spec.ts) — A test under the owning area exercises or imports `LocaleRuntime`. A test under the owning area exercises or imports `en-GB`.
- [`packages/client/ui-theme/tests/host.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/host.client.spec.ts) — A test under the owning area exercises or imports `preference`.
- [`packages/client/ui-theme/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `preference`.
- [`packages/client/ui-theme/tests/theme.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/theme.client.spec.ts) — A test under the owning area exercises or imports `preference`.
- [`packages/client/locale/tests/invariant.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/tests/invariant.client.spec.ts) — A test under the owning area exercises or imports `LocaleRuntime`.
- [`packages/client/ui-theme/tests/boot-theme.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/boot-theme.client.spec.ts) — A test under the owning area exercises or imports `preference`.

## How to read the implementation

1. Start with [`packages/client/locale/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/client/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** QuickJS-to-Rust capability registry.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`
- Aliases: `FALLBACK_LOCALE`, `LocaleRuntime`, `resolveInitialLocale`, `detectBrowserLocale`, `locale`, `language`, `preference`, `window`, `usePinnedBrowserLanguages`, `Config`, `dsh.locale`, `navigator.languages`, `packages/client/locale/src/client/index.ts`, `locale.preference`
- Regex: `(?i)(FALLBACK_LOCALE|LocaleRuntime|resolveInitialLocale|detectBrowserLocale|locale|language|preference|window)`

```bash
rg -n --pcre2 "(?i)(FALLBACK_LOCALE|LocaleRuntime|resolveInitialLocale|detectBrowserLocale|locale|language|preference|window)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0347. Persist Web user preferences through Host settings](0347-persist-web-user-preferences-through-host-settings.md): Shares source implementation: `packages/client/locale/src/client/index.ts`, `packages/client/locale/src/index.ts`.
- **`shares-code-with`** — [0510. Client Settings, Locale, and Theme layering](0510-client-settings-locale-and-theme-layering.md): Shares source implementation: `packages/client/locale/src/client/index.ts`, `packages/client/locale/src/index.ts`.
- **`shares-code-with`** — [0080. Full client copy rollout onto the typed locale seat, and the non-translation boundary](0080-full-client-copy-rollout-onto-the-typed-locale-seat-and-the-non-translat.md): Shares source implementation: `packages/test-support/client-runtime/src/index.ts`, `packages/test-support/client-runtime/src/invariant.ts`.
- **`shares-code-with`** — [0208. Ask-question Web presentation](0208-ask-question-web-presentation.md): Shares source implementation: `packages/client/locale/src/index.ts`, `packages/client/locale/src/invariant.ts`.
- **`shares-code-with`** — [0382. Classify Agent Notes by kind via path-encoded subdirectories](0382-classify-agent-notes-by-kind-via-path-encoded-subdirectories.md): Shares source implementation: `apps/web/tests/support.ts`.
- **`shares-code-with`** — [0687. Agent Notes](0687-agent-notes.md): Shares source implementation: `apps/web/tests/support.ts`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/evidence`.
- **`same-design-pressure`** — [0126. Repository naming contract and pre-release rename ledger](0126-repository-naming-contract-and-pre-release-rename-ledger.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/evidence`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0227-the-settings-language-a-fresh-browser-opens-in-comes-from-the-browser.md`.
