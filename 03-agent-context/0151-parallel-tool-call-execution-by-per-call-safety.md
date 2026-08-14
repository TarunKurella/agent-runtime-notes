---
id: "dsh-note-0151"
title: "Parallel tool-call execution by per-call safety"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-10-parallel-tool-call-execution.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "maxParallelToolCalls"
  - "ToolExecution"
  - "additionalContexts"
  - "defineTool"
  - "tool-call"
  - "isConcurrencySafe"
  - "ctx.tools.executionMode"
  - "tools/pre-execute"
  - "tools/post-execute"
  - "tools/execute"
  - "tool/call"
  - "tool/result"
  - "run_code"
  - "FS_STALE_VERSION"
search_regex: "(?i)(maxParallelToolCalls|ToolExecution|additionalContexts|defineTool|tool\\-call|isConcurrencySafe|ctx\\.tools\\.executionMode|tools/pre\\-execute)"
---

# 0151. Parallel tool-call execution by per-call safety — implementation context

## Open this when

An assistant message may contain several sibling tool-call blocks. Running them serially adds the latency of independent reads and web requests even though the model has already requested them together. Concurrency is a host scheduling concern, not model-facing tool metadata. The loop needs to decide which calls may overlap without hardcoding tool names or exposing scheduler policy in the JSON schema.

## Source decision

Each tool may provide an optional isConcurrencySafe(args) classifier. It is synchronous and pure: it examines only the current call's parsed arguments and performs no I/O or mutation. Only an explicit true opts in; a missing classifier, invalid arguments, a thrown classifier, or any other return value makes the call exclusive. The canonical type contract lives in the tool data structures. The classifier is deliberately unary.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-10-parallel-tool-call-execution.md](../02-notes/implemented/feature/2026-07-10-parallel-tool-call-execution.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-10-parallel-tool-call-execution.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-10-parallel-tool-call-execution.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/subsystems/tools.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/tools.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `ToolExecution`, a construct named by the note. Defines `additionalContexts`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts) | runtime implementation | Defines `defineTool`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Defines `maxParallelToolCalls`, a construct named by the note. | `symbol-definition` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `tools/pre-execute` named by the note. Contains the exact code literal `tools/post-execute` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) | package contract and examples | Contains the exact code literal `tool/call` named by the note. Contains the exact code literal `tool/result` named by the note. | `exact-code-occurrence` |
| [`packages/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/README.zh.md) | package contract and examples | Contains the exact code literal `tools/execute` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `tools/pre-execute` named by the note. Contains the exact code literal `tools/post-execute` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.zh.md) | package contract and examples | Contains the exact code literal `tool/call` named by the note. Contains the exact code literal `tool/result` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `tool/call` named by the note. Contains the exact code literal `tool/result` named by the note. | `exact-code-occurrence` |
| [`docs/capability-seams.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/capability-seams.md) | package contract and examples | Contains the exact code literal `tools/post-execute` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `maxParallelToolCalls` | `const` | [`packages/core/agent-loop/src/index.ts:134`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L134) | `const maxParallelToolCalls = value ?? DEFAULT_MAX_PARALLEL_TOOL_CALLS` |
| `ToolExecution` | `interface` | [`packages/core/tools/src/index.ts:379`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L379) | `export interface ToolExecution extends ToolExecutionInput {` |
| `additionalContexts` | `const` | [`packages/core/tools/src/index.ts:1760`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1760) | `const additionalContexts = [` |
| `defineTool` | `function` | [`packages/core/tools/src/schema.ts:545`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L545) | `export function defineTool<const S extends ParameterSchemaSpec, const O extends ValueSchemaSpec>(` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `defineTool`. A test under the owning area exercises or imports `additionalContexts`.
- [`packages/core/tools/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/scoped.spec.ts) — A test under the owning area exercises or imports `ToolExecution`.
- [`packages/core/tools/tests/py-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/py-types.spec.ts) — A test under the owning area exercises or imports `defineTool`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `defineTool`. A test under the owning area exercises or imports `additionalContexts`.
- [`packages/core/tools/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/invariant.spec.ts) — A test under the owning area exercises or imports `ToolExecution`.
- Source verification intent: Unit coverage pins fail-closed classification, typed argument validation, grouping, barriers, live reclassification after registry replacement, the rolling cap, distinct execution objects, middleware order, ordered results and context, abort draining, and scheduler-failure quiescence. First-party tests pin each parallel declaration. Snapshot coverage pins the visible multi-call transcript: pending calls may overlap while completed results remain model-ordered. Code Mode coverage pins its serial boundary. No provider-backed e2e is required because scheduling is deterministic loop behavior.

## How to read the implementation

1. Start with [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/jobs-tasks`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`
- Aliases: `maxParallelToolCalls`, `ToolExecution`, `additionalContexts`, `defineTool`, `tool-call`, `isConcurrencySafe`, `ctx.tools.executionMode`, `tools/pre-execute`, `tools/post-execute`, `tools/execute`, `tool/call`, `tool/result`, `run_code`, `FS_STALE_VERSION`
- Regex: `(?i)(maxParallelToolCalls|ToolExecution|additionalContexts|defineTool|tool\-call|isConcurrencySafe|ctx\.tools\.executionMode|tools/pre\-execute)`

```bash
rg -n --pcre2 "(?i)(maxParallelToolCalls|ToolExecution|additionalContexts|defineTool|tool\\-call|isConcurrencySafe|ctx\\.tools\\.executionMode|tools/pre\\-execute)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0280. Parallel subagent delegations](0280-parallel-subagent-delegations.md): The source note links to this decision directly.
- **`shares-code-with`** — [0149. The self-referential cordis toolset](0149-the-self-referential-cordis-toolset.md): Shares source implementation: `docs/config-catalog.md`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/schema.ts`.
- **`shares-code-with`** — [0142. Dynamic workflows --- a script-driven multi-agent orchestration seam](0142-dynamic-workflows-a-script-driven-multi-agent-orchestration-seam.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/schema.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/schema.ts`.
- **`shares-code-with`** — [0561. Plan mode --- a logged per-agent session mode](0561-plan-mode-a-logged-per-agent-session-mode.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/schema.ts`.
- **`shares-code-with`** — [0043. Cooperative tool cancellation at the registry boundary](0043-cooperative-tool-cancellation-at-the-registry-boundary.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/schema.ts`.
- **`shares-code-with`** — [0159. Fresh-agent Ralph workflow tool](0159-fresh-agent-ralph-workflow-tool.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/tools/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0151-parallel-tool-call-execution-by-per-call-safety.md`.
