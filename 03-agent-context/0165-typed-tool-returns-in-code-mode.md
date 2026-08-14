---
id: "dsh-note-0165"
title: "Typed tool returns in Code Mode"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-20-code-mode-typed-tool-returns.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "WorkerThreadCodeRuntime"
  - "CodeJsonValue"
  - "CodeRunResult"
  - "JsonValue"
  - "SESSION_FORMAT_VERSION"
  - "keys"
  - "unknown"
  - "parent"
  - "info"
  - "prototype"
  - "oneOf"
  - "jsonSchemaToTs"
  - "waitingFor"
  - "toolName"
search_regex: "(?i)(WorkerThreadCodeRuntime|CodeJsonValue|CodeRunResult|JsonValue|SESSION_FORMAT_VERSION|keys|unknown|parent)"
---

# 0165. Typed tool returns in Code Mode — implementation context

## Open this when

Code Mode originally projected each nested tool result back from ContentBlock[] into one string. That preserved the human-readable Native presentation but erased the canonical result the tool had already produced: programs had to scrape job ids and dynamic mount ids from prose, structured search and workflow results lost their shape, and non-text blocks became placeholders. The generated SDK could describe arguments but could only promise Promise regardless of the tool's real output. The runtime also treated binding values and the final program value as presentation data.

## Source decision

Code Mode is a typed projection of the visible tool registry. Each successful binding resolves to the final canonical JsonValue after post-execute policy, while a failed binding rejects with a real ToolCallError. Intermediate values remain inside the run and cross the worker boundary whole. Only the outer run_code logs, completion value, or failure diagnostic enter the configurable output ledger and the model-facing spill pipeline. This note owns the return and failure contract layered on the original Code Mode foundation.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-20-code-mode-typed-tool-returns.md](../02-notes/implemented/feature/2026-07-20-code-mode-typed-tool-returns.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-20-code-mode-typed-tool-returns.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-20-code-mode-typed-tool-returns.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. Defines `unknown`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/ts-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/ts-types.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `jsonSchemaToTs`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/tools`. Defines `parent`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/code-mode.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `keys`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/json-schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `oneOf`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Defines `parent`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/session/src/json.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/json.ts) | runtime implementation | Defines `JsonValue`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Defines `SESSION_FORMAT_VERSION`, a construct named by the note. | `symbol-definition` |
| [`packages/subagent/tool-subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts) | package entry point | Defines `toolName`, a construct named by the note. | `symbol-definition` |
| [`packages/terminal/tool-terminal/src/render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/tool-terminal/src/render.ts) | runtime implementation | Defines `byteLength`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `WorkerThreadCodeRuntime` | `class` | [`packages/code-runtime/code-runtime-worker-thread/src/index.ts:238`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/index.ts#L238) | `export class WorkerThreadCodeRuntime extends CodeRuntime {` |
| `CodeJsonValue` | `type` | [`packages/code-runtime/code-runtime/src/types.ts:21`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime/src/types.ts#L21) | `export type CodeJsonValue = null \| boolean \| number \| string \| CodeJsonValue[] \| { [key: string]: CodeJsonValue }` |
| `CodeRunResult` | `interface` | [`packages/code-runtime/code-runtime/src/types.ts:115`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime/src/types.ts#L115) | `export interface CodeRunResult {` |
| `JsonValue` | `type` | [`packages/core/session/src/json.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/json.ts#L13) | `export type JsonValue = null \| boolean \| number \| string \| JsonValue[] \| { [key: string]: JsonValue }` |
| `SESSION_FORMAT_VERSION` | `const` | [`packages/core/session/src/types.ts:56`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L56) | `export const SESSION_FORMAT_VERSION = 0` |
| `keys` | `const` | [`packages/core/tools/src/code-mode.ts:228`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L228) | `const keys = Object.keys(current)` |
| `unknown` | `const` | [`packages/core/tools/src/index.ts:1089`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1089) | `const unknown = [...allow ?? [], ...deny ?? []].filter(name => !known.has(name))` |
| `parent` | `const` | [`packages/core/tools/src/index.ts:1371`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1371) | `const parent = exec.parent` |
| `info` | `const` | [`packages/core/tools/src/index.ts:1871`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1871) | `const info = errorInfo(error)` |
| `parent` | `const` | [`packages/core/tools/src/invariant.ts:40`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts#L40) | `const parent = String(event.data.parentCallId)` |
| `prototype` | `const` | [`packages/core/tools/src/json-schema.ts:118`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L118) | `const prototype: unknown = Object.getPrototypeOf(value)` |
| `prototype` | `const` | [`packages/core/tools/src/json-schema.ts:128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L128) | `const prototype: unknown = Object.getPrototypeOf(value)` |
| `oneOf` | `const` | [`packages/core/tools/src/json-schema.ts:290`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L290) | `const oneOf = node.oneOf` |
| `parent` | `const` | [`packages/core/tools/src/json-schema.ts:492`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L492) | `const parent = frames.at(-1)` |
| `oneOf` | `const` | [`packages/core/tools/src/json-schema.ts:539`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L539) | `const oneOf = Object.hasOwn(frame.node, 'oneOf') ? frame.node.oneOf : undefined` |
| `parent` | `const` | [`packages/core/tools/src/py-types.ts:515`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L515) | `const parent = frames.at(-1)` |

### Tests and executable evidence

- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `JsonValue`. A test under the owning area exercises or imports `enum`.
- [`packages/core/tools/tests/schema.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/schema.spec.ts) — A test under the owning area exercises or imports `JsonValue`. A test under the owning area exercises or imports `enum`.
- [`packages/core/session/tests/json.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/json.spec.ts) — A test under the owning area exercises or imports `JsonValue`.
- [`packages/core/tools/tests/ts-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/ts-types.spec.ts) — A test under the owning area exercises or imports `JsonValue`. A test under the owning area exercises or imports `ToolCallError`.
- [`packages/core/tools/tests/py-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/py-types.spec.ts) — A test under the owning area exercises or imports `ToolCallError`. A test under the owning area exercises or imports `enum`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `JsonValue`. A test under the owning area exercises or imports `ToolCallError`.
- [`packages/core/tools/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/invariant.spec.ts) — A test under the owning area exercises or imports `run_code`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `SESSION_FORMAT_VERSION`.
- Source verification intent: Compile-time and snapshot tests pin exact ToolArgsMap, ToolOutputMap, ToolName, schema-to-TypeScript coverage, and exotic names. Registry and real-worker tests cover scalar, array, object, and null values; raw string rendering; absent undefined; consumer-declared real rejection classes, including ToolCallError; invalid arguments and completions, including intrinsic-looking forged prototypes; model-mutated JSON-boundary globals, prototype methods, constructor slots, and inherited descriptor fields; typed binding failures after those mutations; large uncapped intermediate bindings; nested spill suppression; exact.

## How to read the implementation

1. Start with [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/jobs-tasks`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `WorkerThreadCodeRuntime`, `CodeJsonValue`, `CodeRunResult`, `JsonValue`, `SESSION_FORMAT_VERSION`, `keys`, `unknown`, `parent`, `info`, `prototype`, `oneOf`, `jsonSchemaToTs`, `waitingFor`, `toolName`
- Regex: `(?i)(WorkerThreadCodeRuntime|CodeJsonValue|CodeRunResult|JsonValue|SESSION_FORMAT_VERSION|keys|unknown|parent)`

```bash
rg -n --pcre2 "(?i)(WorkerThreadCodeRuntime|CodeJsonValue|CodeRunResult|JsonValue|SESSION_FORMAT_VERSION|keys|unknown|parent)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0015. The background job runtime (`ctx.jobs`) and generic task control tools](0015-the-background-job-runtime-ctx-jobs-and-generic-task-control-tools.md): The source note links to this decision directly.
- **`source-link`** — [0049. Canonical tool output contract](0049-canonical-tool-output-contract.md): The source note links to this decision directly.
- **`source-link`** — [0051. Unified JSON-value schema DSL](0051-unified-json-value-schema-dsl.md): The source note links to this decision directly.
- **`source-link`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): The source note links to this decision directly.
- **`shares-code-with`** — [0228. Code Mode language dispatch and the Python SDK renderer](0228-code-mode-language-dispatch-and-the-python-sdk-renderer.md): Shares source implementation: `packages/core/tools/src/code-mode.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0187. Code Mode UI foundation --- run_code description and native-parity dispatch logging](0187-code-mode-ui-foundation-run-code-description-and-native-parity-dispatch.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/core/tools`.
- **`shares-code-with`** — [0525. Periodic human-review maintenance for dsh-code-review](0525-periodic-human-review-maintenance-for-dsh-code-review.md): Shares source implementation: `packages/core/tools`, `packages/core/tools/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0165-typed-tool-returns-in-code-mode.md`.
