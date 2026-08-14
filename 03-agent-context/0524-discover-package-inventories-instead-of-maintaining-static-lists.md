---
id: "dsh-note-0524"
title: "Discover package inventories instead of maintaining static lists"
status: "proposed"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/process/2026-06-20-discover-package-inventory.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/testing"
  - "lifecycle/proposed"
  - "mechanism/generation"
  - "mechanism/policy"
aliases:
  - "references"
  - "json"
  - "paths"
  - "scripts/publint-all.ts"
  - "packages/<group>/<pkg>"
  - "@deepseek-ai/dsh-*"
  - "tsconfig.host.json"
  - "tsconfig.client.json"
  - "package.json"
  - "gen-module-graph"
  - "gen-cordis-catalog"
  - "--check"
  - "doc-sync"
  - "knip.json"
search_regex: "(?i)(references|json|paths|scripts/publint\\-all\\.ts|packages/<group>/<pkg>|@deepseek\\-ai/dsh\\-\\*|tsconfig\\.host\\.json|tsconfig\\.client\\.json)"
---

# 0524. Discover package inventories instead of maintaining static lists — implementation context

## Open this when

Package and gate inventories are repeated across TypeScript project references, package docs, CI prose, and Knip overrides. Most restate package layout, manifest data, or aggregate command contents. Each new package therefore creates avoidable synchronization points. The package hierarchy already removed several of these by hand: scripts/publint-all.ts now derives its list from the packages// layout, and the two tsconfig paths maps collapsed to one @deepseek-ai/dsh- wildcard.

## Source decision

Make the remaining package/gate inventories discoverable. A single canonical source --- the packages// hierarchy plus package manifests --- should drive the aggregates' references, the module graph, and any other full-package list, with a generate-and-verify step (the existing gen-module-graph / gen-cordis-catalog pattern: a generator writes the artifact, a --check mode in hygiene/doc-sync fails on a stale committed copy). Module graph generation already reads package manifests.

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/process/2026-06-20-discover-package-inventory.md](../02-notes/proposed/process/2026-06-20-discover-package-inventory.md)
- Pinned source: [.agents/notes/proposed/process/2026-06-20-discover-package-inventory.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/process/2026-06-20-discover-package-inventory.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/publint-all.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/package-graph.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-graph.ts) | repository automation | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`scripts/gen-client-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-client-catalog.ts) | repository automation | Defines `references`, a construct named by the note. | `symbol-definition` |
| [`scripts/verify-cordis-config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-cordis-config.ts) | repository automation | Defines `references`, a construct named by the note. | `symbol-definition` |
| [`.github/issue-management/policy.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/issue-management/policy.mjs) | repository automation | Defines `references`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/tool-bash/src/render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/render.ts) | runtime implementation | Defines `paths`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/tool-pwsh/src/render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/src/render.ts) | runtime implementation | Defines `paths`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs-search/src/glob.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/glob.ts) | runtime implementation | Defines `paths`, a construct named by the note. | `symbol-definition` |
| [`packages/workspace/workspace/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts) | package entry point | Defines `paths`, a construct named by the note. | `symbol-definition` |
| [`packages/e2b/subprocess-e2b/src/terminal.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/terminal.ts) | runtime implementation | Defines `paths`, a construct named by the note. | `symbol-definition` |
| [`packages/context/session-reference/src/uri.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/uri.ts) | runtime implementation | Defines `references`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `references` | `const` | [`.github/issue-management/policy.mjs:599`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/issue-management/policy.mjs#L599) | `const references = parseReferences({` |
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `json` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx:1516`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx#L1516) | `const json = parseJsonContainer(value)` |
| `json` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:851`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L851) | `const json = JSON.stringify(value, null, 2)` |
| `references` | `const` | [`packages/context/session-reference/src/uri.ts:69`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/uri.ts#L69) | `const references: SessionReferenceInput[] = []` |
| `paths` | `const` | [`packages/e2b/subprocess-e2b/src/terminal.ts:466`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/terminal.ts#L466) | `const paths: TerminalPaths = {` |
| `json` | `const` | [`packages/extensions/cordis-host-runner/src/guard.ts:525`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/guard.ts#L525) | `const json = JSON.stringify(value)` |
| `paths` | `const` | [`packages/fs/tool-fs-search/src/glob.ts:365`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/glob.ts#L365) | `const paths = value.paths` |
| `paths` | `const` | [`packages/shell/tool-bash/src/render.ts:82`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/render.ts#L82) | `const paths = [read.stdoutSpillPath, read.stderrSpillPath].filter((path): path is string => path !== undefined)` |
| `paths` | `const` | [`packages/shell/tool-pwsh/src/render.ts:99`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/src/render.ts#L99) | `const paths = [read.stdoutSpillPath, read.stderrSpillPath].filter((path): path is string => path !== undefined)` |
| `paths` | `const` | [`packages/workspace/workspace/src/index.ts:529`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts#L529) | `const paths = new Map<string, WorkspaceId>()` |
| `references` | `const` | [`scripts/gen-client-catalog.ts:319`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-client-catalog.ts#L319) | `const references = referencedTypeNames(declarations.map(declaration => declaration.text), types)` |
| `json` | `const` | [`scripts/package-graph.ts:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-graph.ts#L37) | `const json = JSON.parse(readFileSync(resolve(root, rel), 'utf8')) as {` |
| `references` | `const` | [`scripts/verify-cordis-config.ts:280`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-cordis-config.ts#L280) | `const references = pluginReferences.filter(reference => reference.file.startsWith(\`${bundleDir}/\`))` |
| `references` | `const` | [`scripts/verify-cordis-config.ts:396`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-cordis-config.ts#L396) | `const references = (config.config as { references?: Array<{ path?: unknown }> }).references ?? []` |

### Tests and executable evidence

- [`vitest.e2e.config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vitest.e2e.config.ts) — A test under the owning area exercises or imports `tsconfig`.
- [`scripts/clean.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/clean.spec.ts) — A test under the owning area exercises or imports `tsconfig`.
- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`scripts/gen-doc-graphs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.spec.ts) — A test under the owning area exercises or imports `tsconfig`.
- [`scripts/client-tsconfig.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-tsconfig.spec.ts) — A test under the owning area exercises or imports `tsconfig`.
- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — A test under the owning area exercises or imports `tsconfig`.
- [`scripts/package-invariants.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-invariants.spec.ts) — A test under the owning area exercises or imports `tsconfig`.
- [`scripts/translation-pairing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing.spec.ts) — A test under the owning area exercises or imports `gen-cordis-catalog`.
- Source verification intent: Aggregate-config project references are generated from the hierarchy (a generator emits them; a --check gate fails when the committed copy is stale), rather than hand-maintained. Adding a package does not require editing a static package list for any gate. Docs describe the source of truth rather than repeating generated inventories. CI invokes the aggregate commands and lets those commands own their sub-gate lists. knip.json carries a per-package override only where it encodes real information (an extra entry file, an ignored dependency), never a restatement of the default stanza.

## How to read the implementation

1. Start with [`scripts/publint-all.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/testing`, `lifecycle/proposed`, `mechanism/generation`, `mechanism/policy`
- Aliases: `references`, `json`, `paths`, `scripts/publint-all.ts`, `packages/<group>/<pkg>`, `@deepseek-ai/dsh-*`, `tsconfig.host.json`, `tsconfig.client.json`, `package.json`, `gen-module-graph`, `gen-cordis-catalog`, `--check`, `doc-sync`, `knip.json`
- Regex: `(?i)(references|json|paths|scripts/publint\-all\.ts|packages/<group>/<pkg>|@deepseek\-ai/dsh\-\*|tsconfig\.host\.json|tsconfig\.client\.json)`

```bash
rg -n --pcre2 "(?i)(references|json|paths|scripts/publint\\-all\\.ts|packages/<group>/<pkg>|@deepseek\\-ai/dsh\\-\\*|tsconfig\\.host\\.json|tsconfig\\.client\\.json)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0547. Reorganize packages into a modular hierarchy](0547-reorganize-packages-into-a-modular-hierarchy.md): The source note links to this decision directly.
- **`shares-code-with`** — [0418. verify-cordis-config gates source-plane resolution of configured plugins](0418-verify-cordis-config-gates-source-plane-resolution-of-configured-plugins.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/fs/tool-fs-search/src/glob.ts`.
- **`shares-code-with`** — [0638. Run CI examples from built lib](0638-run-ci-examples-from-built-lib.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/fs/tool-fs-search/src/glob.ts`.
- **`shares-code-with`** — [0424. Exact uncovered locations on coverage failure](0424-exact-uncovered-locations-on-coverage-failure.md): Shares source implementation: `packages/client/modules/src/index.ts`, `scripts/package-graph.ts`.
- **`shares-code-with`** — [0441. Python public publication workflow](0441-python-public-publication-workflow.md): Shares source implementation: `packages/client/modules/src/index.ts`, `scripts/package-graph.ts`.
- **`shares-code-with`** — [0417. Oxlint as the repository linter](0417-oxlint-as-the-repository-linter.md): Shares source implementation: `packages/client/modules/src/index.ts`, `scripts/package-graph.ts`.
- **`shares-code-with`** — [0377. Mechanical quality gates over prose guidelines](0377-mechanical-quality-gates-over-prose-guidelines.md): Shares source implementation: `packages/client/modules/src/index.ts`, `scripts/package-graph.ts`.
- **`shares-code-with`** — [0640. doc-sync through the gate scheduler](0640-doc-sync-through-the-gate-scheduler.md): Shares source implementation: `packages/client/modules/src/index.ts`, `scripts/package-graph.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0524-discover-package-inventories-instead-of-maintaining-static-lists.md`.
