---
id: "dsh-note-0067"
title: "Compiler-independent Typert type model"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-27-compiler-independent-typert-model.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "json"
  - "Context"
  - "Events"
  - "unknown"
  - "SERVICE_API"
  - "EVENT_API"
  - "TYPE_API"
  - "CordisCatalogProjector"
  - "TypertEmitError"
  - "FaceModel"
  - "WorkspaceModel"
  - "TypeGraph"
  - "WorkspaceTypertGenerator"
  - "check"
search_regex: "(?i)(json|Context|Events|unknown|SERVICE_API|EVENT_API|TYPE_API|CordisCatalogProjector)"
---

# 0067. Compiler-independent Typert type model — implementation context

## Open this when

Constructing Zod and reflection text directly from the TypeScript AST couples type analysis and business-semantic recognition to a single generation target. Such a generator can answer only "can this syntax be generated?" It cannot provide a canonical representation of packages, faces, public exports, services, events, objects, and their type relationships, nor can static checks and later generation targets reuse it. The host and client are independent TypeScript projects; placing both in one ts.Program merges conflicting Cordis Context and Events declarations.

## Source decision

dsh-typert-generator builds separate ts.Program instances from the host and client projects and uses compiler nodes, symbols, and checkers only as extraction tools. After analysis, every generator and scanner consumes only Typert's own WorkspaceModel, FaceModel, and TypeGraph; the model retains no AST or checker objects. The generator has no dependency on @deepseek-ai/dsh-typert-registry. TypeGraph preserves the developer-authored, pre-evaluation type structure, including generic parameters and applications, explicit inheritance, conditional and mapped types, recursive references, and JSDoc.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-27-compiler-independent-typert-model.md](../02-notes/implemented/architecture/2026-07-27-compiler-independent-typert-model.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-27-compiler-independent-typert-model.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-27-compiler-independent-typert-model.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/typert/loader/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/typert/loader`. | `named-file, named-package-member` |
| [`packages/typert/registry/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/typert/registry`. | `named-file, named-package-member` |
| [`packages/typert/generator/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/typert/generator`. | `exact-code-occurrence, named-file, named-package-member` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. Defines `Context`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/typert/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts) | package entry point | Core file in the package named by the note: `packages/typert/loader`. | `named-package-member` |
| [`packages/core/tools/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/typert/registry/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/src/index.ts) | package entry point | Core file in the package named by the note: `packages/typert/registry`. | `named-package-member` |
| [`packages/typert/registry/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/typert/registry`. | `named-package-member` |
| [`packages/typert/generator/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/index.ts) | package entry point | Core file in the package named by the note: `packages/typert/generator`. | `named-package-member` |
| [`packages/typert/generator/src/model.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/model.ts) | runtime implementation | Core file in the package named by the note: `packages/typert/generator`. Defines `WorkspaceModel`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/typert/loader/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/typert/loader`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `Context` | `interface` | [`packages/core/tools/src/index.ts:138`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L138) | `interface Context {` |
| `Events` | `interface` | [`packages/core/tools/src/index.ts:142`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L142) | `interface Events {` |
| `unknown` | `const` | [`packages/core/tools/src/index.ts:1089`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1089) | `const unknown = [...allow ?? [], ...deny ?? []].filter(name => !known.has(name))` |
| `SERVICE_API` | `const` | [`packages/extensions/tool-cordis/src/api-catalog.ts:83`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/src/api-catalog.ts#L83) | `export const SERVICE_API: readonly ServiceApiEntry[] = [` |
| `EVENT_API` | `const` | [`packages/extensions/tool-cordis/src/api-catalog.ts:2159`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/src/api-catalog.ts#L2159) | `export const EVENT_API: readonly EventApiEntry[] = [` |
| `TYPE_API` | `const` | [`packages/extensions/tool-cordis/src/api-catalog.ts:2611`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/src/api-catalog.ts#L2611) | `export const TYPE_API: readonly TypeApiEntry[] = [` |
| `CordisCatalogProjector` | `class` | [`packages/typert/generator/src/cordis-catalog.ts:136`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts#L136) | `export class CordisCatalogProjector {` |
| `TypertEmitError` | `class` | [`packages/typert/generator/src/emitter.ts:27`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/emitter.ts#L27) | `export class TypertEmitError extends Error {` |
| `FaceModel` | `interface` | [`packages/typert/generator/src/model.ts:177`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/model.ts#L177) | `export interface FaceModel {` |
| `WorkspaceModel` | `interface` | [`packages/typert/generator/src/model.ts:184`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/model.ts#L184) | `export interface WorkspaceModel {` |
| `TypeGraph` | `interface` | [`packages/typert/generator/src/model.ts:430`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/model.ts#L430) | `export interface TypeGraph {` |
| `WorkspaceTypertGenerator` | `class` | [`packages/typert/generator/src/workspace.ts:20`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/workspace.ts#L20) | `export class WorkspaceTypertGenerator {` |
| `check` | `function` | [`vendor/cosmokit/src/types.ts:125`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cosmokit/src/types.ts#L125) | `function check<T>(test: (x: any) => x is T, then: (a: T, b: T) => boolean) {` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/core/tools/tests/schema.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/schema.spec.ts) — A test under the owning area exercises or imports `infer`.
- [`packages/typert/loader/tests/loader.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/tests/loader.spec.ts) — A test under the owning area exercises or imports `dsh-typert-registry`. A test under the owning area exercises or imports `TYPERT`.
- [`packages/typert/registry/tests/typert.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/tests/typert.spec.ts) — A test under the owning area exercises or imports `dsh-typert-registry`. A test under the owning area exercises or imports `dsh-tools`.
- [`packages/typert/generator/tests/renderer.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/renderer.spec.ts) — A test under the owning area exercises or imports `TypeGraph`. A test under the owning area exercises or imports `infer`.
- [`packages/typert/generator/tests/type-model.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/type-model.spec.ts) — A test under the owning area exercises or imports `WorkspaceTypertGenerator`. A test under the owning area exercises or imports `TYPERT`.
- Source verification intent: A small two-face project in the repository snapshots the complete type model, including its source declaration index. Batched workspace analysis and direct focused analysis must produce model-equivalent FaceModel and TypeGraph results for the same faces. Compile-time exhaustive maps and runtime set comparisons ensure that every node, target, declaration, and member discriminant is exercised by source-authored TypeScript syntax; a field-semantics matrix covers every keyword, type operator, and literal value category, plus every state of generics, parameters, tuples, mapped modifiers, import attributes, abstract.

## How to read the implementation

1. Start with [`packages/typert/loader/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/README.md) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `domain/build-release`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `json`, `Context`, `Events`, `unknown`, `SERVICE_API`, `EVENT_API`, `TYPE_API`, `CordisCatalogProjector`, `TypertEmitError`, `FaceModel`, `WorkspaceModel`, `TypeGraph`, `WorkspaceTypertGenerator`, `check`
- Regex: `(?i)(json|Context|Events|unknown|SERVICE_API|EVENT_API|TYPE_API|CordisCatalogProjector)`

```bash
rg -n --pcre2 "(?i)(json|Context|Events|unknown|SERVICE_API|EVENT_API|TYPE_API|CordisCatalogProjector)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0092. Typert Gateway Targeted Method Calls](0092-typert-gateway-targeted-method-calls.md): The source note links to this decision directly.
- **`shares-code-with`** — [0144. The approval seam --- one-shot permission decisions over a waterfall of answerers](0144-the-approval-seam-one-shot-permission-decisions-over-a-waterfall-of-answ.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0165. Typed tool returns in Code Mode](0165-typed-tool-returns-in-code-mode.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0036. Shared scoped-layer storage](0036-shared-scoped-layer-storage.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0159. Fresh-agent Ralph workflow tool](0159-fresh-agent-ralph-workflow-tool.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0137. The `todo_write` tool --- model task list as event-sourced session state](0137-the-todo-write-tool-model-task-list-as-event-sourced-session-state.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0067-compiler-independent-typert-type-model.md`.
