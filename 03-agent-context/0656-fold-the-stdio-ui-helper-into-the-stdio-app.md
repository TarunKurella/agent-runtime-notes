---
id: "dsh-note-0656"
title: "Fold the stdio UI helper into the stdio app"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-07-04-fold-stdio-ui-helper.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/protocols"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/policy"
aliases:
  - "inject"
  - "apply"
  - "Config"
  - "@deepseek-ai/dsh-ui-stdio"
  - "packages/support/"
  - "@deepseek-ai/dsh-stdio-demo"
  - "ui/"
  - "@deepseek-ai/dsh-stdio"
  - "createStdioChat"
  - "StdioRuntime"
  - "unwrapExports"
  - "cordis.yml"
  - "Fold the stdio UI helper into the stdio app"
  - "simplification"
search_regex: "(?i)(inject|apply|Config|@deepseek\\-ai/dsh\\-ui\\-stdio|packages/support/|@deepseek\\-ai/dsh\\-stdio\\-demo|@deepseek\\-ai/dsh\\-stdio|createStdioChat)"
---

# 0656. Fold the stdio UI helper into the stdio app — implementation context

## Open this when

The readline UI was a whole package (@deepseek-ai/dsh-ui-stdio under packages/support/) whose only runtime importer was the app package @deepseek-ai/dsh-stdio-demo. The examples reach the readline UI by loading the app, never by composing the helper themselves; every other repo reference was mechanical or descriptive surface that existed BECAUSE the package boundary existed --- manifest and tsconfig entries, generated module-graph rows, dependency-graph and README rows, and doc comments naming the package.

## Source decision

At the time, the helper moved into @deepseek-ai/dsh-stdio as the terminal-channel plugin. createStdioChat, its StdioRuntime test seam, and its unit tests moved with it, keeping EOF handling, rendering, disposal, and piped-vs-TTY behavior under the per-file coverage gate without hijacking process globals. The module kept the named name/inject/Config/apply export shape consumed by the app mount, while the then-current Echo and REPL Loader smokes proved the composed tree and the plugin-shape suite pinned explicit unwrapExports behavior. The superseding removal note above owns the current package and example state.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-07-04-fold-stdio-ui-helper.md](../02-notes/archived/simplification/2026-07-04-fold-stdio-ui-helper.md)
- Pinned source: [.agents/notes/archived/simplification/2026-07-04-fold-stdio-ui-helper.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-07-04-fold-stdio-ui-helper.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `Config`, a construct named by the note. | `symbol-definition` |
| [`vendor/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts) | package entry point | Defines `Config`, a construct named by the note. | `symbol-definition` |
| [`vendor/include/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts) | package entry point | Defines `Config`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/registry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts) | runtime implementation | Defines `inject`, a construct named by the note. | `symbol-definition` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `inject`, a construct named by the note. Defines `apply`, a construct named by the note. | `symbol-definition` |
| [`packages/e2b/e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/src/index.ts) | package entry point | Defines `Config`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/invariant.ts) | runtime contract checks | Defines `inject`, a construct named by the note. Defines `apply`, a construct named by the note. | `symbol-definition` |
| [`packages/client/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts) | package entry point | Defines `inject`, a construct named by the note. Defines `apply`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts) | package entry point | Defines `inject`, a construct named by the note. Defines `apply`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts) | package entry point | Defines `apply`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `inject` | `const` | [`packages/acp/acp/src/index.ts:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L44) | `export const inject = ['agents']` |
| `apply` | `function` | [`packages/acp/acp/src/index.ts:105`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L105) | `export function apply(ctx: Context, config: AcpConfig): void {` |
| `inject` | `const` | [`packages/client/hmr/src/index.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts#L28) | `export const inject = ['clientModules', 'webServer']` |
| `apply` | `function` | [`packages/client/hmr/src/index.ts:57`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts#L57) | `export function apply(ctx: Context, config: Config): void {` |
| `Config` | `interface` | [`packages/e2b/e2b/src/index.ts:43`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/src/index.ts#L43) | `export interface Config {` |
| `inject` | `const` | [`packages/fs/fs/src/invariant.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/invariant.ts#L12) | `export const inject = ['invariants']` |
| `apply` | `const` | [`packages/fs/fs/src/invariant.ts:47`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/invariant.ts#L47) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `apply` | `function` | [`packages/fs/tool-fs/src/index.ts:54`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts#L54) | `export function apply(ctx: Context, config: Config): void {` |
| `inject` | `const` | [`packages/sdk/server/src/index.ts:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts#L22) | `export const inject = ['agents']` |
| `apply` | `function` | [`packages/sdk/server/src/index.ts:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts#L46) | `export function apply(ctx: Context, config: JsonRpcConfig): void {` |
| `inject` | `const` | [`vendor/cordis/src/registry.ts:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts#L46) | `const inject = (value[symbols.metadata] ??= {}).inject ??= Object.create(null)` |
| `Config` | `interface` | [`vendor/hmr/src/index.ts:553`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L553) | `export interface Config extends ChokidarOptions {` |
| `Config` | `const` | [`vendor/hmr/src/index.ts:560`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L560) | `export const Config: z<Config> = z.object({` |
| `Config` | `interface` | [`vendor/include/src/index.ts:161`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts#L161) | `export interface Config {` |
| `Config` | `interface` | [`vendor/loader/src/index.ts:48`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts#L48) | `export interface Config {` |

### Tests and executable evidence

- [`scripts/dev-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`scripts/test-invariants.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-invariants.spec.ts) — A test under the owning area exercises or imports `unwrapExports`.
- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`scripts/cordis-config-files.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-config-files.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`packages/lsp/tool-lsp/tests/load-path.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/tool-lsp/tests/load-path.spec.ts) — A test under the owning area exercises or imports `unwrapExports`.
- [`packages/web/tool-web/tests/load-path.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/tests/load-path.spec.ts) — A test under the owning area exercises or imports `unwrapExports`.

## How to read the implementation

1. Start with [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/evidence`, `concern/ownership`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/protocols`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/archived`, `mechanism/policy`
- Aliases: `inject`, `apply`, `Config`, `@deepseek-ai/dsh-ui-stdio`, `packages/support/`, `@deepseek-ai/dsh-stdio-demo`, `ui/`, `@deepseek-ai/dsh-stdio`, `createStdioChat`, `StdioRuntime`, `unwrapExports`, `cordis.yml`, `Fold the stdio UI helper into the stdio app`, `simplification`
- Regex: `(?i)(inject|apply|Config|@deepseek\-ai/dsh\-ui\-stdio|packages/support/|@deepseek\-ai/dsh\-stdio\-demo|@deepseek\-ai/dsh\-stdio|createStdioChat)`

```bash
rg -n --pcre2 "(?i)(inject|apply|Config|@deepseek\\-ai/dsh\\-ui\\-stdio|packages/support/|@deepseek\\-ai/dsh\\-stdio\\-demo|@deepseek\\-ai/dsh\\-stdio|createStdioChat)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0573. TUI banner brand gradient](0573-tui-banner-brand-gradient.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0636. Generated plugin config catalog](0636-generated-plugin-config-catalog.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0179. Web todo display --- snapshot side-effect channel + two render surfaces](0179-web-todo-display-snapshot-side-effect-channel-two-render-surfaces.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0103. Slot declaration injection and reload lifetimes](0103-slot-declaration-injection-and-reload-lifetimes.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0455. Remove implicit batching from ordinary sends](0455-remove-implicit-batching-from-ordinary-sends.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0058. Separate context injection from turn execution](0058-separate-context-injection-from-turn-execution.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/fs/fs/src/invariant.ts`.
- **`shares-code-with`** — [0388. Export JSDoc gate](0388-export-jsdoc-gate.md): Shares source implementation: `vendor/cordis/src/registry.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0189. Code Mode live dispatch lifecycle and native-contract parallelism](0189-code-mode-live-dispatch-lifecycle-and-native-contract-parallelism.md): Shares source implementation: `packages/e2b/e2b/src/index.ts`, `vendor/hmr/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0656-fold-the-stdio-ui-helper-into-the-stdio-app.md`.
