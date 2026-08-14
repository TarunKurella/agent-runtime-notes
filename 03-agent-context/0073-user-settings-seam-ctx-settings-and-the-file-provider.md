---
id: "dsh-note-0073"
title: "user-settings seam (`ctx.settings`) and the file provider"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-28-user-settings-seam.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/registry"
aliases:
  - "ctx"
  - "watch"
  - "settings"
  - "describe"
  - "publish"
  - "resolveSpec"
  - "SettingsProvider"
  - "base"
  - "effect"
  - "writable"
  - "raw"
  - "ctx.settings"
  - "packages/settings/"
  - "cordis.yml"
search_regex: "(?i)(watch|settings|describe|publish|resolveSpec|SettingsProvider|base|effect)"
---

# 0073. user-settings seam (`ctx.settings`) and the file provider — implementation context

## Open this when

User-editable configuration had no owner: dsh web read a cwd-anchored profile json through a static whitelist with no write path, the TUI read $DSH_HOME/config.yaml raw loader patches, and both froze at boot. A personal-settings page (web GUI) needs one cross-surface user layer with schema validation, a write path, and hot propagation --- and peer products (Codex, Claude Code, Kimi, OpenCode, Pi) all converged on separating user preferences from extension composition.

## Source decision

Two planes with a litmus test. cordis.yml (+ Include patches) stays the composition plane: which plugins exist, wiring, deployment config, owned by the orchestrator and upgraded with the product. A settings namespace carries only the user-editable subset; the test is "should the personal config page edit it?" Values live in both planes without ambiguity because layering is the contract: schema defaults, then the registrant's composition base (its entry-config subset), then the user document section. Three-package boundary mirroring session-persistence/.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-28-user-settings-seam.md](../02-notes/implemented/architecture/2026-07-28-user-settings-seam.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-28-user-settings-seam.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-28-user-settings-seam.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/bundle/base/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/index.ts) | package entry point | Core file in the package named by the note: `packages/bundle/base`. | `named-package-member` |
| [`packages/bundle/base/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/bundle/base`. | `named-package-member` |
| [`packages/client/ui-theme/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-theme`. Defines `settings`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/settings/settings/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/settings`. Core file in the package named by the note: `packages/settings/settings`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/settings/settings/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/settings/settings`. | `named-package-member` |
| [`packages/client/ui-theme/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-theme`. | `named-package-member` |
| [`packages/settings/settings/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/settings`. Core file in the package named by the note: `packages/settings/settings`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/settings/settings-file/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings-file/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/settings`. Core file in the package named by the note: `packages/settings/settings-file`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/settings/settings-file/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings-file/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/settings/settings-file`. | `named-package-member` |
| [`packages/settings/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/settings`. | `named-directory-member` |
| [`packages/settings`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/settings) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/bundle/base`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ctx` | `const` | [`apps/cli/src/profile-boot.ts:248`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L248) | `const ctx = await boot(NAME, rootConfig, structuredClone(allPatches(composed)), (hostCtx) => {` |
| `watch` | `const` | [`packages/client/hmr/src/index.ts:91`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts#L91) | `const watch = { path, mtimeMs: baseline.mtimeMs, size: baseline.size, dirty: false }` |
| `settings` | `const` | [`packages/client/ui-theme/src/index.ts:21`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/index.ts#L21) | `const settings = ctx.get('settings')` |
| `describe` | `function` | [`packages/core/tools/src/py-types.ts:223`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L223) | `function describe(schema: object): string \| undefined {` |
| `publish` | `const` | [`packages/extensions/ui-cordis/src/client/inventory.ts:64`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/inventory.ts#L64) | `const publish = (next: CordisInventorySnapshot): void => {` |
| `resolveSpec` | `function` | [`packages/settings/settings-file/src/index.ts:55`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings-file/src/index.ts#L55) | `export function resolveSpec(config: Config): ResolvedSpec {` |
| `SettingsProvider` | `class` | [`packages/settings/settings/src/index.ts:350`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts#L350) | `export abstract class SettingsProvider extends Service {` |
| `base` | `const` | [`packages/settings/settings/src/index.ts:490`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts#L490) | `const base = registration.base === undefined ? undefined : structuredClone(registration.base)` |
| `settings` | `const` | [`packages/settings/settings/src/invariant.ts:25`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/invariant.ts#L25) | `const settings = ctx.get('settings')` |
| `effect` | `const` | [`vendor/cordis/src/fiber.ts:366`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts#L366) | `const effect: Effect = runner.execute.call(this)` |
| `writable` | `const` | [`vendor/include/src/index.ts:27`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts#L27) | `const writable: Record<string, string> = {` |
| `raw` | `const` | [`vendor/loader/src/internal.ts:125`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/internal.ts#L125) | `const raw = requireInternal('internal/modules/esm/loader')?.getOrInitializeCascadedLoader()` |

### Tests and executable evidence

- [`packages/settings/settings/tests/memory.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/tests/memory.ts) — A test under the owning area exercises or imports `SettingsProvider`.
- [`packages/settings/settings/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/tests/settings.spec.ts) — A test under the owning area exercises or imports `SettingsProvider`.
- [`packages/client/ui-theme/tests/host.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/host.client.spec.ts) — A test under the owning area exercises or imports `dsh-settings`. A test under the owning area exercises or imports `SettingsProvider`.
- [`packages/settings/settings-file/tests/local.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings-file/tests/local.spec.ts) — A test under the owning area exercises or imports `resolveSpec`.
- [`packages/settings/settings-file/tests/loader-composition.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings-file/tests/loader-composition.spec.ts) — A test under the owning area exercises or imports `dsh-settings-file`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — Contains the exact code literal `dsh-settings` named by the note.
- [`apps/web/tests/default-model.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/default-model.e2e.ts) — Contains the exact code literal `dsh-settings` named by the note.
- [`apps/web/tests/declared-reasoning.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/declared-reasoning.e2e.ts) — Contains the exact code literal `dsh-settings` named by the note.

## How to read the implementation

1. Start with [`packages/bundle/base/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/registry`
- Aliases: `ctx`, `watch`, `settings`, `describe`, `publish`, `resolveSpec`, `SettingsProvider`, `base`, `effect`, `writable`, `raw`, `ctx.settings`, `packages/settings/`, `cordis.yml`
- Regex: `(?i)(watch|settings|describe|publish|resolveSpec|SettingsProvider|base|effect)`

```bash
rg -n --pcre2 "(?i)(watch|settings|describe|publish|resolveSpec|SettingsProvider|base|effect)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0060. dsh web config-tree boot and the web transport layering](0060-dsh-web-config-tree-boot-and-the-web-transport-layering.md): The source note links to this decision directly.
- **`source-link`** — [0086. settings write-path integrity and observer lifecycle](0086-settings-write-path-integrity-and-observer-lifecycle.md): The source note links to this decision directly.
- **`shares-code-with`** — [0510. Client Settings, Locale, and Theme layering](0510-client-settings-locale-and-theme-layering.md): Shares source implementation: `packages/client/ui-theme/src/index.ts`, `packages/client/ui-theme/src/invariant.ts`.
- **`shares-code-with`** — [0355. Broken presets are roster rows, not gaps](0355-broken-presets-are-roster-rows-not-gaps.md): Shares source implementation: `packages/settings/settings/src/index.ts`, `packages/settings/settings/src/invariant.ts`.
- **`shares-code-with`** — [0287. Plugin configuration in the web settings page](0287-plugin-configuration-in-the-web-settings-page.md): Shares source implementation: `packages/bundle/base/src/index.ts`, `packages/bundle/base/src/invariant.ts`.
- **`shares-code-with`** — [0095. One ordering for configuration sources, and what a discovered file may not decide](0095-one-ordering-for-configuration-sources-and-what-a-discovered-file-may-no.md): Shares source implementation: `packages/bundle/base/src/index.ts`, `packages/bundle/base/src/invariant.ts`.
- **`shares-code-with`** — [0077. request-level LLM configuration and the credential seam](0077-request-level-llm-configuration-and-the-credential-seam.md): Shares source implementation: `packages/bundle/base/src/index.ts`, `packages/bundle/base/src/invariant.ts`.
- **`shares-code-with`** — [0087. the web configuration plane](0087-the-web-configuration-plane.md): Shares source implementation: `packages/bundle/base/src/index.ts`, `packages/bundle/base/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0073-user-settings-seam-ctx-settings-and-the-file-provider.md`.
