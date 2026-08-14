---
id: "dsh-note-0131"
title: "Code Mode --- the model writes TypeScript against the tool registry"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-06-15-code-mode.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "container"
  - "language"
  - "logs"
  - "AsyncFunction"
  - "code"
  - "constructor"
  - "CodeJsonValue"
  - "mode"
  - "CodeRunFailedError"
  - "ToolDefinition"
  - "maxParallelSubCalls"
  - "ToolRuntime"
  - "unknown"
  - "parent"
search_regex: "(?i)(container|language|logs|AsyncFunction|code|constructor|CodeJsonValue|mode)"
---

# 0131. Code Mode --- the model writes TypeScript against the tool registry — implementation context

## Open this when

In the registry's native presentation, the agent loop advertises every visible capability as a JSON-schema function definition. ToolRuntime contributes its schemas to the system-prompt assembly, the assembly's tools land on the wire (and in the logged request header), the model invokes one tool-call block per step, and at the time of this note the loop dispatched each call through ctx.tools.execute() sequentially (parallel tool execution was an open TODO then; bounded parallel dispatch has since shipped --- the parallel tool-call note, the rolling pool in docs/architecture.md) --- with every intermediate.

## Source decision

Three decisions, each elaborated in its own section below: Code Mode is a first-class presentation mode of ToolRuntime (dsh-tools), selected by a validated mode config: 'native' (the default, contributing the visible capability schemas), 'code' (the registry contributes only its reserved run_code transport plus a generated SDK .d.ts in the system prompt), or 'both' (native schemas and the transport + SDK). The registry constructs its canonical contribution at the source; the cooperative prompt-assembly result remains authoritative, and the logged request header records exactly that returned presentation.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-06-15-code-mode.md](../02-notes/implemented/feature/2026-06-15-code-mode.md)
- Pinned source: [.agents/notes/implemented/feature/2026-06-15-code-mode.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-06-15-code-mode.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `tools/pre-execute` named by the note. | `exact-code-occurrence, named-file` |
| [`docs/defensive-patterns.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/defensive-patterns.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`vendor/cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |
| [`vendor/cordis/src/logger.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/logger.ts) | runtime implementation | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |
| [`vendor/cordis/src/reflect.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/reflect.ts) | runtime implementation | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |
| [`vendor/cordis/src/service.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/service.ts) | runtime implementation | Core file in the package named by the note: `vendor/cordis`. Defines `constructor`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. Defines `ToolRuntime`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `parent`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `container` | `const` | [`packages/client/ui-primitives/src/JsonTree.tsx:268`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/JsonTree.tsx#L268) | `const container = isExpandableValue(value)` |
| `language` | `const` | [`packages/client/ui-primitives/src/markdown/render.tsx:301`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/render.tsx#L301) | `const language = node.lang ?? undefined` |
| `logs` | `const` | [`packages/code-runtime/code-runtime-worker-thread/src/bootstrap.ts:374`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/bootstrap.ts#L374) | `const logs = new LogBuffer(` |
| `AsyncFunction` | `const` | [`packages/code-runtime/code-runtime-worker-thread/src/bootstrap.ts:405`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/bootstrap.ts#L405) | `const AsyncFunction = (async () => {}).constructor as new (...args: string[]) => (...fnArgs: unknown[]) => Promise<unknown>` |
| `logs` | `const` | [`packages/code-runtime/code-runtime-worker-thread/src/index.ts:398`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/index.ts#L398) | `const logs: string[] = []` |
| `code` | `const` | [`packages/code-runtime/code-runtime-worker-thread/src/output-json.ts:69`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/output-json.ts#L69) | `const code = intrinsicReflectApply(intrinsicStringCharCodeAt, character, [0]) as number` |
| `constructor` | `const` | [`packages/code-runtime/code-runtime-worker-thread/src/worker-json.ts:85`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/worker-json.ts#L85) | `const constructor: unknown = descriptor?.value` |
| `CodeJsonValue` | `type` | [`packages/code-runtime/code-runtime/src/types.ts:21`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime/src/types.ts#L21) | `export type CodeJsonValue = null \| boolean \| number \| string \| CodeJsonValue[] \| { [key: string]: CodeJsonValue }` |
| `mode` | `const` | [`packages/core/agent-loop/src/tool-calls.ts:88`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/tool-calls.ts#L88) | `const mode = ctx.tools.executionMode(first.exec).kind` |
| `CodeRunFailedError` | `class` | [`packages/core/tools/src/code-mode.ts:139`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L139) | `export class CodeRunFailedError extends HarnessError {` |
| `mode` | `const` | [`packages/core/tools/src/code-mode.ts:419`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L419) | `const mode = head.classify()` |
| `ToolDefinition` | `interface` | [`packages/core/tools/src/index.ts:222`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L222) | `export interface ToolDefinition extends ToolSchema {` |
| `maxParallelSubCalls` | `const` | [`packages/core/tools/src/index.ts:776`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L776) | `const maxParallelSubCalls = value ?? 10` |
| `ToolRuntime` | `class` | [`packages/core/tools/src/index.ts:787`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L787) | `export class ToolRuntime extends Service {` |
| `mode` | `const` | [`packages/core/tools/src/index.ts:881`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L881) | `const mode = this.modeFor(context.scope)` |
| `mode` | `const` | [`packages/core/tools/src/index.ts:907`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L907) | `const mode = layers[index]?.mode` |

### Tests and executable evidence

- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `ToolRuntime`. A test under the owning area exercises or imports `defineTool`.
- [`packages/core/tools/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/scoped.spec.ts) — A test under the owning area exercises or imports `ToolDefinition`. A test under the owning area exercises or imports `defineProperty`.
- [`packages/core/tools/tests/schema.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/schema.spec.ts) — A test under the owning area exercises or imports `enum`. A test under the owning area exercises or imports `defineProperty`.
- [`packages/core/tools/tests/py-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/py-types.spec.ts) — A test under the owning area exercises or imports `ToolCallError`. A test under the owning area exercises or imports `defineTool`.
- [`packages/core/tools/tests/ts-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/ts-types.spec.ts) — A test under the owning area exercises or imports `ToolCallError`. A test under the owning area exercises or imports `jsonSchemaToTs`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `ToolRuntime`. A test under the owning area exercises or imports `tool-result`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `run_code`. A test under the owning area exercises or imports `codeRuntime`.
- [`packages/core/tools/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/invariant.spec.ts) — A test under the owning area exercises or imports `run_code`.
- Source verification intent: Worker runtime: Real-worker tests cover typed binding values and failures, every lossless JSON completion root, invalid and over-limit output, exact combined ledger boundaries, compute and wall budgets, hostile binding traffic, empty environment, and disposal to quiescence. A built-package test runs the worker entry under plain Node. Registry integration: Tests cover code generation, all presentation modes, reserved-name and restriction rules, scoped visibility, authoritative assembly rewrites, toolOrder, runtime compatibility failures, full-pipeline sub-dispatch, parent-token correlation, serialization.

## How to read the implementation

1. Start with [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/jobs-tasks`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/registry`
- Aliases: `container`, `language`, `logs`, `AsyncFunction`, `code`, `constructor`, `CodeJsonValue`, `mode`, `CodeRunFailedError`, `ToolDefinition`, `maxParallelSubCalls`, `ToolRuntime`, `unknown`, `parent`
- Regex: `(?i)(container|language|logs|AsyncFunction|code|constructor|CodeJsonValue|mode)`

```bash
rg -n --pcre2 "(?i)(container|language|logs|AsyncFunction|code|constructor|CodeJsonValue|mode)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0611. Keep the Code Mode result card complete](0611-keep-the-code-mode-result-card-complete.md): The source note links to this decision directly.
- **`source-link`** — [0007. Capability seams --- Service Definition / Service Provider / Consumer roles](0007-capability-seams-service-definition-service-provider-consumer-roles.md): The source note links to this decision directly.
- **`source-link`** — [0023. Tagged render-intent union for tool-call presentation](0023-tagged-render-intent-union-for-tool-call-presentation.md): The source note links to this decision directly.
- **`source-link`** — [0025. Every LLM request is reconstructable from the session log](0025-every-llm-request-is-reconstructable-from-the-session-log.md): The source note links to this decision directly.
- **`shares-code-with`** — [0561. Plan mode --- a logged per-agent session mode](0561-plan-mode-a-logged-per-agent-session-mode.md): Shares source implementation: `docs/defensive-patterns.md`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0018. Web capability seam - stable tools over multiple providers](0018-web-capability-seam-stable-tools-over-multiple-providers.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/types.ts`.
- **`shares-code-with`** — [0507. Runtime schemas for the event vocabulary (Zod vs the merge-extensible-map pattern)](0507-runtime-schemas-for-the-event-vocabulary-zod-vs-the-merge-extensible-map.md): Shares source implementation: `docs/architecture.md`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0145. Explicit model-facing tool order](0145-explicit-model-facing-tool-order.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md`.
