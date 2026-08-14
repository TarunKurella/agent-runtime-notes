---
id: "dsh-note-0051"
title: "Unified JSON-value schema DSL"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-20-unified-json-value-schema-dsl.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/session-state"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "JsonValue"
  - "JsonSchemaNode"
  - "ObjectJsonSchema"
  - "oneOf"
  - "assertSupportedJsonSchema"
  - "assertObjectJsonSchema"
  - "validateJsonSchemaValue"
  - "ValueSchemaSpec"
  - "ParameterSchemaSpec"
  - "InferValue"
  - "InferArgs"
  - "valueSchemaSpecToJsonSchema"
  - "parameterSchemaSpecToJsonSchema"
  - "dsh-tools"
search_regex: "(?i)(JsonValue|JsonSchemaNode|ObjectJsonSchema|oneOf|assertSupportedJsonSchema|assertObjectJsonSchema|validateJsonSchemaValue|ValueSchemaSpec)"
---

# 0051. Unified JSON-value schema DSL — implementation context

## Open this when

Tool parameters used a small author DSL while subagent/workflow structured output used a separate raw JSON Schema subset and validator. The two vocabularies disagreed about roots, scalar constraints, and validation, so a typed canonical tool-output contract would either duplicate both paths again or accept schemas that some projection could not enforce.

## Source decision

dsh-tools owns one JSON-value schema vocabulary with two representations. ValueSchemaSpec is the author form for any JSON root; ParameterSchemaSpec is its implicit object-property-map form with per-property required: true. JsonSchemaNode is the raw wire form. Both support string, finite number, integer, boolean, null, array, object, type-correct scalar enum/const, and exact-one oneOf; { type: 'json' } is author-only sugar for an annotation-only unconstrained raw node. An explicit author object must declare additionalProperties: true | false.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-20-unified-json-value-schema-dsl.md](../02-notes/implemented/architecture/2026-07-20-unified-json-value-schema-dsl.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-20-unified-json-value-schema-dsl.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-20-unified-json-value-schema-dsl.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `ValueSchemaSpec`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/json-schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `JsonSchemaNode`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/session/src/json.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/json.ts) | runtime implementation | Defines `JsonValue`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-tools` named by the note. | `exact-code-occurrence` |
| [`tsconfig.base.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.base.json) | composition and configuration | Contains the exact code literal `dsh-tools` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) | package contract and examples | Contains the exact code literal `dsh-tools` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `JsonValue` | `type` | [`packages/core/session/src/json.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/json.ts#L13) | `export type JsonValue = null \| boolean \| number \| string \| JsonValue[] \| { [key: string]: JsonValue }` |
| `JsonSchemaNode` | `interface` | [`packages/core/tools/src/json-schema.ts:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L31) | `export interface JsonSchemaNode {` |
| `ObjectJsonSchema` | `type` | [`packages/core/tools/src/json-schema.ts:59`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L59) | `export type ObjectJsonSchema = JsonSchemaNode & { type: 'object' }` |
| `oneOf` | `const` | [`packages/core/tools/src/json-schema.ts:290`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L290) | `const oneOf = node.oneOf` |
| `assertSupportedJsonSchema` | `function` | [`packages/core/tools/src/json-schema.ts:385`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L385) | `export function assertSupportedJsonSchema(schema: unknown): asserts schema is JsonSchemaNode {` |
| `assertObjectJsonSchema` | `function` | [`packages/core/tools/src/json-schema.ts:397`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L397) | `export function assertObjectJsonSchema(schema: unknown): asserts schema is ObjectJsonSchema {` |
| `oneOf` | `const` | [`packages/core/tools/src/json-schema.ts:539`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L539) | `const oneOf = Object.hasOwn(frame.node, 'oneOf') ? frame.node.oneOf : undefined` |
| `validateJsonSchemaValue` | `function` | [`packages/core/tools/src/json-schema.ts:654`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L654) | `export function validateJsonSchemaValue(schema: JsonSchemaNode, value: unknown, path = 'value'): string[] {` |
| `ValueSchemaSpec` | `type` | [`packages/core/tools/src/schema.ts:85`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L85) | `export type ValueSchemaSpec =` |
| `ParameterSchemaSpec` | `type` | [`packages/core/tools/src/schema.ts:103`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L103) | `export type ParameterSchemaSpec = {` |
| `InferValue` | `type` | [`packages/core/tools/src/schema.ts:172`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L172) | `export type InferValue<S> = InferValueAt<S, []>` |
| `InferArgs` | `type` | [`packages/core/tools/src/schema.ts:175`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L175) | `export type InferArgs<S> = InferProperties<S, []>` |
| `valueSchemaSpecToJsonSchema` | `function` | [`packages/core/tools/src/schema.ts:438`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L438) | `export function valueSchemaSpecToJsonSchema(spec: ValueSchemaSpec): JsonSchemaNode {` |
| `parameterSchemaSpecToJsonSchema` | `function` | [`packages/core/tools/src/schema.ts:449`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L449) | `export function parameterSchemaSpecToJsonSchema(spec: ParameterSchemaSpec): ParameterJsonSchema {` |

### Tests and executable evidence

- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `ParameterSchemaSpec`. A test under the owning area exercises or imports `JsonSchemaNode`.
- [`packages/core/tools/tests/schema.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/schema.spec.ts) — A test under the owning area exercises or imports `ValueSchemaSpec`. A test under the owning area exercises or imports `ParameterSchemaSpec`.
- [`packages/core/session/tests/json.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/json.spec.ts) — A test under the owning area exercises or imports `JsonValue`.
- [`packages/core/tools/tests/py-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/py-types.spec.ts) — A test under the owning area exercises or imports `enum`. A test under the owning area exercises or imports `oneOf`.
- [`packages/core/tools/tests/ts-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/ts-types.spec.ts) — A test under the owning area exercises or imports `enum`. A test under the owning area exercises or imports `oneOf`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `JsonSchemaNode`. A test under the owning area exercises or imports `oneOf`.
- [`packages/core/tools/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/properties.spec.ts) — A test under the owning area exercises or imports `ValueSchemaSpec`. A test under the owning area exercises or imports `ParameterSchemaSpec`.
- [`packages/core/tools/tests/json-schema.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/json-schema.spec.ts) — A test under the owning area exercises or imports `JsonSchemaNode`. A test under the owning area exercises or imports `enum`.

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

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/schema-types`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/session-state`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `JsonValue`, `JsonSchemaNode`, `ObjectJsonSchema`, `oneOf`, `assertSupportedJsonSchema`, `assertObjectJsonSchema`, `validateJsonSchemaValue`, `ValueSchemaSpec`, `ParameterSchemaSpec`, `InferValue`, `InferArgs`, `valueSchemaSpecToJsonSchema`, `parameterSchemaSpecToJsonSchema`, `dsh-tools`
- Regex: `(?i)(JsonValue|JsonSchemaNode|ObjectJsonSchema|oneOf|assertSupportedJsonSchema|assertObjectJsonSchema|validateJsonSchemaValue|ValueSchemaSpec)`

```bash
rg -n --pcre2 "(?i)(JsonValue|JsonSchemaNode|ObjectJsonSchema|oneOf|assertSupportedJsonSchema|assertObjectJsonSchema|validateJsonSchemaValue|ValueSchemaSpec)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0165. Typed tool returns in Code Mode](0165-typed-tool-returns-in-code-mode.md): Shares source implementation: `packages/core/session/src/json.ts`, `packages/core/tools`.
- **`shares-code-with`** — [0544. Tool schemas are part of the system-prompt assembly](0544-tool-schemas-are-part-of-the-system-prompt-assembly.md): Shares source implementation: `packages/core/tools`, `packages/core/tools/README.md`.
- **`shares-code-with`** — [0525. Periodic human-review maintenance for dsh-code-review](0525-periodic-human-review-maintenance-for-dsh-code-review.md): Shares source implementation: `packages/core/tools`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0188. Code Mode chat rendering --- sub-calls as native rows under the parent](0188-code-mode-chat-rendering-sub-calls-as-native-rows-under-the-parent.md): Shares source implementation: `packages/core/tools`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0558. Rich ACP bash rendering --- the terminal card via the `_meta` convention](0558-rich-acp-bash-rendering-the-terminal-card-via-the-meta-convention.md): Shares source implementation: `packages/core/tools/README.md`, `packages/core/tools/package.json`.
- **`shares-code-with`** — [0137. The `todo_write` tool --- model task list as event-sourced session state](0137-the-todo-write-tool-model-task-list-as-event-sourced-session-state.md): Shares source implementation: `packages/core/tools`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0654. Drop `GenerateOptions.prefill` and `ToolSchema.strict` --- request knobs with no working end-to-end path](0654-drop-generateoptions-prefill-and-toolschema-strict-request-knobs-with-no.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0051-unified-json-value-schema-dsl.md`.
