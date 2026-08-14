---
id: "dsh-note-0638"
title: "Run CI examples from built lib"
status: "archived"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/process/2026-07-17-run-ci-examples-from-built-lib.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/protocols"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/policy"
aliases:
  - "exports"
  - "json"
  - "lib"
  - "paths"
  - "node --import tsx"
  - "lib/"
  - "cordis.yml"
  - "examples/"
  - "examples/node_modules"
  - "examples/<agent>/"
  - "packages/<group>/<package>/"
  - "examples/<agent>/tests/fixtures/<group>/<package>/cordis.yml"
  - "examples/package.json"
  - "tsconfig.json"
search_regex: "(?i)(exports|json|paths|node[- ]\\-\\-import[- ]tsx|lib/|cordis\\.yml|examples/|examples/node_modules)"
---

# 0638. Run CI examples from built lib — implementation context

## Open this when

CI boots examples and Cordis-backed test projects through node --import tsx and the root tsconfig paths map. This adds TypeScript transformation cost and changes package resolution: imports resolve to workspace source instead of following package exports into built lib/. These runs therefore do not test the same code or resolution behavior as an installed consumer. A package can pass CI while its built export graph is incomplete or resolves differently.

## Source decision

Execution has two modes. src is the default local-development mode and uses tsx; lib is the strict CI mode and starts built bins with plain Node, without tsx or tsconfig path mapping. CI subprocesses that boot an example or a checked-in cordis.yml use lib mode. TypeScript fixtures that only implement an ACP or MCP peer and do not load Cordis run directly with Node. An explicit source-path regression may remain in src mode. Every test Cordis config must resolve its bare modules by walking upward from the config directory.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/process/2026-07-17-run-ci-examples-from-built-lib.md](../02-notes/archived/process/2026-07-17-run-ci-examples-from-built-lib.md)
- Pinned source: [.agents/notes/archived/process/2026-07-17-run-ci-examples-from-built-lib.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/process/2026-07-17-run-ci-examples-from-built-lib.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`examples/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/package.json) | composition and configuration | The source note names this file directly. | `named-file` |
| [`scripts/package-graph.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-graph.ts) | repository automation | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/client/tsdown.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/tsdown.client.ts) | runtime implementation | Defines `lib`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/tool-bash/src/render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/render.ts) | runtime implementation | Defines `paths`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/tool-pwsh/src/render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/src/render.ts) | runtime implementation | Defines `paths`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs-search/src/glob.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/glob.ts) | runtime implementation | Defines `paths`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/emitter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/emitter.ts) | runtime implementation | Defines `exports`, a construct named by the note. | `symbol-definition` |
| [`packages/workspace/workspace/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts) | package entry point | Defines `paths`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/analyzer.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts) | runtime implementation | Defines `exports`, a construct named by the note. | `symbol-definition` |
| [`packages/e2b/subprocess-e2b/src/terminal.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/terminal.ts) | runtime implementation | Defines `paths`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/client/system.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/client/system.ts) | runtime implementation | Defines `exports`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `exports` | `const` | [`packages/client/modules/src/client/system.ts:126`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/client/system.ts#L126) | `const exports = registered(this.makeRequire(edges))` |
| `exports` | `const` | [`packages/client/modules/src/client/system.ts:163`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/client/system.ts#L163) | `const exports = this.statics.get(specifier)` |
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `lib` | `const` | [`packages/client/tsdown.client.ts:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/tsdown.client.ts#L98) | `const lib = clientLibraryConfig(id, libEntry, options.lib)` |
| `lib` | `const` | [`packages/client/tsdown.client.ts:118`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/tsdown.client.ts#L118) | `const lib = clientLibraryConfig(id, libEntry)` |
| `json` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx:1516`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx#L1516) | `const json = parseJsonContainer(value)` |
| `json` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:851`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L851) | `const json = JSON.stringify(value, null, 2)` |
| `paths` | `const` | [`packages/e2b/subprocess-e2b/src/terminal.ts:466`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/terminal.ts#L466) | `const paths: TerminalPaths = {` |
| `json` | `const` | [`packages/extensions/cordis-host-runner/src/guard.ts:525`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/guard.ts#L525) | `const json = JSON.stringify(value)` |
| `paths` | `const` | [`packages/fs/tool-fs-search/src/glob.ts:365`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/glob.ts#L365) | `const paths = value.paths` |
| `paths` | `const` | [`packages/shell/tool-bash/src/render.ts:82`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/render.ts#L82) | `const paths = [read.stdoutSpillPath, read.stderrSpillPath].filter((path): path is string => path !== undefined)` |
| `paths` | `const` | [`packages/shell/tool-pwsh/src/render.ts:99`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/src/render.ts#L99) | `const paths = [read.stdoutSpillPath, read.stderrSpillPath].filter((path): path is string => path !== undefined)` |
| `exports` | `const` | [`packages/typert/generator/src/analyzer.ts:789`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L789) | `const exports = statement.exportClause === undefined` |
| `exports` | `const` | [`packages/typert/generator/src/emitter.ts:558`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/emitter.ts#L558) | `const exports = this.schemas.map((model): SchemaExport => ({` |
| `paths` | `const` | [`packages/workspace/workspace/src/index.ts:529`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts#L529) | `const paths = new Map<string, WorkspaceId>()` |
| `json` | `const` | [`scripts/package-graph.ts:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-graph.ts#L37) | `const json = JSON.parse(readFileSync(resolve(root, rel), 'utf8')) as {` |

### Tests and executable evidence

- [`scripts/clean.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/clean.spec.ts) — A test under the owning area exercises or imports `lib`. A test under the owning area exercises or imports `node_modules`.
- [`scripts/dev-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.spec.ts) — A test under the owning area exercises or imports `lib`. A test under the owning area exercises or imports `yml`.
- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `lib`. A test under the owning area exercises or imports `DSH_EXAMPLE_MODE`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `lib`. A test under the owning area exercises or imports `yml`.
- [`scripts/publint-all.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.spec.ts) — A test under the owning area exercises or imports `lib`.
- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — A test under the owning area exercises or imports `yml`. A test under the owning area exercises or imports `node_modules`.
- [`scripts/test-fixture-cleanup.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-fixture-cleanup.ts) — A test under the owning area exercises or imports `node_modules`.
- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — A test under the owning area exercises or imports `yml`. A test under the owning area exercises or imports `node_modules`.

## How to read the implementation

1. Start with [`examples/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/package.json) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
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

- Tags: `class/process`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/protocols`, `domain/storage`, `domain/testing`, `lifecycle/archived`, `mechanism/policy`
- Aliases: `exports`, `json`, `lib`, `paths`, `node --import tsx`, `lib/`, `cordis.yml`, `examples/`, `examples/node_modules`, `examples/<agent>/`, `packages/<group>/<package>/`, `examples/<agent>/tests/fixtures/<group>/<package>/cordis.yml`, `examples/package.json`, `tsconfig.json`
- Regex: `(?i)(exports|json|paths|node[- ]\-\-import[- ]tsx|lib/|cordis\.yml|examples/|examples/node_modules)`

```bash
rg -n --pcre2 "(?i)(exports|json|paths|node[- ]\\-\\-import[- ]tsx|lib/|cordis\\.yml|examples/|examples/node_modules)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0524. Discover package inventories instead of maintaining static lists](0524-discover-package-inventories-instead-of-maintaining-static-lists.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/fs/tool-fs-search/src/glob.ts`.
- **`shares-code-with`** — [0418. verify-cordis-config gates source-plane resolution of configured plugins](0418-verify-cordis-config-gates-source-plane-resolution-of-configured-plugins.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/tsdown.client.ts`.
- **`shares-code-with`** — [0380. TSC-first build and one compiler ownership](0380-tsc-first-build-and-one-compiler-ownership.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/typert/generator/src/analyzer.ts`.
- **`shares-code-with`** — [0441. Python public publication workflow](0441-python-public-publication-workflow.md): Shares source implementation: `packages/client/modules/src/index.ts`, `scripts/package-graph.ts`.
- **`shares-code-with`** — [0417. Oxlint as the repository linter](0417-oxlint-as-the-repository-linter.md): Shares source implementation: `packages/client/modules/src/index.ts`, `scripts/package-graph.ts`.
- **`shares-code-with`** — [0379. pnpm as the package manager instead of Yarn 4](0379-pnpm-as-the-package-manager-instead-of-yarn-4.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/typert/generator/src/analyzer.ts`.
- **`shares-code-with`** — [0377. Mechanical quality gates over prose guidelines](0377-mechanical-quality-gates-over-prose-guidelines.md): Shares source implementation: `packages/client/modules/src/index.ts`, `scripts/package-graph.ts`.
- **`shares-code-with`** — [0424. Exact uncovered locations on coverage failure](0424-exact-uncovered-locations-on-coverage-failure.md): Shares source implementation: `packages/client/modules/src/index.ts`, `scripts/package-graph.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0638-run-ci-examples-from-built-lib.md`.
