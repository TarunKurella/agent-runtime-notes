---
id: "dsh-note-0035"
title: "Agent-scope runtime design and correctness"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-12-agent-scope-runtime-design.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "agentEvents"
  - "AgentHandle"
  - "parent"
  - "ScopeKey"
  - "Scoped"
  - "createScope"
  - "scopeOf"
  - "scopeTarget"
  - "ScopedLayers"
  - "flush"
  - "started"
  - "SystemPrompt"
  - "ToolExecutionInput"
  - "ToolExecution"
search_regex: "(?i)(agentEvents|AgentHandle|parent|ScopeKey|Scoped|createScope|scopeOf|scopeTarget)"
---

# 0035. Agent-scope runtime design and correctness — implementation context

## Open this when

The agent-scope contract is simple for contributors: register through agent.ctx, resolve one global-plus-agent view, publish only after setup, and retain the scope until work stops. The runtime must preserve that contract across a cooperative plugin framework, asynchronous creation, reentrant listeners, durable session commits, and worker or process failure. The main design risk is adding a second mechanism for every race. Separate reservations, readiness sentinels, cancellation relays, snapshot layers, and protection registries can mirror the same fact until no reader can tell which one is authoritative.

## Source decision

The runtime uses one mechanism per independent fact. Scope routing has an opaque carrier and shared layer store; each live registry object has one entry record; each create or resume operation has one transaction; typed same-process calls borrow readonly values; real data boundaries materialize once; the cooperative prompt-assembly result is authoritative; and worker/process code retains separate terminal and quiescence state only where different owners can genuinely race.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-12-agent-scope-runtime-design.md](../02-notes/implemented/architecture/2026-07-12-agent-scope-runtime-design.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-12-agent-scope-runtime-design.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-12-agent-scope-runtime-design.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. Defines `parent`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/scope/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/scope`. Defines `Scoped`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/scope/src/store.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/store.ts) | runtime implementation | Core file in the package named by the note: `packages/core/scope`. Defines `ScopedLayers`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/agent/src/dispatch.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/dispatch.ts) | runtime implementation | Core file in the package named by the note: `packages/core/agent`. Defines `agentEvents`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/repair.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `started`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/scope/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/scope`. | `named-package-member` |
| [`packages/preset/persona/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/persona/src/index.ts) | package entry point | Core file in the package named by the note: `packages/preset/persona`. | `named-package-member` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `agentEvents` | `function` | [`packages/core/agent/src/dispatch.ts:107`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/dispatch.ts#L107) | `export function agentEvents(ctx: Context, agent: Agent, carrier: Scoped<Agent> = agentCarrier(agent)): AgentEventDispatch {` |
| `AgentHandle` | `interface` | [`packages/core/agent/src/index.ts:172`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L172) | `export interface AgentHandle {` |
| `parent` | `const` | [`packages/core/agent/src/index.ts:677`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L677) | `const parent = fiber.parent.fiber` |
| `ScopeKey` | `type` | [`packages/core/scope/src/index.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts#L15) | `export type ScopeKey = object` |
| `Scoped` | `type` | [`packages/core/scope/src/index.ts:27`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts#L27) | `export type Scoped<T extends object> = object & { readonly [ScopedBrand]: T }` |
| `createScope` | `function` | [`packages/core/scope/src/index.ts:137`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts#L137) | `export function createScope(ctx: Context, key: ScopeKey, options?: CreateScopeOptions): Scope {` |
| `scopeOf` | `function` | [`packages/core/scope/src/index.ts:154`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts#L154) | `export function scopeOf(ctx: Context): ScopeKey \| undefined {` |
| `scopeTarget` | `function` | [`packages/core/scope/src/index.ts:170`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts#L170) | `export function scopeTarget<T extends object>(base: T, key: ScopeKey \| undefined): Scoped<T> {` |
| `ScopedLayers` | `class` | [`packages/core/scope/src/store.ts:159`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/store.ts#L159) | `export class ScopedLayers<L extends ScopeLayer> {` |
| `flush` | `const` | [`packages/core/session/src/chunk-rows.ts:196`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts#L196) | `const flush = (): void => {` |
| `started` | `const` | [`packages/core/session/src/repair.ts:92`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts#L92) | `const started = callSeq !== undefined` |
| `SystemPrompt` | `class` | [`packages/core/system-prompt/src/index.ts:338`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/index.ts#L338) | `export class SystemPrompt extends Service {` |
| `ToolExecutionInput` | `interface` | [`packages/core/tools/src/index.ts:314`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L314) | `export interface ToolExecutionInput {` |
| `ToolExecution` | `interface` | [`packages/core/tools/src/index.ts:379`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L379) | `export interface ToolExecution extends ToolExecutionInput {` |
| `ToolRestriction` | `interface` | [`packages/core/tools/src/index.ts:680`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L680) | `export interface ToolRestriction {` |
| `ToolGuard` | `type` | [`packages/core/tools/src/index.ts:711`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L711) | `export type ToolGuard = (execution: Readonly<ToolExecution>) => string \| undefined` |

### Tests and executable evidence

- [`python/sdk/tests/test_client.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_client.py) — A test under the owning area exercises or imports `initialize`.
- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `ToolRuntime`. A test under the owning area exercises or imports `finalizeContent`.
- [`packages/core/scope/tests/scope.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/tests/scope.spec.ts) — A test under the owning area exercises or imports `Scoped`. A test under the owning area exercises or imports `createScope`.
- [`packages/core/scope/tests/store.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/tests/store.spec.ts) — A test under the owning area exercises or imports `ScopeKey`. A test under the owning area exercises or imports `createScope`.
- [`packages/core/agent/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/agent.spec.ts) — A test under the owning area exercises or imports `agentEvents`.
- [`packages/core/tools/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/scoped.spec.ts) — A test under the owning area exercises or imports `ToolExecutionInput`. A test under the owning area exercises or imports `ToolExecution`.
- [`python/sdk/tests/test_bundled_runtime.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_bundled_runtime.py) — A test under the owning area exercises or imports `initialize`.
- [`packages/core/session/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/scoped.spec.ts) — A test under the owning area exercises or imports `ScopeKey`. A test under the owning area exercises or imports `createScope`.

## How to read the implementation

1. Start with [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `agentEvents`, `AgentHandle`, `parent`, `ScopeKey`, `Scoped`, `createScope`, `scopeOf`, `scopeTarget`, `ScopedLayers`, `flush`, `started`, `SystemPrompt`, `ToolExecutionInput`, `ToolExecution`
- Regex: `(?i)(agentEvents|AgentHandle|parent|ScopeKey|Scoped|createScope|scopeOf|scopeTarget)`

```bash
rg -n --pcre2 "(?i)(agentEvents|AgentHandle|parent|ScopeKey|Scoped|createScope|scopeOf|scopeTarget)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0031. The agent is a registration scope](0031-the-agent-is-a-registration-scope.md): The source note links to this decision directly.
- **`source-link`** — [0036. Shared scoped-layer storage](0036-shared-scoped-layer-storage.md): The source note links to this decision directly.
- **`source-link`** — [0153. Configure subagent persona, tool visibility, and depth](0153-configure-subagent-persona-tool-visibility-and-depth.md): The source note links to this decision directly.
- **`source-link`** — [0173. Durable subagent catalog and list_agents](0173-durable-subagent-catalog-and-list-agents.md): The source note links to this decision directly.
- **`shares-code-with`** — [0024. Prompt variables and tool-guidance ownership](0024-prompt-variables-and-tool-guidance-ownership.md): Shares source implementation: `packages/core/agent/src/dispatch.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0047. Package-owned invariant service contract](0047-package-owned-invariant-service-contract.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0265. Settlement delivery belongs to the continuation manager](0265-settlement-delivery-belongs-to-the-continuation-manager.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0112. Per-preset standing mounts over a scope parent chain](0112-per-preset-standing-mounts-over-a-scope-parent-chain.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0035-agent-scope-runtime-design-and-correctness.md`.
