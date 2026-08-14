---
id: "dsh-note-0670"
title: "Consolidate gate scripts on already-present deps and builtins"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-07-26-consolidate-gate-scripts-on-existing-deps.md"
implementation_evidence: "high"
target_anchor: "repository tests and release policy"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/filesystem"
  - "lifecycle/archived"
  - "mechanism/policy"
aliases:
  - "code"
  - "parseArgs"
  - "parseOptions"
  - "discoverPluginDirs"
  - "markdownFences"
  - "markdownProseLines"
  - "addPath"
  - "listSources"
  - "realPackageNames"
  - "extractEquivBlocks"
  - "scripts/"
  - "globSync"
  - "scripts/md-fences.ts"
  - "doc-typecheck.ts"
search_regex: "(?i)(code|parseArgs|parseOptions|discoverPluginDirs|markdownFences|markdownProseLines|addPath|listSources)"
---

# 0670. Consolidate gate scripts on already-present deps and builtins — implementation context

## Open this when

The scripts/ gates mostly used the right tools (node:fs globSync in 15+ gates, mdast/micromark in the markdown gates), but a handful of stragglers hand-rolled what a sibling gate already did with an existing dependency or builtin: Duplicated fence scanners. scripts/md-fences.ts (~55 lines, consumed by doc-typecheck.ts) and extractEquivBlocks in scripts/verify-type-equiv.ts (~39 lines) were two copies of the same regex line-scanner for fenced code blocks, while scripts/verify-mermaid.ts already extracted fences by visiting mdast code nodes --- and markdownProseLines in scripts/markdown.ts itself parsed to mdast.

## Source decision

A shared mdast fence helper, markdownFences in scripts/markdown.ts, visits code nodes for the language, full info string, body, and 1-based opening-fence line; doc-typecheck.ts and verify-type-equiv.ts extract fences through it. md-fences.ts and the duplicated extractEquivBlocks scanner are deleted, and markdownProseLines derives fenced lines from the parsed code nodes' positions instead of a second regex. Both CLIs parse argv via parseArgs; unknown options and missing values still fail loud, with parseArgs's own error text instead of the bespoke usage strings. The five straggler walks use globSync.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-07-26-consolidate-gate-scripts-on-existing-deps.md](../02-notes/archived/simplification/2026-07-26-consolidate-gate-scripts-on-existing-deps.md)
- Pinned source: [.agents/notes/archived/simplification/2026-07-26-consolidate-gate-scripts-on-existing-deps.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-07-26-consolidate-gate-scripts-on-existing-deps.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/markdown.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/markdown.ts) | repository automation | The source note names this file directly. Defines `markdownProseLines`, a construct named by the note. | `named-file, symbol-definition` |
| [`scripts/publint-all.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.ts) | repository automation | The source note names this file directly. Defines `addPath`, a construct named by the note. | `named-file, symbol-definition` |
| [`scripts/verify-mermaid.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-mermaid.ts) | repository automation | The source note names this file directly. Contains the exact code literal `scripts/verify-mermaid.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`scripts/verify-type-equiv.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-type-equiv.ts) | repository automation | The source note names this file directly. Defines `extractEquivBlocks`, a construct named by the note. | `named-file, symbol-definition` |
| [`scripts/package-invariants.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-invariants.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/verify-built-package-invariants.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-built-package-invariants.mjs) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/dev-web.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.ts) | repository automation | Defines `discoverPluginDirs`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/src/plugin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts) | runtime implementation | Defines `code`, a construct named by the note. | `symbol-definition` |
| [`scripts/change-scope.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/change-scope.ts) | repository automation | Defines `parseOptions`, a construct named by the note. | `symbol-definition` |
| [`vendor/include/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts) | package entry point | Defines `code`, a construct named by the note. | `symbol-definition` |
| [`vendor/cosmokit/src/string.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cosmokit/src/string.ts) | runtime implementation | Defines `code`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts) | package entry point | Defines `code`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `code` | `const` | [`apps/cli/src/plugin.ts:135`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts#L135) | `const code = (result.error as NodeJS.ErrnoException).code` |
| `code` | `const` | [`packages/client/hmr/src/index.ts:70`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts#L70) | `const code = (error as NodeJS.ErrnoException).code` |
| `parseArgs` | `function` | [`packages/client/ui-tool/src/client/tool/models/tool-call-model.ts:119`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/models/tool-call-model.ts#L119) | `function parseArgs(argsRaw: string): unknown {` |
| `parseArgs` | `function` | [`packages/extensions/ui-cordis/src/client/card-model.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/card-model.ts#L61) | `function parseArgs(argsRaw: string): Record<string, unknown> \| null {` |
| `code` | `const` | [`packages/goal/goal/src/index.ts:170`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L170) | `const code = record?.['code']` |
| `parseArgs` | `function` | [`packages/sandbox/sandbox-windows-acl/src/runner.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/runner.ts#L75) | `function parseArgs(raw: string[]): ParsedArgs {` |
| `parseOptions` | `function` | [`scripts/change-scope.ts:102`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/change-scope.ts#L102) | `function parseOptions(args: string[]): ChangeScopeOptions {` |
| `discoverPluginDirs` | `function` | [`scripts/dev-web.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.ts#L36) | `export function discoverPluginDirs(root = repoRoot): string[] {` |
| `markdownFences` | `function` | [`scripts/markdown.ts:64`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/markdown.ts#L64) | `export function markdownFences(source: string): MarkdownFence[] {` |
| `markdownProseLines` | `function` | [`scripts/markdown.ts:160`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/markdown.ts#L160) | `export function markdownProseLines(source: string): MarkdownProseLine[] {` |
| `addPath` | `function` | [`scripts/publint-all.ts:92`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.ts#L92) | `function addPath(path: string, paths: Set<string>): void {` |
| `listSources` | `function` | [`scripts/verify-client-domain-graph.ts:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-client-domain-graph.ts#L31) | `function listSources(dir: string): string[] {` |
| `realPackageNames` | `function` | [`scripts/verify-package-paths.ts:42`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-package-paths.ts#L42) | `function realPackageNames(): Set<string> {` |
| `extractEquivBlocks` | `function` | [`scripts/verify-type-equiv.ts:84`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-type-equiv.ts#L84) | `function extractEquivBlocks(docRel: string): EquivBlock[] {` |
| `code` | `const` | [`vendor/cosmokit/src/string.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cosmokit/src/string.ts#L26) | `const code = source.charCodeAt(i)` |
| `code` | `const` | [`vendor/include/src/index.ts:39`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts#L39) | `const code = (error as NodeJS.ErrnoException \| null)?.code` |

### Tests and executable evidence

- [`scripts/dev-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.spec.ts) — A test under the owning area exercises or imports `discoverPluginDirs`.
- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `doc-typecheck`. A test under the owning area exercises or imports `publint`.
- [`scripts/publint-all.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.spec.ts) — A test under the owning area exercises or imports `publint`.
- [`scripts/coverage-exempt.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/coverage-exempt.spec.ts) — A test under the owning area exercises or imports `globSync`.
- [`scripts/client-tsconfig.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-tsconfig.spec.ts) — A test under the owning area exercises or imports `readdirSync`.
- [`scripts/test-fixture-cleanup.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-fixture-cleanup.ts) — A test under the owning area exercises or imports `readdirSync`.
- [`scripts/project-doc-site.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.spec.ts) — A test under the owning area exercises or imports `globSync`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `readdirSync`.

## How to read the implementation

1. Start with [`scripts/markdown.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/markdown.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and release policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/evidence`, `concern/ownership`, `domain/build-release`, `domain/filesystem`, `lifecycle/archived`, `mechanism/policy`
- Aliases: `code`, `parseArgs`, `parseOptions`, `discoverPluginDirs`, `markdownFences`, `markdownProseLines`, `addPath`, `listSources`, `realPackageNames`, `extractEquivBlocks`, `scripts/`, `globSync`, `scripts/md-fences.ts`, `doc-typecheck.ts`
- Regex: `(?i)(code|parseArgs|parseOptions|discoverPluginDirs|markdownFences|markdownProseLines|addPath|listSources)`

```bash
rg -n --pcre2 "(?i)(code|parseArgs|parseOptions|discoverPluginDirs|markdownFences|markdownProseLines|addPath|listSources)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0637. Parallel GitHub CI gates](0637-parallel-github-ci-gates.md): Shares source implementation: `scripts/publint-all.ts`, `scripts/verify-built-package-invariants.mjs`.
- **`shares-code-with`** — [0439. Private npm publication as three independent sequences](0439-private-npm-publication-as-three-independent-sequences.md): Shares source implementation: `apps/cli/src/plugin.ts`, `vendor/include/src/index.ts`.
- **`shares-code-with`** — [0478. Copy-only preset authoring, and the way into a preset's files](0478-copy-only-preset-authoring-and-the-way-into-a-preset-s-files.md): Shares source implementation: `apps/cli/src/plugin.ts`, `vendor/include/src/index.ts`.
- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `scripts/publint-all.ts`, `scripts/verify-built-package-invariants.mjs`.
- **`shares-code-with`** — [0005. Runtime arg validation at the model boundary](0005-runtime-arg-validation-at-the-model-boundary.md): Shares source implementation: `apps/cli/src/plugin.ts`, `vendor/include/src/index.ts`.
- **`shares-code-with`** — [0388. Export JSDoc gate](0388-export-jsdoc-gate.md): Shares source implementation: `vendor/include/src/index.ts`.
- **`shares-code-with`** — [0383. Subsystems catalog and the `ts type-equiv` drift gate](0383-subsystems-catalog-and-the-ts-type-equiv-drift-gate.md): Shares source implementation: `scripts/verify-type-equiv.ts`.
- **`shares-code-with`** — [0656. Fold the stdio UI helper into the stdio app](0656-fold-the-stdio-ui-helper-into-the-stdio-app.md): Shares source implementation: `vendor/include/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0670-consolidate-gate-scripts-on-already-present-deps-and-builtins.md`.
