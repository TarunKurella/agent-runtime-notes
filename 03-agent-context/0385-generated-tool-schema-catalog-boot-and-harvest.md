---
id: "dsh-note-0385"
title: "Generated tool-schema catalog (boot-and-harvest)"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-02-tool-schema-catalog.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "json"
  - "subagent"
  - "SystemPrompt"
  - "ToolRuntime"
  - "defineTool"
  - "parameters"
  - "apply"
  - "paths"
  - "description"
  - "Context"
  - "TOOL_PACKAGES"
  - "assertManifestComplete"
  - "scripts/gen-tool-catalog.ts"
  - "ctx.tools.schemas"
search_regex: "(?i)(json|subagent|SystemPrompt|ToolRuntime|defineTool|parameters|apply|paths)"
---

# 0385. Generated tool-schema catalog (boot-and-harvest) — implementation context

## Open this when

The repository had no single reference for the names, descriptions, and JSON Schemas actually exposed to the model. Source declarations are scattered and runtime-composed, while the existing Cordis reference and subsystem pages cover wiring and vocabulary rather than tools.

## Source decision

Generate the catalog by booting each tool plugin and reading its registered schemas, not by parsing source. scripts/gen-tool-catalog.ts mounts each shipped tool package on a fresh cordis Context (with SystemPrompt + ToolRuntime and the injected services the plugin's apply reads), calls ctx.tools.schemas() --- exactly the ToolSchema[] the model is sent --- disposes the context, and renders one ## section per package with a json parameters block per tool.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-02-tool-schema-catalog.md](../02-notes/implemented/process/2026-07-02-tool-schema-catalog.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-02-tool-schema-catalog.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-02-tool-schema-catalog.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`scripts/gen-tool-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-tool-catalog.ts) | repository automation | The source note names this file directly. Defines `assertManifestComplete`, a construct named by the note. | `exact-code-occurrence, named-file, symbol-definition` |
| [`packages/jobs/tool-jobs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/jobs/tool-jobs`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/todo/tool-todo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/index.ts) | package entry point | Core file in the package named by the note: `packages/todo/tool-todo`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/todo/tool-todo/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/todo/tool-todo`. | `named-package-member` |
| [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-bash`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/tool-bash/src/render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/render.ts) | runtime implementation | Core file in the package named by the note: `packages/shell/tool-bash`. Defines `paths`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/subagent`. Defines `Context`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/subagent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/jobs/tool-jobs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/jobs/tool-jobs`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/todo/tool-todo/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/todo/tool-todo`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/tool-bash/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/tool-bash`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `subagent` | `const` | [`packages/client/ui-subagent/src/client/index.ts:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-subagent/src/client/index.ts#L45) | `const subagent = owner.session?.subagent` |
| `SystemPrompt` | `class` | [`packages/core/system-prompt/src/index.ts:338`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/index.ts#L338) | `export class SystemPrompt extends Service {` |
| `ToolRuntime` | `class` | [`packages/core/tools/src/index.ts:787`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L787) | `export class ToolRuntime extends Service {` |
| `defineTool` | `function` | [`packages/core/tools/src/schema.ts:545`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L545) | `export function defineTool<const S extends ParameterSchemaSpec, const O extends ValueSchemaSpec>(` |
| `parameters` | `const` | [`packages/core/tools/src/schema.ts:566`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L566) | `const parameters = parameterSchemaSpecToJsonSchema(options.parameters)` |
| `apply` | `function` | [`packages/jobs/tool-jobs/src/index.ts:205`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L205) | `export function apply(ctx: Context, config: Config): void {` |
| `apply` | `const` | [`packages/jobs/tool-jobs/src/invariant.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/invariant.ts#L28) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `apply` | `function` | [`packages/shell/tool-bash/src/index.ts:190`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts#L190) | `export function apply(ctx: Context, config: Config = {}): void {` |
| `paths` | `const` | [`packages/shell/tool-bash/src/render.ts:82`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/render.ts#L82) | `const paths = [read.stdoutSpillPath, read.stderrSpillPath].filter((path): path is string => path !== undefined)` |
| `description` | `const` | [`packages/skill/skill/src/index.ts:751`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L751) | `const description = skill.description` |
| `Context` | `interface` | [`packages/subagent/subagent/src/index.ts:130`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts#L130) | `interface Context {` |
| `apply` | `function` | [`packages/todo/tool-todo/src/index.ts:128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/index.ts#L128) | `export function apply(ctx: Context, config: Config): void {` |
| `apply` | `const` | [`packages/todo/tool-todo/src/invariant.ts:65`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/invariant.ts#L65) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `TOOL_PACKAGES` | `const` | [`scripts/gen-tool-catalog.ts:184`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-tool-catalog.ts#L184) | `const TOOL_PACKAGES: ToolPackage[] = [` |
| `assertManifestComplete` | `function` | [`scripts/gen-tool-catalog.ts:581`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-tool-catalog.ts#L581) | `export function assertManifestComplete(packages: ToolPackage[] = TOOL_PACKAGES, scanRoot: string = root): void {` |

### Tests and executable evidence

- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `ToolRuntime`. A test under the owning area exercises or imports `defineTool`.
- [`packages/core/tools/tests/py-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/py-types.spec.ts) — A test under the owning area exercises or imports `defineTool`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `defineTool`.
- [`packages/shell/tool-bash/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/tests/tools.spec.ts) — A test under the owning area exercises or imports `job_output`. A test under the owning area exercises or imports `job_kill`.
- [`packages/jobs/tool-jobs/tests/tool-jobs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/tests/tool-jobs.spec.ts) — A test under the owning area exercises or imports `SystemPrompt`. A test under the owning area exercises or imports `ToolRuntime`.
- [`packages/todo/tool-todo/tests/tool-todo.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/tool-todo.spec.ts) — A test under the owning area exercises or imports `tool-todo`.
- [`packages/core/tools/tests/execution-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/execution-mode.spec.ts) — A test under the owning area exercises or imports `ToolRuntime`.
- [`packages/todo/tool-todo/tests/projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/projection.spec.ts) — A test under the owning area exercises or imports `tool-todo`.

## How to read the implementation

1. Start with [`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.md) because it has the strongest evidence link to the note.
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

- Tags: `class/process`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/shell-terminal`, `domain/testing`, `domain/tools`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `json`, `subagent`, `SystemPrompt`, `ToolRuntime`, `defineTool`, `parameters`, `apply`, `paths`, `description`, `Context`, `TOOL_PACKAGES`, `assertManifestComplete`, `scripts/gen-tool-catalog.ts`, `ctx.tools.schemas`
- Regex: `(?i)(json|subagent|SystemPrompt|ToolRuntime|defineTool|parameters|apply|paths)`

```bash
rg -n --pcre2 "(?i)(json|subagent|SystemPrompt|ToolRuntime|defineTool|parameters|apply|paths)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0524. Discover package inventories instead of maintaining static lists](0524-discover-package-inventories-instead-of-maintaining-static-lists.md): The source note links to this decision directly.
- **`shares-code-with`** — [0365. The preset-authoring agent mount-validates its own composition](0365-the-preset-authoring-agent-mount-validates-its-own-composition.md): Shares source implementation: `packages/jobs/tool-jobs/src/index.ts`, `packages/jobs/tool-jobs/src/invariant.ts`.
- **`shares-code-with`** — [0290. Background job completion wakes an idle owner](0290-background-job-completion-wakes-an-idle-owner.md): Shares source implementation: `packages/jobs/tool-jobs/src/index.ts`, `packages/jobs/tool-jobs/src/invariant.ts`.
- **`shares-code-with`** — [0173. Durable subagent catalog and list_agents](0173-durable-subagent-catalog-and-list-agents.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/types.ts`.
- **`shares-code-with`** — [0023. Tagged render-intent union for tool-call presentation](0023-tagged-render-intent-union-for-tool-call-presentation.md): Shares source implementation: `packages/shell/tool-bash/src/index.ts`, `packages/todo/tool-todo/src/index.ts`.
- **`shares-code-with`** — [0265. Settlement delivery belongs to the continuation manager](0265-settlement-delivery-belongs-to-the-continuation-manager.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/types.ts`.
- **`shares-code-with`** — [0049. Canonical tool output contract](0049-canonical-tool-output-contract.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/types.ts`.
- **`shares-code-with`** — [0190. Allow several `in_progress` todos at once](0190-allow-several-in-progress-todos-at-once.md): Shares source implementation: `packages/todo/tool-todo/src/index.ts`, `packages/todo/tool-todo/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0385-generated-tool-schema-catalog-boot-and-harvest.md`.
