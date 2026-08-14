---
id: "dsh-note-0092"
title: "Typert Gateway Targeted Method Calls"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-02-typert-remote-method-calls.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
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
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "agents"
  - "direct"
  - "namespace"
  - "args"
  - "connection"
  - "signal"
  - "implementation"
  - "method"
  - "Context"
  - "agentFor"
  - "clean"
  - "sessions"
  - "rpc"
  - "request"
search_regex: "(?i)(agents|direct|namespace|args|connection|signal|implementation|method)"
---

# 0092. Typert Gateway Targeted Method Calls — implementation context

## Open this when

The Host API Proxy handles direct method calls, stateful interactions, and Session event streams. These concerns have different lifecycles, routing semantics, and client programming interfaces. Continuing to export all business operations through one package would couple business Services, transport protocols, state machines, and client types. This decision covers only targeted method calls in which one request produces one result. Stateful interactions such as Permission and Approval, as well as Session event streams, remain separate designs.

## Source decision

A business Service extends TypertRemoteService and declares callable methods with @Remote or @RemoteScope(). A Service that already has another base class may instead expose the same binding through bindTypertRemote(). Typert generates the Host-local reflection artifact and a platform-independent Remote consumer projection from the Host Program. The Client Program continues to generate its own local reflection artifact independently. The Remote consumer projection contains .d.ts, .d.ts.map, and .js files.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-02-typert-remote-method-calls.md](../02-notes/implemented/architecture/2026-08-02-typert-remote-method-calls.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-02-typert-remote-method-calls.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-02-typert-remote-method-calls.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/goal/goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts) | package entry point | Core file in the package named by the note: `packages/goal/goal`. Defines `Context`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/goal/goal/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/goal/goal`. Defines `GoalView`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. Defines `Context`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/inbox.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/inbox.ts) | runtime implementation | Core file in the package named by the note: `packages/core/agent`. Defines `location`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/scope/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/scope`. Defines `scope`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/scope/src/store.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/store.ts) | runtime implementation | Core file in the package named by the note: `packages/core/scope`. Defines `scope`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/api/gateway/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/api/gateway`. Core file in the package named by the note: `packages/api/gateway`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/api/gateway/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/api/gateway`. Core file in the package named by the note: `packages/api/gateway`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/api/remotes/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/api/remotes`. Core file in the package named by the note: `packages/api/remotes`. | `named-directory-member, named-package-member` |
| [`packages/api/remotes/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/api/remotes`. Core file in the package named by the note: `packages/api/remotes`. | `named-directory-member, named-package-member` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. Defines `Context`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `agents` | `const` | [`packages/acp/acp/src/index.ts:108`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L108) | `const agents = ctx.agents` |
| `direct` | `const` | [`packages/api/gateway/src/client/index.ts:195`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts#L195) | `const direct = new Map<string, Set<string>>()` |
| `namespace` | `const` | [`packages/api/gateway/src/client/index.ts:208`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts#L208) | `const namespace = this.namespaces.get(descriptor.namespace)?.service` |
| `namespace` | `const` | [`packages/api/gateway/src/client/index.ts:263`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts#L263) | `const namespace = await this.namespace(descriptor.namespace)` |
| `namespace` | `const` | [`packages/api/gateway/src/client/index.ts:281`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts#L281) | `const namespace = await this.namespace(descriptor.namespace)` |
| `args` | `const` | [`packages/api/gateway/src/client/index.ts:376`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts#L376) | `const args = Object.create(null) as Record<string, unknown>` |
| `connection` | `const` | [`packages/api/gateway/src/client/index.ts:399`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts#L399) | `const connection = this.ownerCtx.get('connection') as ConnectionHandle \| undefined` |
| `signal` | `const` | [`packages/api/gateway/src/client/index.ts:402`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts#L402) | `const signal = callerSignal === undefined` |
| `direct` | `const` | [`packages/api/gateway/src/client/index.ts:481`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts#L481) | `const direct = current?.direct` |
| `namespace` | `const` | [`packages/api/gateway/src/index.ts:131`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts#L131) | `const namespace = Reflect.get(binding, 'namespace') as string` |
| `args` | `const` | [`packages/api/gateway/src/index.ts:159`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts#L159) | `const args = await Promise.all(descriptor.parameters.map(parameter =>` |
| `implementation` | `const` | [`packages/api/gateway/src/index.ts:162`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts#L162) | `const implementation = descriptor.implementation ?? descriptor.method` |
| `method` | `const` | [`packages/api/gateway/src/index.ts:163`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts#L163) | `const method = Reflect.get(receiver, implementation) as unknown` |
| `implementation` | `let` | [`packages/api/gateway/src/index.ts:544`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts#L544) | `let implementation: ((this: object, ...args: never[]) => unknown) \| undefined` |
| `Context` | `interface` | [`packages/api/gateway/src/types.ts:50`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/types.ts#L50) | `interface Context {` |
| `agentFor` | `const` | [`packages/api/remotes/src/agent-lookup.ts:136`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/src/agent-lookup.ts#L136) | `const agentFor = async (sessionId: SessionId): Promise<ApiRemoteAgentResult> => {` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/remote-model/packages/remote/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/remote-model/packages/remote/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/remote-model/packages/remote`. A test under the owning area exercises or imports `TypertRemoteService`.
- [`packages/typert/generator/tests/fixtures/remote-model/packages/remote/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/remote-model/packages/remote/src/types.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/remote-model/packages/remote`. A test under the owning area exercises or imports `CreateGoalResult`.
- [`packages/typert/generator/tests/fixtures/remote-model/packages/remote`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/remote-model/packages/remote) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/remote-model/packages/remote/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/remote-model/packages/remote/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/remote-model/packages/remote`. A test under the owning area exercises or imports `remote-client`.
- [`packages/goal/goal/tests/goal.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/tests/goal.spec.ts) — A test under the owning area exercises or imports `remoteExportCreate`.
- [`packages/core/agent/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/agent.spec.ts) — A test under the owning area exercises or imports `WeakMap`. A test under the owning area exercises or imports `agentId`.
- [`packages/api/remotes/tests/built-lib.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/tests/built-lib.e2e.ts) — A test under the owning area exercises or imports `TypertRemoteService`. A test under the owning area exercises or imports `webServer`.
- [`packages/typert/protocol/tests/protocol.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/protocol/tests/protocol.spec.ts) — A test under the owning area exercises or imports `TypertRemoteService`. A test under the owning area exercises or imports `bindTypertRemote`.
- Source verification intent: Goal Service directly decorates mutation methods whose business signatures already match the Remote contract and keeps remoteExportCreate(...) only to adapt GoalView into CreateGoalResult, without a second route, codec, or Client method list. A clean build:lib emits Host and consumer Remote artifacts before Client compilation, including the business package's JS, DTS, and declaration map under /remote. After clean, standalone typecheck, lint, and doc-typecheck regenerate the Remote contracts; the pre-push hook uses the same prepared typecheck, and CI source consumers wait for one shared contract pass.

## How to read the implementation

1. Start with [`packages/goal/goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/jobs-tasks`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `agents`, `direct`, `namespace`, `args`, `connection`, `signal`, `implementation`, `method`, `Context`, `agentFor`, `clean`, `sessions`, `rpc`, `request`
- Regex: `(?i)(agents|direct|namespace|args|connection|signal|implementation|method)`

```bash
rg -n --pcre2 "(?i)(agents|direct|namespace|args|connection|signal|implementation|method)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0035. Agent-scope runtime design and correctness](0035-agent-scope-runtime-design-and-correctness.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares source implementation: `packages/api/remotes/src/index.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0162. Persisted same-session goal domain](0162-persisted-same-session-goal-domain.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0164. Same-session goal-round driver](0164-same-session-goal-round-driver.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0046. Meaningful package invariant contracts](0046-meaningful-package-invariant-contracts.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/goal/goal/src/index.ts`.
- **`shares-code-with`** — [0024. Prompt variables and tool-guidance ownership](0024-prompt-variables-and-tool-guidance-ownership.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0157. Harness-level goal-based execution](0157-harness-level-goal-based-execution.md): Shares source implementation: `packages/goal/goal/src/index.ts`, `packages/goal/goal/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0092-typert-gateway-targeted-method-calls.md`.
