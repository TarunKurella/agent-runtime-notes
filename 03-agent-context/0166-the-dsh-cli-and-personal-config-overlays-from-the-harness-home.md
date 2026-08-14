---
id: "dsh-note-0166"
title: "The dsh CLI and personal config overlays from the Harness home"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-20-dsh-cli-personal-config.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/generation"
  - "mechanism/registry"
aliases:
  - "patches"
  - "web"
  - "boot"
  - "defaultModel"
  - "dsh"
  - "disabled"
  - "providers"
  - "insert"
  - "yaml"
  - "resolveDshHome"
  - "entryListSchema"
  - "PatchOptions"
  - "config"
  - "examples/tui-agent/cordis.yml"
search_regex: "(?i)(patches|boot|defaultModel|disabled|providers|insert|yaml|resolveDshHome)"
---

# 0166. The dsh CLI and personal config overlays from the Harness home — implementation context

## Open this when

A developer's own preferences --- which provider and model the TUI uses, personal credentials, a private adapter route --- had nowhere to live except edits to committed files. Pointing the TUI demo at a personal Anthropic-proxy Opus route meant patching examples/tui-agent/cordis.yml and .env in the working tree, which risks committing secrets and repeats per checkout. There was also no installable command: running the agent in an arbitrary project directory required invoking the repo's demo script from the repo root.

## Source decision

The entry modes and the personal file's name and location below are superseded by the profile plugin bundles decision: dsh boots profiles, and the personal layer became the per-profile and home-level cordis.patch.yml. What survives unchanged is this note's substance --- the Harness home as the machine-level layer's root, patch semantics over a shipped composition, and fail-loud parsing. Two coupled pieces, aligned with the apps/ assembly tier proposed by the dsh web PR (#443): The dsh CLI (apps/cli, npm name @deepseek-ai/dsh). apps/ is the product-assembly tier over packages/ libraries.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-20-dsh-cli-personal-config.md](../02-notes/implemented/feature/2026-07-20-dsh-cli-personal-config.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-20-dsh-cli-personal-config.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-20-dsh-cli-personal-config.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `apps/cli`. The source note names this file directly. | `named-directory-member, named-file, named-package-member` |
| [`apps/cli/src/args.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts) | runtime implementation | Entry point or contract under the directory named by the note: `apps/cli`. Core file in the package named by the note: `apps/cli`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Core file in the package named by the note: `packages/boot/app-boot`. Defines `boot`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/boot/app-boot/src/profile.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/profile.ts) | runtime implementation | Core file in the package named by the note: `packages/boot/app-boot`. Defines `patches`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/util/home-paths/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/index.ts) | package entry point | Core file in the package named by the note: `packages/util/home-paths`. Defines `resolveDshHome`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/boot/app-boot/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/boot/app-boot`. | `named-package-member` |
| [`packages/util/home-paths/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/util/home-paths`. | `named-package-member` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `apps/cli`. Core file in the package named by the note: `apps/cli`. | `exact-code-occurrence, named-directory-member, named-package-member` |
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The source note names this implementation area directly. The note names this package or capability. | `named-directory, named-package` |
| [`apps/web`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/web) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/boot/app-boot`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/util/home-paths`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `patches` | `const` | [`apps/cli/src/args.ts:84`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts#L84) | `const patches = options.patch ?? []` |
| `web` | `const` | [`apps/cli/src/args.ts:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts#L156) | `const web = program.command('web').description('boot the web profile (alias of --profile web); the web app\'s own flags follow')` |
| `patches` | `const` | [`packages/boot/app-boot/src/index.ts:247`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L247) | `const patches = compose(userPatches)` |
| `boot` | `function` | [`packages/boot/app-boot/src/index.ts:757`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L757) | `export async function boot(` |
| `patches` | `const` | [`packages/boot/app-boot/src/profile.ts:399`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/profile.ts#L399) | `const patches = options.userLayer !== false && existsSync(patchPath)` |
| `defaultModel` | `const` | [`packages/bundle/headless/src/index.ts:101`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts#L101) | `const defaultModel = ctx.get('agentDefaultModel')` |
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |
| `disabled` | `const` | [`packages/client/ui-settings-plugins/src/client/BashCard.tsx:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/client/BashCard.tsx#L23) | `const disabled = !state.writable` |
| `providers` | `const` | [`packages/lsp/lsp-stdio/src/index.ts:141`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/index.ts#L141) | `const providers = await (async () => {` |
| `insert` | `const` | [`packages/session-query/session-query-sqlite/src/index.ts:584`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/src/index.ts#L584) | `const insert = db.prepare(\`` |
| `yaml` | `const` | [`packages/skill/skill-filesystem/src/index.ts:917`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts#L917) | `const yaml = raw.slice(start, closing.start)` |
| `resolveDshHome` | `function` | [`packages/util/home-paths/src/index.ts:87`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/index.ts#L87) | `export function resolveDshHome(configured?: string, env: Record<string, string \| undefined> = process.env): string {` |
| `entryListSchema` | `const` | [`vendor/include/src/index.ts:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts#L23) | `export const entryListSchema = yaml.JSON_SCHEMA.extend(JsExpr)` |
| `PatchOptions` | `interface` | [`vendor/include/src/index.ts:145`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts#L145) | `export interface PatchOptions {` |
| `config` | `const` | [`vendor/loader/src/index.ts:93`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts#L93) | `const config = next()` |

### Tests and executable evidence

- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — The source note names this file directly. A test under the owning area exercises or imports `$DSH_HOME`.
- [`packages/boot/app-boot/tests/user-patches.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/user-patches.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `dsh-app-boot`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `disabled`. A test under the owning area exercises or imports `dsh-app-boot`.
- [`apps/web/tests/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/README.zh.md) — A test under the owning area exercises or imports `boot`.
- [`apps/cli/tests/args.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/args.spec.ts) — A test under the owning area exercises or imports `patches`. A test under the owning area exercises or imports `providers`.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `$DSH_HOME`. A test under the owning area exercises or imports `DSH_AGENTS_HOME`.
- [`apps/web/tests/assembled-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/assembled-boot.ts) — A test under the owning area exercises or imports `boot`.
- [`apps/web/tests/default-model.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/default-model.e2e.ts) — A test under the owning area exercises or imports `disabled`. A test under the owning area exercises or imports `yaml`.
- Source verification intent: packages/boot/app-boot/tests/user-patches.spec.ts pins parsing, startup application, exact-path add/failure/recovery/removal, last-good rollback, failure broadcast, and preservation of app-owned patches. apps/cli/tests/built-bin.e2e.ts boots the real dsh bin over a profile and exercises the live patch layer end to end. Test launchers isolate $DSH_HOME, so a developer's real overlay cannot leak into fixtures.

## How to read the implementation

1. Start with [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

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

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/generation`, `mechanism/registry`
- Aliases: `patches`, `web`, `boot`, `defaultModel`, `dsh`, `disabled`, `providers`, `insert`, `yaml`, `resolveDshHome`, `entryListSchema`, `PatchOptions`, `config`, `examples/tui-agent/cordis.yml`
- Regex: `(?i)(patches|boot|defaultModel|disabled|providers|insert|yaml|resolveDshHome)`

```bash
rg -n --pcre2 "(?i)(patches|boot|defaultModel|disabled|providers|insert|yaml|resolveDshHome)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0059. One harness home resolver](0059-one-harness-home-resolver.md): The source note links to this decision directly.
- **`source-link`** — [0074. dsh source launch through the tsx ESM hook](0074-dsh-source-launch-through-the-tsx-esm-hook.md): The source note links to this decision directly.
- **`source-link`** — [0101. Profile plugin bundles replace the fixed surface overlays](0101-profile-plugin-bundles-replace-the-fixed-surface-overlays.md): The source note links to this decision directly.
- **`source-link`** — [0124. Loader interpolates the entry `disabled` field](0124-loader-interpolates-the-entry-disabled-field.md): The source note links to this decision directly.
- **`shares-code-with`** — [0553. Parse `dsh` argv through one Commander adapter](0553-parse-dsh-argv-through-one-commander-adapter.md): Shares source implementation: `apps/cli/package.json`, `apps/cli/src/args.ts`.
- **`shares-code-with`** — [0556. Native TypeScript source launch for dsh](0556-native-typescript-source-launch-for-dsh.md): Shares source implementation: `apps/cli/README.md`, `apps/cli/package.json`.
- **`shares-code-with`** — [0570. dsh tells the agent where its own source lives](0570-dsh-tells-the-agent-where-its-own-source-lives.md): Shares source implementation: `apps/cli/README.md`, `apps/cli/package.json`.
- **`shares-code-with`** — [0676. Explicit-config dsh entrypoint](0676-explicit-config-dsh-entrypoint.md): Shares source implementation: `apps/cli/README.md`, `apps/cli/package.json`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0166-the-dsh-cli-and-personal-config-overlays-from-the-harness-home.md`.
