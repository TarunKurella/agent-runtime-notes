---
id: "dsh-note-0654"
title: "Drop `GenerateOptions.prefill` and `ToolSchema.strict` --- request knobs with no working end-to-end path"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-07-04-drop-inert-request-knobs.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "sessionId"
  - "strict"
  - "system"
  - "maxTokens"
  - "signal"
  - "schemas"
  - "DefineToolOptions"
  - "defineTool"
  - "LlmError"
  - "messages"
  - "ToolSchema"
  - "GenerateOptions"
  - "model"
  - "GenerateOptions.prefill"
search_regex: "(?i)(sessionId|strict|system|maxTokens|signal|schemas|DefineToolOptions|defineTool)"
---

# 0654. Drop `GenerateOptions.prefill` and `ToolSchema.strict` --- request knobs with no working end-to-end path — implementation context

## Open this when

Two request-contract knobs rode the whole request pipeline, yet neither could do anything: prefill (packages/llm/llm/src/types.ts) had no production setter --- the loop assembles model/system/tools/messages plus sessionId/signal, and the compaction backend adds only maxTokens --- and BOTH adapters rejected it: packages/llm/llm-deepseek/src/serialize.ts and packages/llm/llm-pi-ai/src/adapter.ts each threw LlmError('UNSUPPORTED') on a non-undefined prefill. The field's entire observable behavior was two throws, each pinned by one adapter test.

## Source decision

prefill is removed from GenerateOptions, along with both adapters' UNSUPPORTED guards, the tests pinning the throws, the paste line in core.md, and the adapter README rows documenting the rejection. The cookbook's UNSUPPORTED guidance (adding-an-llm-adapter.md) states the rule generically --- a GenerateOptions field your provider cannot honor throws LlmError(..., 'UNSUPPORTED') --- instead of using prefill as the example. The content-block vocabulary Agent Note's consequences record prefill as producer-gated rather than as having a home, per implemented/AGENTS.md.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-07-04-drop-inert-request-knobs.md](../02-notes/archived/simplification/2026-07-04-drop-inert-request-knobs.md)
- Pinned source: [.agents/notes/archived/simplification/2026-07-04-drop-inert-request-knobs.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-07-04-drop-inert-request-knobs.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/gen-tool-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-tool-catalog.ts) | repository automation | The source note names this file directly. Contains the exact code literal `scripts/gen-tool-catalog.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | The source note names this file directly. Defines `ToolSchema`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | The source note names this file directly. Core file in the package named by the note: `packages/core/tools`. | `named-file, named-package-member, symbol-definition` |
| [`scripts/type-equiv.manifest.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/type-equiv.manifest.json) | repository automation | The source note names this file directly. Contains the exact code literal `packages/llm/llm/src/types.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/core/tools/src/schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts) | runtime implementation | The source note names this file directly. Core file in the package named by the note: `packages/core/tools`. | `named-file, named-package-member, symbol-definition` |
| [`packages/llm/llm-pi-ai/src/adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts) | provider/backend adapter | The source note names this file directly. | `named-file` |
| [`docs/cookbook/adding-an-llm-adapter.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cookbook/adding-an-llm-adapter.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `packages/llm/llm/src/types.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/llm/llm-deepseek/src/serialize.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/serialize.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/code-mode.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `signal`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `sessionId` | `const` | [`packages/acp/acp/src/index.ts:254`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L254) | `const sessionId = SessionId(randomUUID())` |
| `strict` | `const` | [`packages/api/gateway/src/index.ts:225`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts#L225) | `const strict = this.ctx.typert.local.get(endpoint)` |
| `system` | `const` | [`packages/core/agent-loop/src/agent.ts:337`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L337) | `const system = renderPrompt(assembly)` |
| `maxTokens` | `const` | [`packages/core/agent-loop/src/agent.ts:427`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L427) | `const maxTokens = this.options.maxTokens` |
| `signal` | `const` | [`packages/core/tools/src/code-mode.ts:401`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L401) | `const signal = new Promise<void>((resolve) => { wake = resolve })` |
| `schemas` | `const` | [`packages/core/tools/src/index.ts:984`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L984) | `const schemas = [...view.visible.values()].map(definition => this.schemaOf(definition, false))` |
| `schemas` | `const` | [`packages/core/tools/src/index.ts:993`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L993) | `const schemas = [...view.visible.values()].map(definition => this.schemaOf(definition, false))` |
| `signal` | `const` | [`packages/core/tools/src/index.ts:1372`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1372) | `const signal = exec.signal` |
| `signal` | `const` | [`packages/core/tools/src/index.ts:1538`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1538) | `const signal = fused.signal` |
| `DefineToolOptions` | `interface` | [`packages/core/tools/src/schema.ts:483`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L483) | `export interface DefineToolOptions<S extends ParameterSchemaSpec, O extends ValueSchemaSpec> {` |
| `defineTool` | `function` | [`packages/core/tools/src/schema.ts:545`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L545) | `export function defineTool<const S extends ParameterSchemaSpec, const O extends ValueSchemaSpec>(` |
| `LlmError` | `class` | [`packages/llm/llm/src/index.ts:83`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L83) | `export class LlmError extends HarnessError {` |
| `messages` | `const` | [`packages/llm/llm/src/index.ts:824`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L824) | `const messages: Message[] = options.messages.map((message) => {` |
| `ToolSchema` | `interface` | [`packages/llm/llm/src/types.ts:312`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L312) | `export interface ToolSchema {` |
| `GenerateOptions` | `interface` | [`packages/llm/llm/src/types.ts:320`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L320) | `export interface GenerateOptions {` |
| `model` | `const` | [`packages/typert/loader/src/index.ts:109`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts#L109) | `const model = requireObject(pkgName, manifest.model, 'TYPERT.model')` |

### Tests and executable evidence

- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `LlmError`. A test under the owning area exercises or imports `GenerateOptions`.
- [`packages/llm/llm/tests/topology.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/topology.spec.ts) — A test under the owning area exercises or imports `LlmError`. A test under the owning area exercises or imports `GenerateOptions`.
- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `ToolSchema`. A test under the owning area exercises or imports `defineTool`.
- [`packages/llm/llm/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/invariant.spec.ts) — A test under the owning area exercises or imports `GenerateOptions`.
- [`packages/core/tools/tests/py-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/py-types.spec.ts) — A test under the owning area exercises or imports `defineTool`.
- [`packages/llm/llm/tests/call-config.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/call-config.spec.ts) — A test under the owning area exercises or imports `GenerateOptions`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `maxTokens`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `defineTool`.
- Source verification intent: rg prefill returns only Agent Note records (this one and the content-block vocabulary Agent Note's producer-gated consequence); a tool-schema-scoped rg strict returns only this Agent Note, the surviving pi-ai scrub, and unrelated prose such as strictEqual. Both adapters' contract tests pass without the guards, and the pi-ai fixup still scrubs the library's strict default --- wire parity pinned by its serializer tests.

## How to read the implementation

1. Start with [`scripts/gen-tool-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-tool-catalog.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `domain/agent-loop`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/adapter`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `sessionId`, `strict`, `system`, `maxTokens`, `signal`, `schemas`, `DefineToolOptions`, `defineTool`, `LlmError`, `messages`, `ToolSchema`, `GenerateOptions`, `model`, `GenerateOptions.prefill`
- Regex: `(?i)(sessionId|strict|system|maxTokens|signal|schemas|DefineToolOptions|defineTool)`

```bash
rg -n --pcre2 "(?i)(sessionId|strict|system|maxTokens|signal|schemas|DefineToolOptions|defineTool)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0685. AGENTS.md --- Archived Agent Notes](0685-agents-md-archived-agent-notes.md): The source note links to this decision directly.
- **`shares-code-with`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0145. Explicit model-facing tool order](0145-explicit-model-facing-tool-order.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0006. Structured error taxonomy](0006-structured-error-taxonomy.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0051. Unified JSON-value schema DSL](0051-unified-json-value-schema-dsl.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/schema.ts`.
- **`shares-code-with`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/types.ts`.
- **`shares-code-with`** — [0641. Tutorial-style Cordis docs under docs/cordis-tutorial](0641-tutorial-style-cordis-docs-under-docs-cordis-tutorial.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/schema.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0654-drop-generateoptions-prefill-and-toolschema-strict-request-knobs-with-no.md`.
