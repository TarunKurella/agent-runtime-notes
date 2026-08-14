---
id: "dsh-note-0418"
title: "verify-cordis-config gates source-plane resolution of configured plugins"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-30-cordis-config-source-plane-resolution-gate.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "plugin"
  - "exports"
  - "json"
  - "lib"
  - "paths"
  - "validateSourcePlaneResolution"
  - "apps/cli/config/tui.cordis.yml"
  - "@deepseek-ai/dsh-tui/prompt"
  - "@deepseek-ai/dsh-*"
  - "tui/prompt"
  - "<group>/*/src"
  - "lib/prompt.js"
  - "lib/"
  - "DSH_EXAMPLE_MODE=lib"
search_regex: "(?i)(plugin|exports|json|paths|validateSourcePlaneResolution|apps/cli/config/tui\\.cordis\\.yml|@deepseek\\-ai/dsh\\-tui/prompt|@deepseek\\-ai/dsh\\-\\*)"
---

# 0418. verify-cordis-config gates source-plane resolution of configured plugins — implementation context

## Open this when

apps/cli/config/tui.cordis.yml gained the @deepseek-ai/dsh-tui/prompt entry without a matching tsconfig paths mapping. The generic @deepseek-ai/dsh- wildcard substitutes tui/prompt whole into its //src candidates, none of which exist, so the tsx source launch fell back to package exports and resolved lib/prompt.js --- an artifact-plane file.

## Source decision

scripts/verify-cordis-config.ts (validateSourcePlaneResolution) requires every configured specifier of a local workspace package --- harness packages and vendored Cordis alike --- to resolve through the tsconfig.base.json paths facade to a .ts/.tsx source file, using ts.resolveModuleName from the repository root. A failed resolution or a .d.ts hit (the exports fallback into built lib/types) fails verify-cordis-config, naming the config files and the specifier. The missing @deepseek-ai/dsh-tui/prompt mapping is added next to the other explicit subpath entries; removing it reproduces the gate failure.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-30-cordis-config-source-plane-resolution-gate.md](../02-notes/implemented/process/2026-07-30-cordis-config-source-plane-resolution-gate.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-30-cordis-config-source-plane-resolution-gate.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-30-cordis-config-source-plane-resolution-gate.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/verify-cordis-config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-cordis-config.ts) | repository automation | The source note names this file directly. Defines `validateSourcePlaneResolution`, a construct named by the note. | `exact-code-occurrence, named-file, symbol-definition` |
| [`apps/cli/src/args.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts) | runtime implementation | Defines `plugin`, a construct named by the note. | `symbol-definition` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `plugin`, a construct named by the note. | `symbol-definition` |
| [`scripts/package-graph.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-graph.ts) | repository automation | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`vendor/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts) | package entry point | Defines `plugin`, a construct named by the note. | `symbol-definition` |
| [`packages/client/tsdown.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/tsdown.client.ts) | runtime implementation | Defines `lib`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/tool-bash/src/render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/render.ts) | runtime implementation | Defines `paths`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/tool-pwsh/src/render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/src/render.ts) | runtime implementation | Defines `paths`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs-search/src/glob.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/glob.ts) | runtime implementation | Defines `paths`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/emitter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/emitter.ts) | runtime implementation | Defines `exports`, a construct named by the note. | `symbol-definition` |
| [`packages/workspace/workspace/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts) | package entry point | Defines `paths`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `plugin` | `const` | [`apps/cli/src/args.ts:171`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts#L171) | `const plugin = program.command('plugin').description('manage a profile\'s plugins by forwarding the remaining arguments to pnpm in the profile directory')` |
| `exports` | `const` | [`packages/client/modules/src/client/system.ts:126`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/client/system.ts#L126) | `const exports = registered(this.makeRequire(edges))` |
| `exports` | `const` | [`packages/client/modules/src/client/system.ts:163`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/client/system.ts#L163) | `const exports = this.statics.get(specifier)` |
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `lib` | `const` | [`packages/client/tsdown.client.ts:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/tsdown.client.ts#L98) | `const lib = clientLibraryConfig(id, libEntry, options.lib)` |
| `lib` | `const` | [`packages/client/tsdown.client.ts:118`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/tsdown.client.ts#L118) | `const lib = clientLibraryConfig(id, libEntry)` |
| `json` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx:1516`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx#L1516) | `const json = parseJsonContainer(value)` |
| `json` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:851`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L851) | `const json = JSON.stringify(value, null, 2)` |
| `paths` | `const` | [`packages/e2b/subprocess-e2b/src/terminal.ts:466`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/terminal.ts#L466) | `const paths: TerminalPaths = {` |
| `json` | `const` | [`packages/extensions/cordis-host-runner/src/guard.ts:525`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/guard.ts#L525) | `const json = JSON.stringify(value)` |
| `plugin` | `const` | [`packages/extensions/tool-cordis/src/index.ts:126`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/src/index.ts#L126) | `const plugin = ctx.dynamicCordisRunner.inspectPlugin(agent, pluginId)` |
| `plugin` | `const` | [`packages/extensions/tool-cordis/src/index.ts:218`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/src/index.ts#L218) | `const plugin = args.plugin.kind === 'new'` |
| `paths` | `const` | [`packages/fs/tool-fs-search/src/glob.ts:365`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/glob.ts#L365) | `const paths = value.paths` |
| `paths` | `const` | [`packages/shell/tool-bash/src/render.ts:82`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/render.ts#L82) | `const paths = [read.stdoutSpillPath, read.stderrSpillPath].filter((path): path is string => path !== undefined)` |
| `paths` | `const` | [`packages/shell/tool-pwsh/src/render.ts:99`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/src/render.ts#L99) | `const paths = [read.stdoutSpillPath, read.stderrSpillPath].filter((path): path is string => path !== undefined)` |
| `exports` | `const` | [`packages/typert/generator/src/analyzer.ts:789`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L789) | `const exports = statement.exportClause === undefined` |

### Tests and executable evidence

- [`scripts/clean.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/clean.spec.ts) — A test under the owning area exercises or imports `lib`. Contains the exact code literal `lib/types` named by the note.
- [`scripts/dev-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.spec.ts) — A test under the owning area exercises or imports `lib`.
- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `lib`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `lib`.
- [`scripts/publint-all.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.spec.ts) — A test under the owning area exercises or imports `lib`.
- [`scripts/client-bundle-css.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-css.spec.ts) — A test under the owning area exercises or imports `lib`.
- [`scripts/package-invariants.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-invariants.spec.ts) — A test under the owning area exercises or imports `lib`.
- [`scripts/translation-pairing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing.spec.ts) — A test under the owning area exercises or imports `lib`.

## How to read the implementation

1. Start with [`scripts/verify-cordis-config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-cordis-config.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/process`, `concern/discovery-routing`, `concern/evidence`, `concern/recovery`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `plugin`, `exports`, `json`, `lib`, `paths`, `validateSourcePlaneResolution`, `apps/cli/config/tui.cordis.yml`, `@deepseek-ai/dsh-tui/prompt`, `@deepseek-ai/dsh-*`, `tui/prompt`, `<group>/*/src`, `lib/prompt.js`, `lib/`, `DSH_EXAMPLE_MODE=lib`
- Regex: `(?i)(plugin|exports|json|paths|validateSourcePlaneResolution|apps/cli/config/tui\.cordis\.yml|@deepseek\-ai/dsh\-tui/prompt|@deepseek\-ai/dsh\-\*)`

```bash
rg -n --pcre2 "(?i)(plugin|exports|json|paths|validateSourcePlaneResolution|apps/cli/config/tui\\.cordis\\.yml|@deepseek\\-ai/dsh\\-tui/prompt|@deepseek\\-ai/dsh\\-\\*)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0074. dsh source launch through the tsx ESM hook](0074-dsh-source-launch-through-the-tsx-esm-hook.md): The source note links to this decision directly.
- **`shares-code-with`** — [0524. Discover package inventories instead of maintaining static lists](0524-discover-package-inventories-instead-of-maintaining-static-lists.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/fs/tool-fs-search/src/glob.ts`.
- **`shares-code-with`** — [0638. Run CI examples from built lib](0638-run-ci-examples-from-built-lib.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/tsdown.client.ts`.
- **`shares-code-with`** — [0424. Exact uncovered locations on coverage failure](0424-exact-uncovered-locations-on-coverage-failure.md): Shares source implementation: `packages/client/modules/src/index.ts`, `scripts/package-graph.ts`.
- **`shares-code-with`** — [0573. TUI banner brand gradient](0573-tui-banner-brand-gradient.md): Shares source implementation: `vendor/hmr/src/index.ts`, `vendor/loader/src/index.ts`.
- **`shares-code-with`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): Shares source implementation: `vendor/hmr/src/index.ts`, `vendor/loader/src/index.ts`.
- **`shares-code-with`** — [0439. Private npm publication as three independent sequences](0439-private-npm-publication-as-three-independent-sequences.md): Shares source implementation: `vendor/hmr/src/index.ts`, `vendor/loader/src/index.ts`.
- **`shares-code-with`** — [0377. Mechanical quality gates over prose guidelines](0377-mechanical-quality-gates-over-prose-guidelines.md): Shares source implementation: `packages/client/modules/src/index.ts`, `scripts/package-graph.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0418-verify-cordis-config-gates-source-plane-resolution-of-configured-plugins.md`.
