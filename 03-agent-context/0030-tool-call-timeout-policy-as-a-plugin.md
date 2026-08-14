---
id: "dsh-note-0030"
title: "Tool-call timeout policy as a plugin"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-07-tool-call-timeout-policy.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/tools"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "web"
  - "shell"
  - "signal"
  - "ToolDefinition"
  - "ToolExecutionResult"
  - "timeoutMs"
  - "inject"
  - "apply"
  - "defineTool"
  - "TOOL_TIMEOUT"
  - "runHook"
  - "searchTimeoutMs"
  - "deadline"
  - "timeoutOf"
search_regex: "(?i)(shell|signal|ToolDefinition|ToolExecutionResult|timeoutMs|inject|apply|defineTool)"
---

# 0030. Tool-call timeout policy as a plugin — implementation context

## Open this when

The timeout/deadline Agent Note extracted the timing-and-classification primitive into @deepseek-ai/dsh-timeout, but timeout policy was still attached to individual capabilities and model-facing schemas. bash exposed timeoutMs; web_fetch exposed timeout_ms; web_search had no model-facing timeout even though providers already honor exec.signal; a future grep/glob tool would either import the timeout library directly or invent its own timeout policy.

## Source decision

Tool-call timeout is a policy that applies only to model-facing tool execution, in three parts: @deepseek-ai/dsh-timeout remains the shared library that owns deadline() and timeoutOf(). @deepseek-ai/dsh-tools has an around-dispatch waterfall, tools/execute, between tools/pre-execute and tools/post-execute. The repository naming contract names @deepseek-ai/dsh-tool-call-timeout-policy for the exact operation it limits. The plugin reads each tool's declared timeoutMs from the runtime and wraps a call that has one by deriving a new exec.signal.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-07-tool-call-timeout-policy.md](../02-notes/implemented/architecture/2026-07-07-tool-call-timeout-policy.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-07-tool-call-timeout-policy.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-07-tool-call-timeout-policy.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/cordis/src/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. Defines `timeoutMs`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `defineTool`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/shell/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/shell/shell/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/core/tools/src/testing.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/testing.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/shell/shell/src/render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/render.ts) | runtime implementation | Core file in the package named by the note: `packages/shell/shell`. Defines `signal`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/util/timeout/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/index.ts) | package entry point | Core file in the package named by the note: `packages/util/timeout`. Defines `signal`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/web/tool-web/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/index.ts) | package entry point | Core file in the package named by the note: `packages/web/tool-web`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/web/tool-web/src/fetch.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts) | runtime implementation | Core file in the package named by the note: `packages/web/tool-web`. | `named-package-member` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `web` | `const` | [`apps/cli/src/args.ts:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts#L156) | `const web = program.command('web').description('boot the web profile (alias of --profile web); the web app\'s own flags follow')` |
| `shell` | `const` | [`packages/client/web/src/boot.tsx:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L121) | `const shell = this.ctx.get('appShell')` |
| `signal` | `const` | [`packages/core/tools/src/code-mode.ts:401`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L401) | `const signal = new Promise<void>((resolve) => { wake = resolve })` |
| `ToolDefinition` | `interface` | [`packages/core/tools/src/index.ts:222`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L222) | `export interface ToolDefinition extends ToolSchema {` |
| `ToolExecutionResult` | `type` | [`packages/core/tools/src/index.ts:580`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L580) | `export type ToolExecutionResult = ToolExecutionSuccess \| ToolExecutionFailure` |
| `timeoutMs` | `const` | [`packages/core/tools/src/index.ts:1046`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1046) | `const timeoutMs = definition.timeoutMs` |
| `signal` | `const` | [`packages/core/tools/src/index.ts:1372`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1372) | `const signal = exec.signal` |
| `signal` | `const` | [`packages/core/tools/src/index.ts:1538`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1538) | `const signal = fused.signal` |
| `inject` | `const` | [`packages/core/tools/src/invariant.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts#L13) | `export const inject = ['invariants']` |
| `apply` | `const` | [`packages/core/tools/src/invariant.ts:127`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts#L127) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `defineTool` | `function` | [`packages/core/tools/src/schema.ts:545`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L545) | `export function defineTool<const S extends ParameterSchemaSpec, const O extends ValueSchemaSpec>(` |
| `TOOL_TIMEOUT` | `const` | [`packages/guard/timeout-policy/src/index.ts:25`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/guard/timeout-policy/src/index.ts#L25) | `export const TOOL_TIMEOUT = 'TOOL_TIMEOUT'` |
| `timeoutMs` | `const` | [`packages/guard/timeout-policy/src/index.ts:57`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/guard/timeout-policy/src/index.ts#L57) | `const timeoutMs = ctx.tools.get(exec.name, exec.agent)?.timeoutMs` |
| `runHook` | `function` | [`packages/hooks/hook-protocol/src/runner.ts:67`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/runner.ts#L67) | `export async function runHook(` |
| `searchTimeoutMs` | `const` | [`packages/session-query/tool-session-query/src/index.ts:127`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/index.ts#L127) | `const searchTimeoutMs = config.searchTimeoutMs ?? DEFAULT_SEARCH_TIMEOUT_MS` |
| `timeoutMs` | `const` | [`packages/shell/bash-local/src/index.ts:147`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts#L147) | `const timeoutMs = clampTimeout(` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `timeoutMs`. A test under the owning area exercises or imports `finally`.
- [`apps/web/tests/hmr-live.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/hmr-live.e2e.ts) — A test under the owning area exercises or imports `finally`.
- [`apps/web/tests/steering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/steering.e2e.ts) — A test under the owning area exercises or imports `race`.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `web_fetch`. A test under the owning area exercises or imports `web_search`.
- [`apps/web/tests/assembled-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/assembled-boot.ts) — A test under the owning area exercises or imports `inject`.

## How to read the implementation

1. Start with [`vendor/cordis/src/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/tools`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `web`, `shell`, `signal`, `ToolDefinition`, `ToolExecutionResult`, `timeoutMs`, `inject`, `apply`, `defineTool`, `TOOL_TIMEOUT`, `runHook`, `searchTimeoutMs`, `deadline`, `timeoutOf`
- Regex: `(?i)(shell|signal|ToolDefinition|ToolExecutionResult|timeoutMs|inject|apply|defineTool)`

```bash
rg -n --pcre2 "(?i)(shell|signal|ToolDefinition|ToolExecutionResult|timeoutMs|inject|apply|defineTool)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0028. A shared timeout/deadline primitive, with hard-kill left to each capability](0028-a-shared-timeout-deadline-primitive-with-hard-kill-left-to-each-capabili.md): The source note links to this decision directly.
- **`source-link`** — [0043. Cooperative tool cancellation at the registry boundary](0043-cooperative-tool-cancellation-at-the-registry-boundary.md): The source note links to this decision directly.
- **`source-link`** — [0126. Repository naming contract and pre-release rename ledger](0126-repository-naming-contract-and-pre-release-rename-ledger.md): The source note links to this decision directly.
- **`shares-code-with`** — [0149. The self-referential cordis toolset](0149-the-self-referential-cordis-toolset.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/schema.ts`.
- **`shares-code-with`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/schema.ts`.
- **`shares-code-with`** — [0142. Dynamic workflows --- a script-driven multi-agent orchestration seam](0142-dynamic-workflows-a-script-driven-multi-agent-orchestration-seam.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/schema.ts`.
- **`shares-code-with`** — [0158. persistent PTY sessions](0158-persistent-pty-sessions.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/types.ts`.
- **`shares-code-with`** — [0032. Tool output spill policy](0032-tool-output-spill-policy.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/testing.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0030-tool-call-timeout-policy-as-a-plugin.md`.
