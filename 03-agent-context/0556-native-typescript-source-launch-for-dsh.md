---
id: "dsh-note-0556"
title: "Native TypeScript source launch for dsh"
status: "archived"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/architecture/2026-07-28-dsh-native-typescript-source-launch.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/observability"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "plugin"
  - "dependencies"
  - "json"
  - "dsh"
  - "lib"
  - "paths"
  - "require"
  - "--experimental-transform-types"
  - "verify-cordis-config"
  - "apps/cli/src/bin.ts"
  - "lib/"
  - "cordis.yml"
  - "node --experimental-transform-types"
  - "bin/dsh"
search_regex: "(?i)(plugin|dependencies|json|paths|require|\\-\\-experimental\\-transform\\-types|verify\\-cordis\\-config|apps/cli/src/bin\\.ts)"
---

# 0556. Native TypeScript source launch for dsh — implementation context

## Open this when

The dsh source entry point originally used tsx to run apps/cli/src/bin.ts, with the same third-party loader implicitly handling both TypeScript transformation and the root tsconfig's paths resolution. With Node handling TypeScript natively, it does not apply tsconfig path mappings; resolving through package exports would instead mix potentially stale or nonexistent lib/ artifacts into the source launch. Node's transform also does not perform type analysis.

## Source decision

The dsh TUI, Web, and headless source launches use node --experimental-transform-types; Node performs TypeScript transformation without loading tsx or esbuild. bin/dsh, the root-level dsh/TUI/Web demos, and Code Mode TUI enter the same apps/cli/src/bin.ts launch chain. Test and E2E launchers retain their existing strategies, and the built lib/bin.js continues to run under ordinary Node. scripts/tspath-loader.ts registers only a module resolve hook.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/architecture/2026-07-28-dsh-native-typescript-source-launch.md](../02-notes/archived/architecture/2026-07-28-dsh-native-typescript-source-launch.md)
- Pinned source: [.agents/notes/archived/architecture/2026-07-28-dsh-native-typescript-source-launch.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/architecture/2026-07-28-dsh-native-typescript-source-launch.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`apps/cli/src/bin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/bin.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `apps/cli`. The source note names this file directly. | `exact-code-occurrence, named-directory-member, named-file, named-package-member` |
| [`apps/cli/src/args.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts) | runtime implementation | Entry point or contract under the directory named by the note: `apps/cli`. Core file in the package named by the note: `apps/cli`. | `named-directory-member, named-package-member, symbol-definition` |
| [`apps/cli/src/plugin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts) | runtime implementation | Entry point or contract under the directory named by the note: `apps/cli`. Core file in the package named by the note: `apps/cli`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Core file in the package named by the note: `packages/boot/app-boot`. | `named-package-member` |
| [`packages/boot/app-boot/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/boot/app-boot`. | `named-package-member` |
| [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `apps/cli`. Core file in the package named by the note: `apps/cli`. | `named-directory-member, named-package-member` |
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The source note names this implementation area directly. The note names this package or capability. | `named-directory, named-package` |
| [`packages/boot/app-boot`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `require`, a construct named by the note. | `symbol-definition` |
| [`packages/client/tsdown.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/tsdown.client.ts) | runtime implementation | Defines `lib`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `plugin` | `const` | [`apps/cli/src/args.ts:171`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts#L171) | `const plugin = program.command('plugin').description('manage a profile\'s plugins by forwarding the remaining arguments to pnpm in the profile directory')` |
| `dependencies` | `const` | [`apps/cli/src/plugin.ts:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts#L62) | `const dependencies = Object.keys(after.dependencies ?? {})` |
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |
| `lib` | `const` | [`packages/client/tsdown.client.ts:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/tsdown.client.ts#L98) | `const lib = clientLibraryConfig(id, libEntry, options.lib)` |
| `paths` | `const` | [`packages/shell/tool-bash/src/render.ts:82`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/render.ts#L82) | `const paths = [read.stdoutSpillPath, read.stderrSpillPath].filter((path): path is string => path !== undefined)` |
| `require` | `const` | [`vendor/hmr/src/index.ts:463`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L463) | `const require = createRequire(import.meta.url)` |

### Tests and executable evidence

- [`apps/cli/tests/windows-shell.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/windows-shell.spec.ts) — A test under the owning area exercises or imports `dsh-app-boot`. Contains the exact code literal `dsh-app-boot` named by the note.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `dsh-app-boot`. Contains the exact code literal `apps/cli/package.json` named by the note.
- [`apps/cli/tests/memory-mcp-configs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/memory-mcp-configs.spec.ts) — A test under the owning area exercises or imports `dsh-app-boot`.
- [`packages/boot/app-boot/tests/profile.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/profile.spec.ts) — A test under the owning area exercises or imports `dsh-app-boot`.
- [`apps/cli/tests/fixtures/dsh-badge/snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/dsh-badge/snapshot.ts) — A test under the owning area exercises or imports `dsh-app-boot`.
- [`packages/boot/app-boot/tests/user-patches.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/user-patches.spec.ts) — A test under the owning area exercises or imports `dsh-app-boot`.
- [`vitest.e2e.config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vitest.e2e.config.ts) — Contains the exact code literal `apps/cli` named by the note.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — Contains the exact code literal `apps/cli/package.json` named by the note. Contains the exact code literal `dsh-app-boot` named by the note.

## How to read the implementation

1. Start with [`vendor/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/README.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/observability`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/policy`, `mechanism/projection`
- Aliases: `plugin`, `dependencies`, `json`, `dsh`, `lib`, `paths`, `require`, `--experimental-transform-types`, `verify-cordis-config`, `apps/cli/src/bin.ts`, `lib/`, `cordis.yml`, `node --experimental-transform-types`, `bin/dsh`
- Regex: `(?i)(plugin|dependencies|json|paths|require|\-\-experimental\-transform\-types|verify\-cordis\-config|apps/cli/src/bin\.ts)`

```bash
rg -n --pcre2 "(?i)(plugin|dependencies|json|paths|require|\\-\\-experimental\\-transform\\-types|verify\\-cordis\\-config|apps/cli/src/bin\\.ts)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0570. dsh tells the agent where its own source lives](0570-dsh-tells-the-agent-where-its-own-source-lives.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0553. Parse `dsh` argv through one Commander adapter](0553-parse-dsh-argv-through-one-commander-adapter.md): Shares source implementation: `apps/cli/package.json`, `apps/cli/src/args.ts`.
- **`shares-code-with`** — [0166. The dsh CLI and personal config overlays from the Harness home](0166-the-dsh-cli-and-personal-config-overlays-from-the-harness-home.md): Shares source implementation: `apps/cli/README.md`, `apps/cli/package.json`.
- **`shares-code-with`** — [0597. `dsh meta` boots the TUI over the harness checkout](0597-dsh-meta-boots-the-tui-over-the-harness-checkout.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0527. Artifact-first NPM baseline publication](0527-artifact-first-npm-baseline-publication.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0676. Explicit-config dsh entrypoint](0676-explicit-config-dsh-entrypoint.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0485. Source run without a managed installer](0485-source-run-without-a-managed-installer.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0579. Product-level TUI session resume](0579-product-level-tui-session-resume.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0556-native-typescript-source-launch-for-dsh.md`.
