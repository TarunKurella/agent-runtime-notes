---
id: "dsh-note-0636"
title: "Generated plugin config catalog"
status: "archived"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/process/2026-07-06-generated-config-catalog.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/storage"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/generation"
  - "mechanism/policy"
aliases:
  - "AcpConfig"
  - "apply"
  - "Config"
  - "LINK_MAP"
  - "scripts/gen-config-catalog.ts"
  - "--write"
  - "--check"
  - "z.object"
  - "z.intersect"
  - "BasicCompactConfig"
  - "packages/<group>/<pkg>"
  - "unwrapExports"
  - "exports.default ?? exports"
  - "docs/config-catalog.md"
search_regex: "(?i)(AcpConfig|apply|Config|LINK_MAP|scripts/gen\\-config\\-catalog\\.ts|\\-\\-write|\\-\\-check|z\\.object)"
---

# 0636. Generated plugin config catalog — implementation context

## Open this when

The repository had no source-backed reference for plugin configuration. Package READMEs documented fields inconsistently, did not enumerate which packages are loadable, and did not verify that runtime schemas agree with declared config types.

## Source decision

scripts/gen-config-catalog.ts emits docs/config-catalog.md from each plugin's declared config type and JSDoc, with injection requirements, referenced-type links, and a source pointer. Package-local types are included transitively; workspace and external types are linked or named. Deterministic --write and --check modes make the committed page a generated artifact.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/process/2026-07-06-generated-config-catalog.md](../02-notes/archived/process/2026-07-06-generated-config-catalog.md)
- Pinned source: [.agents/notes/archived/process/2026-07-06-generated-config-catalog.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/process/2026-07-06-generated-config-catalog.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `scripts/gen-config-catalog.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`scripts/gen-config-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-config-catalog.ts) | repository automation | The source note names this file directly. Contains the exact code literal `scripts/gen-config-catalog.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `Config`, a construct named by the note. | `symbol-definition` |
| [`vendor/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts) | package entry point | Defines `Config`, a construct named by the note. | `symbol-definition` |
| [`vendor/include/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts) | package entry point | Defines `Config`, a construct named by the note. | `symbol-definition` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `apply`, a construct named by the note. Defines `AcpConfig`, a construct named by the note. | `symbol-definition` |
| [`packages/e2b/e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/src/index.ts) | package entry point | Defines `Config`, a construct named by the note. | `symbol-definition` |
| [`scripts/gen-cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts) | repository automation | Defines `LINK_MAP`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/invariant.ts) | runtime contract checks | Defines `apply`, a construct named by the note. | `symbol-definition` |
| [`packages/client/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts) | package entry point | Defines `apply`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts) | package entry point | Defines `apply`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts) | package entry point | Defines `apply`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `AcpConfig` | `interface` | [`packages/acp/acp/src/index.ts:70`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L70) | `export interface AcpConfig {` |
| `apply` | `function` | [`packages/acp/acp/src/index.ts:105`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L105) | `export function apply(ctx: Context, config: AcpConfig): void {` |
| `apply` | `function` | [`packages/client/hmr/src/index.ts:57`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts#L57) | `export function apply(ctx: Context, config: Config): void {` |
| `Config` | `interface` | [`packages/e2b/e2b/src/index.ts:43`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/src/index.ts#L43) | `export interface Config {` |
| `apply` | `const` | [`packages/fs/fs/src/invariant.ts:47`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/invariant.ts#L47) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `apply` | `function` | [`packages/fs/tool-fs/src/index.ts:54`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts#L54) | `export function apply(ctx: Context, config: Config): void {` |
| `apply` | `function` | [`packages/sdk/server/src/index.ts:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts#L46) | `export function apply(ctx: Context, config: JsonRpcConfig): void {` |
| `LINK_MAP` | `const` | [`scripts/gen-cordis-catalog.ts:215`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts#L215) | `export const LINK_MAP: Readonly<Record<string, string>> = {` |
| `LINK_MAP` | `const` | [`scripts/gen-persistence-catalog.ts:40`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-persistence-catalog.ts#L40) | `const LINK_MAP: Record<string, string> = {` |
| `Config` | `interface` | [`vendor/hmr/src/index.ts:553`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L553) | `export interface Config extends ChokidarOptions {` |
| `Config` | `const` | [`vendor/hmr/src/index.ts:560`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L560) | `export const Config: z<Config> = z.object({` |
| `Config` | `interface` | [`vendor/include/src/index.ts:161`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts#L161) | `export interface Config {` |
| `Config` | `interface` | [`vendor/loader/src/index.ts:48`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts#L48) | `export interface Config {` |

### Tests and executable evidence

- [`vitest.e2e.config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vitest.e2e.config.ts) — A test under the owning area exercises or imports `and`.
- [`scripts/dev-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`scripts/test-invariants.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-invariants.spec.ts) — A test under the owning area exercises or imports `unwrapExports`.
- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`packages/acp/acp/tests/harness.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/harness.ts) — A test under the owning area exercises or imports `AcpConfig`.

## How to read the implementation

1. Start with [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/evidence`, `concern/ownership`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/storage`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/generation`, `mechanism/policy`
- Aliases: `AcpConfig`, `apply`, `Config`, `LINK_MAP`, `scripts/gen-config-catalog.ts`, `--write`, `--check`, `z.object`, `z.intersect`, `BasicCompactConfig`, `packages/<group>/<pkg>`, `unwrapExports`, `exports.default ?? exports`, `docs/config-catalog.md`
- Regex: `(?i)(AcpConfig|apply|Config|LINK_MAP|scripts/gen\-config\-catalog\.ts|\-\-write|\-\-check|z\.object)`

```bash
rg -n --pcre2 "(?i)(AcpConfig|apply|Config|LINK_MAP|scripts/gen\\-config\\-catalog\\.ts|\\-\\-write|\\-\\-check|z\\.object)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0573. TUI banner brand gradient](0573-tui-banner-brand-gradient.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0656. Fold the stdio UI helper into the stdio app](0656-fold-the-stdio-ui-helper-into-the-stdio-app.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0103. Slot declaration injection and reload lifetimes](0103-slot-declaration-injection-and-reload-lifetimes.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0632. Generated cordis events + services catalog](0632-generated-cordis-events-services-catalog.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `scripts/gen-cordis-catalog.ts`.
- **`shares-code-with`** — [0189. Code Mode live dispatch lifecycle and native-contract parallelism](0189-code-mode-live-dispatch-lifecycle-and-native-contract-parallelism.md): Shares source implementation: `packages/e2b/e2b/src/index.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0439. Private npm publication as three independent sequences](0439-private-npm-publication-as-three-independent-sequences.md): Shares source implementation: `vendor/hmr/src/index.ts`, `vendor/include/src/index.ts`.
- **`shares-code-with`** — [0179. Web todo display --- snapshot side-effect channel + two render surfaces](0179-web-todo-display-snapshot-side-effect-channel-two-render-surfaces.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0058. Separate context injection from turn execution](0058-separate-context-injection-from-turn-execution.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/fs/fs/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0636-generated-plugin-config-catalog.md`.
