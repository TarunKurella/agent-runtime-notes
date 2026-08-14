---
id: "dsh-note-0005"
title: "Runtime arg validation at the model boundary"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-06-11-runtime-arg-validation.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/registry"
aliases:
  - "code"
  - "oneOf"
  - "items"
  - "validateJsonSchemaValue"
  - "ParameterSchemaSpec"
  - "InferArgs"
  - "ToolArgsError"
  - "validateArgs"
  - "defineTool"
  - "HarnessError"
  - "InferArgs<S>"
  - "validateArgs(spec, args): string[]"
  - "INVALID_ARGS"
  - ".message"
search_regex: "(?i)(code|oneOf|items|validateJsonSchemaValue|ParameterSchemaSpec|InferArgs|ToolArgsError|validateArgs)"
---

# 0005. Runtime arg validation at the model boundary — implementation context

## Open this when

defineTool (the unified schema DSL) gives tool authors a typed execute(args) via the InferArgs mapping. But that type is a compile-time claim about a value that arrives at runtime as model-generated JSON: nothing forced the model to honor the schema, so a malformed call --- missing a required key, a string where a number was declared, or a literal outside the declared set --- reached execute typed-in-name-only. The tool body then either crashed on the bad shape or silently misbehaved.

## Source decision

validateArgs(spec, args): string[] compiles a ParameterSchemaSpec and delegates to the shared validateJsonSchemaValue() walker, returning human-readable violations for a well-formed declaration. defineTool snapshots the compiled parameter schema at definition time and runs that validation before the typed body; violations throw ToolArgsError (INVALID_ARGS), which the registry returns as an error result the model can correct.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-06-11-runtime-arg-validation.md](../02-notes/implemented/architecture/2026-06-11-runtime-arg-validation.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-06-11-runtime-arg-validation.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-06-11-runtime-arg-validation.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/cli/src/plugin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts) | runtime implementation | Defines `code`, a construct named by the note. | `symbol-definition` |
| [`vendor/include/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts) | package entry point | Defines `code`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/error.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts) | runtime implementation | Defines `HarnessError`, a construct named by the note. | `symbol-definition` |
| [`vendor/cosmokit/src/string.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cosmokit/src/string.ts) | runtime implementation | Defines `code`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts) | package entry point | Defines `code`, a construct named by the note. | `symbol-definition` |
| [`packages/client/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts) | package entry point | Defines `code`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts) | runtime implementation | Defines `defineTool`, a construct named by the note. Defines `InferArgs`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/json-schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts) | runtime implementation | Defines `validateJsonSchemaValue`, a construct named by the note. Defines `items`, a construct named by the note. | `symbol-definition` |
| [`packages/lsp/lsp-stdio/src/instance.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/instance.ts) | runtime implementation | Defines `items`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs-search/src/glob.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/glob.ts) | runtime implementation | Defines `items`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `items`, a construct named by the note. | `symbol-definition` |
| [`python/sdk/src/deepseek_harness/errors.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/src/deepseek_harness/errors.py) | runtime implementation | Defines `HarnessError`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `code` | `const` | [`apps/cli/src/plugin.ts:135`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts#L135) | `const code = (result.error as NodeJS.ErrnoException).code` |
| `code` | `const` | [`packages/client/hmr/src/index.ts:70`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts#L70) | `const code = (error as NodeJS.ErrnoException).code` |
| `oneOf` | `const` | [`packages/core/tools/src/json-schema.ts:290`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L290) | `const oneOf = node.oneOf` |
| `oneOf` | `const` | [`packages/core/tools/src/json-schema.ts:539`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L539) | `const oneOf = Object.hasOwn(frame.node, 'oneOf') ? frame.node.oneOf : undefined` |
| `items` | `const` | [`packages/core/tools/src/json-schema.ts:593`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L593) | `const items = Object.hasOwn(frame.node, 'items') ? frame.node.items : undefined` |
| `validateJsonSchemaValue` | `function` | [`packages/core/tools/src/json-schema.ts:654`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L654) | `export function validateJsonSchemaValue(schema: JsonSchemaNode, value: unknown, path = 'value'): string[] {` |
| `ParameterSchemaSpec` | `type` | [`packages/core/tools/src/schema.ts:103`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L103) | `export type ParameterSchemaSpec = {` |
| `InferArgs` | `type` | [`packages/core/tools/src/schema.ts:175`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L175) | `export type InferArgs<S> = InferProperties<S, []>` |
| `ToolArgsError` | `class` | [`packages/core/tools/src/schema.ts:461`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L461) | `export class ToolArgsError extends HarnessError {` |
| `validateArgs` | `function` | [`packages/core/tools/src/schema.ts:478`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L478) | `export function validateArgs(spec: ParameterSchemaSpec, args: unknown): string[] {` |
| `defineTool` | `function` | [`packages/core/tools/src/schema.ts:545`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L545) | `export function defineTool<const S extends ParameterSchemaSpec, const O extends ValueSchemaSpec>(` |
| `oneOf` | `const` | [`packages/extensions/cordis-host-runner/src/guard.ts:391`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/guard.ts#L391) | `const oneOf: Record<string, unknown>[] = []` |
| `items` | `const` | [`packages/fs/tool-fs-search/src/glob.ts:179`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/glob.ts#L179) | `const items = [path]` |
| `code` | `const` | [`packages/goal/goal/src/index.ts:170`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L170) | `const code = record?.['code']` |
| `items` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1735`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1735) | `const items = ctx.sessions.list().map(summarizeAttached)` |
| `HarnessError` | `class` | [`packages/llm/llm/src/error.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts#L13) | `export class HarnessError extends Error {` |

### Tests and executable evidence

- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `HarnessError`.
- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `defineTool`. A test under the owning area exercises or imports `InferArgs`.
- [`packages/core/tools/tests/schema.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/schema.spec.ts) — A test under the owning area exercises or imports `InferArgs`. A test under the owning area exercises or imports `ParameterSchemaSpec`.
- [`packages/core/tools/tests/py-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/py-types.spec.ts) — A test under the owning area exercises or imports `defineTool`. A test under the owning area exercises or imports `INVALID_ARGS`.
- [`packages/core/tools/tests/ts-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/ts-types.spec.ts) — A test under the owning area exercises or imports `INVALID_ARGS`. A test under the owning area exercises or imports `oneOf`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `defineTool`. A test under the owning area exercises or imports `INVALID_ARGS`.
- [`packages/core/tools/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/properties.spec.ts) — A test under the owning area exercises or imports `InferArgs`. A test under the owning area exercises or imports `validateArgs`.
- [`packages/lsp/tool-lsp/tests/tool-lsp.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/tool-lsp/tests/tool-lsp.spec.ts) — A test under the owning area exercises or imports `INVALID_ARGS`.

## How to read the implementation

1. Start with [`apps/cli/src/plugin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/filesystem`, `domain/llm`, `domain/testing`, `domain/tools`, `lifecycle/implemented`, `mechanism/registry`
- Aliases: `code`, `oneOf`, `items`, `validateJsonSchemaValue`, `ParameterSchemaSpec`, `InferArgs`, `ToolArgsError`, `validateArgs`, `defineTool`, `HarnessError`, `InferArgs<S>`, `validateArgs(spec, args): string[]`, `INVALID_ARGS`, `.message`
- Regex: `(?i)(code|oneOf|items|validateJsonSchemaValue|ParameterSchemaSpec|InferArgs|ToolArgsError|validateArgs)`

```bash
rg -n --pcre2 "(?i)(code|oneOf|items|validateJsonSchemaValue|ParameterSchemaSpec|InferArgs|ToolArgsError|validateArgs)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0006. Structured error taxonomy](0006-structured-error-taxonomy.md): The source note links to this decision directly.
- **`source-link`** — [0051. Unified JSON-value schema DSL](0051-unified-json-value-schema-dsl.md): The source note links to this decision directly.
- **`source-link`** — [0494. Property-based testing for protocol-shaped code](0494-property-based-testing-for-protocol-shaped-code.md): The source note links to this decision directly.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/core/tools/src/schema.ts`, `vendor/include/src/index.ts`.
- **`shares-code-with`** — [0078. Terminal LLM stream failures](0078-terminal-llm-stream-failures.md): Shares source implementation: `packages/core/tools/src/json-schema.ts`, `vendor/cosmokit/src/string.ts`.
- **`shares-code-with`** — [0439. Private npm publication as three independent sequences](0439-private-npm-publication-as-three-independent-sequences.md): Shares source implementation: `apps/cli/src/plugin.ts`, `vendor/include/src/index.ts`.
- **`shares-code-with`** — [0188. Code Mode chat rendering --- sub-calls as native rows under the parent](0188-code-mode-chat-rendering-sub-calls-as-native-rows-under-the-parent.md): Shares source implementation: `apps/cli/src/plugin.ts`, `packages/core/tools/src/schema.ts`.
- **`shares-code-with`** — [0376. The configurable-provider directory withholds OAuth-only providers](0376-the-configurable-provider-directory-withholds-oauth-only-providers.md): Shares source implementation: `packages/core/tools/src/json-schema.ts`, `packages/core/tools/src/schema.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0005-runtime-arg-validation-at-the-model-boundary.md`.
