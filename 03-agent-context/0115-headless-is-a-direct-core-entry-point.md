---
id: "dsh-note-0115"
title: "headless is a direct core entry point"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-09-headless-direct-core-entry-point.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/registry"
aliases:
  - "loadProfile"
  - "AgentDefaultModelConfig"
  - "ModelSelection"
  - "reasoningEffort"
  - "rpcId"
  - "InProcessApiClient"
  - "completed"
  - "flush"
  - "dsh-base"
  - "dsh-headless"
  - "headless-runner"
  - "@deepseek-ai/dsh-host-*"
  - "ctx.agentDefaultModel.currentSelection"
  - "ctx.agents.create"
search_regex: "(?i)(loadProfile|AgentDefaultModelConfig|ModelSelection|reasoningEffort|rpcId|InProcessApiClient|completed|flush)"
---

# 0115. headless is a direct core entry point — implementation context

## Open this when

The headless product contract is one local task with final assistant text on stdout, a success-sensitive exit code, empty stderr on success, and no listening port. A composition containing Workspace Host services, ApiProxy, HTTP, the Web runtime, or browser plugins contradicts that contract and makes local completion depend on an unrelated transport tree. The direct entry point still needs the same deployment model state as Web-created Agents.

## Source decision

The shipped headless profile contains dsh-base and dsh-headless. The headless bundle supplies its persona and tool mode, disables HMR, mounts the Code Mode worker explicitly, and inserts headless-runner. Its tree contains no @deepseek-ai/dsh-host- package, ApiProxy, HTTP server, Web runtime, or browser client. Code Mode and Session persistence are one-shot Agent capabilities independent of Web presentation. headless-runner is a direct core entry point.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-09-headless-direct-core-entry-point.md](../02-notes/implemented/architecture/2026-08-09-headless-direct-core-entry-point.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-09-headless-direct-core-entry-point.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-09-headless-direct-core-entry-point.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/bundle/base/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/index.ts) | package entry point | Core file in the package named by the note: `packages/bundle/base`. | `named-package-member` |
| [`packages/bundle/web-app/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/src/index.ts) | package entry point | Core file in the package named by the note: `packages/bundle/web-app`. | `named-package-member` |
| [`packages/bundle/base/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/bundle/base`. | `named-package-member` |
| [`packages/bundle/headless/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts) | package entry point | Core file in the package named by the note: `packages/bundle/headless`. | `named-package-member` |
| [`packages/bundle/web-app/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/bundle/web-app`. | `named-package-member` |
| [`packages/bundle/headless/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/bundle/headless`. | `named-package-member` |
| [`packages/core/agent-default-model/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-default-model/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-default-model`. Defines `AgentDefaultModelConfig`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent-default-model/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-default-model/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent-default-model`. | `named-package-member` |
| [`packages/bundle/base`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/bundle/web-app`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/bundle/headless`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/agent-default-model`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-default-model) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `loadProfile` | `function` | [`packages/boot/app-boot/src/profile.ts:371`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/profile.ts#L371) | `export function loadProfile(` |
| `AgentDefaultModelConfig` | `class` | [`packages/core/agent-default-model/src/index.ts:64`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-default-model/src/index.ts#L64) | `export class AgentDefaultModelConfig extends Service {` |
| `ModelSelection` | `interface` | [`packages/core/agent/src/model-selection.ts:10`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/model-selection.ts#L10) | `export interface ModelSelection {` |
| `reasoningEffort` | `const` | [`packages/core/session/src/index.ts:266`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L266) | `const reasoningEffort = configRecord['reasoningEffort']` |
| `rpcId` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1377`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1377) | `const rpcId = RpcId(randomUUID())` |
| `InProcessApiClient` | `class` | [`packages/host/apiproxy/src/fetch/client.ts:520`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/fetch/client.ts#L520) | `export class InProcessApiClient extends AbstractApiClient {` |
| `completed` | `let` | [`packages/llm/llm/src/index.ts:872`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L872) | `let completed = false` |
| `flush` | `const` | [`packages/typert/loader/src/index.ts:392`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts#L392) | `const flush = (onError: (error: Error) => void): Promise<void>[] => {` |

### Tests and executable evidence

- [`packages/bundle/base/tests/base.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/tests/base.spec.ts) — A test under the owning area exercises or imports `dsh-base`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `reasoningEffort`.
- [`packages/boot/app-boot/tests/profile.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/profile.spec.ts) — A test under the owning area exercises or imports `loadProfile`.
- [`packages/bundle/headless/tests/startup.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/tests/startup.spec.ts) — A test under the owning area exercises or imports `headless-runner`.
- [`packages/bundle/headless/tests/headless.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/tests/headless.spec.ts) — A test under the owning area exercises or imports `AgentDefaultModelConfig`. A test under the owning area exercises or imports `agentDefaultModel`.
- [`packages/core/session/tests/request-header.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/request-header.spec.ts) — A test under the owning area exercises or imports `reasoningEffort`.
- [`packages/host/apiproxy/tests/fetch-carrier.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/fetch-carrier.spec.ts) — A test under the owning area exercises or imports `InProcessApiClient`.
- [`packages/host/apiproxy/tests/client-handler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/client-handler.spec.ts) — A test under the owning area exercises or imports `InProcessApiClient`.
- Source verification intent: Package tests use the real Session store and Agent registry around a scripted Agent factory to pin idle-to-idle aggregation, late asynchronous completion, terminal model diagnostics, other non-completed exits, direct failures, Loader-time disposal, and flush-before-exit ordering. The keyless assembled snapshots drive dsh --profile headless through a replayed tool round trip, record a user/message with source.kind: 'user', and expose a terminal model failure on stderr. Built-bin acceptance reaches a mock provider through the published entry and requires final text on stdout, exit 0, and empty stderr.

## How to read the implementation

1. Start with [`packages/bundle/base/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/schema-types`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/registry`
- Aliases: `loadProfile`, `AgentDefaultModelConfig`, `ModelSelection`, `reasoningEffort`, `rpcId`, `InProcessApiClient`, `completed`, `flush`, `dsh-base`, `dsh-headless`, `headless-runner`, `@deepseek-ai/dsh-host-*`, `ctx.agentDefaultModel.currentSelection`, `ctx.agents.create`
- Regex: `(?i)(loadProfile|AgentDefaultModelConfig|ModelSelection|reasoningEffort|rpcId|InProcessApiClient|completed|flush)`

```bash
rg -n --pcre2 "(?i)(loadProfile|AgentDefaultModelConfig|ModelSelection|reasoningEffort|rpcId|InProcessApiClient|completed|flush)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0610. `dsh run` owns one-shot headless execution](0610-dsh-run-owns-one-shot-headless-execution.md): The source note links to this decision directly.
- **`source-link`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): The source note links to this decision directly.
- **`source-link`** — [0060. dsh web config-tree boot and the web transport layering](0060-dsh-web-config-tree-boot-and-the-web-transport-layering.md): The source note links to this decision directly.
- **`source-link`** — [0105. Apps own their command line through `ctx.cmdlineArgs`](0105-apps-own-their-command-line-through-ctx-cmdlineargs.md): The source note links to this decision directly.
- **`shares-code-with`** — [0313. Web agents receive explicit runtime context](0313-web-agents-receive-explicit-runtime-context.md): Shares source implementation: `packages/bundle/base`, `packages/bundle/base/src/index.ts`.
- **`shares-code-with`** — [0101. Profile plugin bundles replace the fixed surface overlays](0101-profile-plugin-bundles-replace-the-fixed-surface-overlays.md): Shares source implementation: `packages/bundle/base/src/index.ts`, `packages/bundle/base/src/invariant.ts`.
- **`shares-code-with`** — [0357. Child agents join their parent's preset composition](0357-child-agents-join-their-parent-s-preset-composition.md): Shares source implementation: `packages/bundle/base/src/index.ts`, `packages/bundle/base/src/invariant.ts`.
- **`shares-code-with`** — [0123. Trim the command-line seams to existing interfaces](0123-trim-the-command-line-seams-to-existing-interfaces.md): Shares source implementation: `packages/bundle/web-app`, `packages/bundle/web-app/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0115-headless-is-a-direct-core-entry-point.md`.
