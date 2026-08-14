---
id: "dsh-note-0543"
title: "Custom typed tool-schema DSL instead of schemastery"
status: "archived"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/architecture/2026-06-11-custom-schema-dsl.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/schema-types"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/tools"
  - "lifecycle/archived"
  - "mechanism/adapter"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "ToolDefinition"
  - "required"
  - "ParameterSchemaSpec"
  - "InferArgs"
  - "parameterSchemaSpecToJsonSchema"
  - "defineTool"
  - "InferArgs<S>"
  - "ToolRegistry.register"
  - "Custom typed tool-schema DSL instead of schemastery"
  - "architecture"
  - "boundary"
  - "compatibility"
  - "evidence"
  - "schema types"
search_regex: "(?i)(ToolDefinition|required|ParameterSchemaSpec|InferArgs|parameterSchemaSpecToJsonSchema|defineTool|InferArgs<S>|ToolRegistry\\.register)"
---

# 0543. Custom typed tool-schema DSL instead of schemastery — implementation context

## Open this when

Tool parameters must reach the model as standard JSON Schema while giving tool authors typed execute(args) without casts. Schemastery already serves plugin config, but the tool-author API needs per-property required: true booleans rather than JSON Schema's separate required array.

## Source decision

This decision is superseded by the unified JSON-value schema DSL, which retains the small authoring surface while making parameters and typed values share one vocabulary. ParameterSchemaSpec keeps per-property required: true; InferArgs maps required keys to non-optional properties; parameterSchemaSpecToJsonSchema() compiles the implicit open object root; and defineTool() ties inference, compilation, and validation together. Raw JSON-Schema ToolDefinitions remain accepted by ToolRegistry.register() for MCP and other external tools.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/architecture/2026-06-11-custom-schema-dsl.md](../02-notes/archived/architecture/2026-06-11-custom-schema-dsl.md)
- Pinned source: [.agents/notes/archived/architecture/2026-06-11-custom-schema-dsl.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/architecture/2026-06-11-custom-schema-dsl.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `ToolDefinition`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts) | runtime implementation | Defines `required`, a construct named by the note. Defines `ParameterSchemaSpec`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/ts-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/ts-types.ts) | runtime implementation | Defines `required`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Defines `required`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/json-schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts) | runtime implementation | Defines `required`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ToolDefinition` | `interface` | [`packages/core/tools/src/index.ts:222`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L222) | `export interface ToolDefinition extends ToolSchema {` |
| `required` | `const` | [`packages/core/tools/src/json-schema.ts:210`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L210) | `const required = hasRequired ? node.required : undefined` |
| `required` | `const` | [`packages/core/tools/src/json-schema.ts:561`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L561) | `const required = Object.hasOwn(frame.node, 'required') ? frame.node.required ?? [] : []` |
| `required` | `const` | [`packages/core/tools/src/py-types.ts:562`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L562) | `const required = new Set(node.required)` |
| `ParameterSchemaSpec` | `type` | [`packages/core/tools/src/schema.ts:103`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L103) | `export type ParameterSchemaSpec = {` |
| `InferArgs` | `type` | [`packages/core/tools/src/schema.ts:175`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L175) | `export type InferArgs<S> = InferProperties<S, []>` |
| `required` | `const` | [`packages/core/tools/src/schema.ts:310`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L310) | `const required: string[] = []` |
| `parameterSchemaSpecToJsonSchema` | `function` | [`packages/core/tools/src/schema.ts:449`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L449) | `export function parameterSchemaSpecToJsonSchema(spec: ParameterSchemaSpec): ParameterJsonSchema {` |
| `defineTool` | `function` | [`packages/core/tools/src/schema.ts:545`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L545) | `export function defineTool<const S extends ParameterSchemaSpec, const O extends ValueSchemaSpec>(` |
| `required` | `const` | [`packages/core/tools/src/ts-types.ts:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/ts-types.ts#L156) | `const required = new Set(frame.node.required)` |

### Tests and executable evidence

- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `ParameterSchemaSpec`. A test under the owning area exercises or imports `InferArgs`.
- [`packages/core/tools/tests/schema.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/schema.spec.ts) — A test under the owning area exercises or imports `ParameterSchemaSpec`. A test under the owning area exercises or imports `InferArgs`.
- [`packages/core/tools/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/scoped.spec.ts) — A test under the owning area exercises or imports `ToolDefinition`.
- [`packages/core/tools/tests/py-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/py-types.spec.ts) — A test under the owning area exercises or imports `parameterSchemaSpecToJsonSchema`. A test under the owning area exercises or imports `defineTool`.
- [`packages/core/tools/tests/ts-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/ts-types.spec.ts) — A test under the owning area exercises or imports `parameterSchemaSpecToJsonSchema`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `defineTool`.
- [`packages/core/tools/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/properties.spec.ts) — A test under the owning area exercises or imports `ParameterSchemaSpec`. A test under the owning area exercises or imports `InferArgs`.
- [`packages/core/tools/tests/execution-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/execution-mode.spec.ts) — A test under the owning area exercises or imports `ToolDefinition`.

## How to read the implementation

1. Start with [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/schema-types`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/tools`, `lifecycle/archived`, `mechanism/adapter`, `mechanism/generation`, `mechanism/policy`, `mechanism/registry`
- Aliases: `ToolDefinition`, `required`, `ParameterSchemaSpec`, `InferArgs`, `parameterSchemaSpecToJsonSchema`, `defineTool`, `InferArgs<S>`, `ToolRegistry.register`, `Custom typed tool-schema DSL instead of schemastery`, `architecture`, `boundary`, `compatibility`, `evidence`, `schema types`
- Regex: `(?i)(ToolDefinition|required|ParameterSchemaSpec|InferArgs|parameterSchemaSpecToJsonSchema|defineTool|InferArgs<S>|ToolRegistry\.register)`

```bash
rg -n --pcre2 "(?i)(ToolDefinition|required|ParameterSchemaSpec|InferArgs|parameterSchemaSpecToJsonSchema|defineTool|InferArgs<S>|ToolRegistry\\.register)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0051. Unified JSON-value schema DSL](0051-unified-json-value-schema-dsl.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/json-schema.ts`.
- **`shares-code-with`** — [0165. Typed tool returns in Code Mode](0165-typed-tool-returns-in-code-mode.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/json-schema.ts`.
- **`shares-code-with`** — [0151. Parallel tool-call execution by per-call safety](0151-parallel-tool-call-execution-by-per-call-safety.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/schema.ts`.
- **`shares-code-with`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/py-types.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/py-types.ts`.
- **`shares-code-with`** — [0228. Code Mode language dispatch and the Python SDK renderer](0228-code-mode-language-dispatch-and-the-python-sdk-renderer.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/py-types.ts`.
- **`shares-code-with`** — [0142. Dynamic workflows --- a script-driven multi-agent orchestration seam](0142-dynamic-workflows-a-script-driven-multi-agent-orchestration-seam.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/py-types.ts`.
- **`shares-code-with`** — [0078. Terminal LLM stream failures](0078-terminal-llm-stream-failures.md): Shares source implementation: `packages/core/tools/src/json-schema.ts`, `packages/core/tools/src/py-types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0543-custom-typed-tool-schema-dsl-instead-of-schemastery.md`.
