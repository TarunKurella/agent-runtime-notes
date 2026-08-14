---
id: "dsh-note-0354"
title: "Code Mode collapses the executor, not just the wire"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-07-code-mode-executor-collapse.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "code"
  - "subagent"
  - "ToolRuntime"
  - "schemas"
  - "parent"
  - "native"
  - "get"
  - "wireSchemas"
  - "run_code"
  - "resolveExecution"
  - "modeFor"
  - "UNKNOWN_TOOL"
  - "ABORTED_BEFORE_DISPATCH"
  - "createExecution"
search_regex: "(?i)(code|subagent|ToolRuntime|schemas|parent|native|wireSchemas|run_code)"
---

# 0354. Code Mode collapses the executor, not just the wire — implementation context

## Open this when

mode: 'code' collapsed only the announcement surface, not the execution surface. wireSchemas() sent the model exactly one tool --- run_code --- but the executor resolved every call through get(), which returns the full visible map plus the reserved transport. A model that emitted a native tool name (write, read, bash, subagent, …) bypassed run_code entirely: the call traversed the normal pipeline and executed, even though no schema for it had ever been advertised. Providers do not intercept unadvertised tool names, so schema omission enforced nothing.

## Source decision

ToolRuntime resolves callable definitions through a new private resolveExecution(name, scope, nested) that applies the mode collapse at the operation boundary that owns it. When modeFor(scope) resolves to code, a model-direct call (nested = false) may only name the reserved run_code transport; every native name resolves to undefined and surfaces as the executor's existing UNKNOWN_TOOL error, whose message names the route back through run_code because the name IS declared to this model (an already-aborted caller signal keeps the cancellation contract: ABORTED_BEFORE_DISPATCH, with the visible tool's finalizer.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-07-code-mode-executor-collapse.md](../02-notes/implemented/bug-fix/2026-08-07-code-mode-executor-collapse.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-07-code-mode-executor-collapse.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-07-code-mode-executor-collapse.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/subagent/subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/workflow/workflow/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow/src/index.ts) | package entry point | Core file in the package named by the note: `packages/workflow/workflow`. | `named-package-member` |
| [`packages/workflow/workflow/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/workflow/workflow`. | `named-package-member` |
| [`packages/subagent/subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/workflow/workflow/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/workflow/workflow`. | `named-package-member` |
| [`packages/extensions/tool-cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/src/index.ts) | package entry point | Core file in the package named by the note: `packages/extensions/tool-cordis`. | `named-package-member` |
| [`packages/extensions/tool-cordis/src/inspect.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/src/inspect.ts) | runtime implementation | Core file in the package named by the note: `packages/extensions/tool-cordis`. Defines `parent`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/subagent/src/continuation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/continuation.ts) | runtime implementation | Core file in the package named by the note: `packages/subagent/subagent`. Defines `parent`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/extensions/tool-cordis/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/extensions/tool-cordis`. | `named-package-member` |
| [`packages/subagent/subagent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/workflow/workflow`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `code` | `const` | [`apps/cli/src/plugin.ts:135`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts#L135) | `const code = (result.error as NodeJS.ErrnoException).code` |
| `subagent` | `const` | [`packages/client/ui-subagent/src/client/index.ts:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-subagent/src/client/index.ts#L45) | `const subagent = owner.session?.subagent` |
| `ToolRuntime` | `class` | [`packages/core/tools/src/index.ts:787`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L787) | `export class ToolRuntime extends Service {` |
| `schemas` | `const` | [`packages/core/tools/src/index.ts:984`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L984) | `const schemas = [...view.visible.values()].map(definition => this.schemaOf(definition, false))` |
| `parent` | `const` | [`packages/extensions/tool-cordis/src/inspect.ts:92`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/src/inspect.ts#L92) | `const parent = current.parent.fiber` |
| `native` | `const` | [`packages/sandbox/sandbox-windows-acl/src/index.ts:359`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/index.ts#L359) | `const native = spawnSandboxedInherited(api, token, { command: options.command, args, cwd })` |
| `get` | `const` | [`packages/shell/tool-bash-persistent/src/index.ts:227`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash-persistent/src/index.ts#L227) | `const get = (owner: Agent, signal: AbortSignal): Promise<TerminalSessionId> => {` |
| `parent` | `const` | [`packages/subagent/subagent/src/continuation.ts:405`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/continuation.ts#L405) | `const parent = request.parent` |
| `parent` | `const` | [`packages/subagent/subagent/src/continuation.ts:591`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/continuation.ts#L591) | `const parent = this.resolveReportParent(child)` |
| `parent` | `const` | [`packages/subagent/subagent/src/continuation.ts:619`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/continuation.ts#L619) | `const parent = parentId === undefined ? undefined : this.ctx.agents.get(parentId)` |
| `parent` | `const` | [`packages/subagent/subagent/src/continuation.ts:824`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/continuation.ts#L824) | `const parent = this.ctx.agents.get(parentSession)` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `ToolRuntime`.
- [`packages/core/tools/tests/execution-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/execution-mode.spec.ts) — A test under the owning area exercises or imports `ToolRuntime`.
- [`packages/extensions/tool-cordis/tests/cordis-lifecycle.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/tests/cordis-lifecycle.spec.ts) — A test under the owning area exercises or imports `tool-cordis`.

## How to read the implementation

1. Start with [`packages/subagent/subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `code`, `subagent`, `ToolRuntime`, `schemas`, `parent`, `native`, `get`, `wireSchemas`, `run_code`, `resolveExecution`, `modeFor`, `UNKNOWN_TOOL`, `ABORTED_BEFORE_DISPATCH`, `createExecution`
- Regex: `(?i)(code|subagent|ToolRuntime|schemas|parent|native|wireSchemas|run_code)`

```bash
rg -n --pcre2 "(?i)(code|subagent|ToolRuntime|schemas|parent|native|wireSchemas|run_code)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): The source note links to this decision directly.
- **`shares-code-with`** — [0263. The continuable child return channel is an obligation](0263-the-continuable-child-return-channel-is-an-obligation.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/invariant.ts`.
- **`shares-code-with`** — [0049. Canonical tool output contract](0049-canonical-tool-output-contract.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/invariant.ts`.
- **`shares-code-with`** — [0184. In-process subagent policy inheritance --- the child starts under the parent's sandbox override](0184-in-process-subagent-policy-inheritance-the-child-starts-under-the-parent.md): Shares source implementation: `packages/subagent/subagent/src/continuation.ts`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0106. Subagent list identity via the projection unit](0106-subagent-list-identity-via-the-projection-unit.md): Shares source implementation: `packages/subagent/subagent/src/continuation.ts`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0285. Delegated subagents run with approvals pinned to `'never'`](0285-delegated-subagents-run-with-approvals-pinned-to-never.md): Shares source implementation: `packages/subagent/subagent/src/continuation.ts`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0173. Durable subagent catalog and list_agents](0173-durable-subagent-catalog-and-list-agents.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/invariant.ts`.
- **`shares-code-with`** — [0281. Continuable subagent policy inheritance --- the durable child log owns the delegation-time snapshot](0281-continuable-subagent-policy-inheritance-the-durable-child-log-owns-the-d.md): Shares source implementation: `packages/subagent/subagent/src/continuation.ts`, `packages/subagent/subagent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0354-code-mode-collapses-the-executor-not-just-the-wire.md`.
