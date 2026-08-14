---
id: "dsh-note-0061"
title: "Web client Agent-scope parity model and the provisioning channel (agents/scope / blank reuse / provide)"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-25-web-client-session-scope-and-provide-channel.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
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
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "sessionId"
  - "summarize"
  - "conversation"
  - "ConversationRoot"
  - "SessionStandardProps"
  - "SessionMaybeStandardProps"
  - "SessionMaybeProvideInfo"
  - "standardKit"
  - "SessionMaybeEntry"
  - "observableHook"
  - "SessionMaybeProvider"
  - "loopCtx"
  - "agentEvents"
  - "createScope"
search_regex: "(?i)(sessionId|summarize|conversation|ConversationRoot|SessionStandardProps|SessionMaybeStandardProps|SessionMaybeProvideInfo|standardKit)"
---

# 0061. Web client Agent-scope parity model and the provisioning channel (agents/scope / blank reuse / provide) — implementation context

## Open this when

The web client had a single global session surface: slots all rendered from the root context, so plugins had no notion of "which agent/session is current"; the draft's true copy was buried inside the Session object, leaving any plugin that wanted to participate in input with nowhere to hook in. To support a command/input system, the platform layer first had to answer: Who owns session interaction state (menus, popups, drafts, in-flight requests), and how two sessions are structurally isolated; What a "new session" is before the host entity exists --- whether the client must forge an independent life for it; How.

## Source decision

Host-side session.create(workspaceId) produces Session + Agent + cwd in one piece (an atomic bundle, never split); the client side is the mirror of that birth --- the instant a session row enters the list mirror, the client mints its Agent scope (actx + provide + the full input surface mounted): Session identity is the host's true form from birth: the sessionId arrives via the session.create response / the host/session-added frame, and every client-side address (the scope tag, slot store keys, RPC addressing) uses that same id.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-25-web-client-session-scope-and-provide-channel.md](../02-notes/implemented/architecture/2026-07-25-web-client-session-scope-and-provide-channel.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-25-web-client-session-scope-and-provide-channel.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-25-web-client-session-scope-and-provide-channel.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/scope/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/scope`. Defines `createScope`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/json.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/json.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `current`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. Defines `Events`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. Defines `SessionId`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/scope/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/scope`. | `named-package-member` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`.`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/.) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/scope`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/session) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `root`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/fiber.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts) | runtime implementation | Defines `effect`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/registry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts) | runtime implementation | Defines `resolve`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `sessionId` | `const` | [`packages/acp/acp/src/index.ts:254`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L254) | `const sessionId = SessionId(randomUUID())` |
| `summarize` | `function` | [`packages/bundle/headless/src/index.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts#L61) | `function summarize(events: readonly SessionEvent[], firstSeq: number): RunOutcome {` |
| `conversation` | `const` | [`packages/client/runtime/src/client/index.ts:190`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/index.ts#L190) | `const conversation = {` |
| `ConversationRoot` | `function` | [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx#L15) | `export function ConversationRoot({` |
| `SessionStandardProps` | `interface` | [`packages/client/ui-slots/src/index.ts:184`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L184) | `export interface SessionStandardProps {}` |
| `SessionMaybeStandardProps` | `interface` | [`packages/client/ui-slots/src/index.ts:191`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L191) | `export interface SessionMaybeStandardProps {}` |
| `SessionMaybeProvideInfo` | `interface` | [`packages/client/ui-slots/src/renderer.ts:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/renderer.ts#L62) | `export interface SessionMaybeProvideInfo {` |
| `standardKit` | `function` | [`packages/client/web-react/src/scoped-slots.tsx:395`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/scoped-slots.tsx#L395) | `function standardKit(` |
| `SessionMaybeEntry` | `function` | [`packages/client/web-react/src/scoped-slots.tsx:547`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/scoped-slots.tsx#L547) | `function SessionMaybeEntry({ entry, ownerProps, slotKey, slotInjected, hookContext, hasHookContext }: {` |
| `observableHook` | `function` | [`packages/client/web-react/src/session-provider.tsx:58`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/session-provider.tsx#L58) | `export function observableHook<T>(source: HostObservable<T>): SnapshotSelectorHook<T> {` |
| `SessionMaybeProvider` | `function` | [`packages/client/web-react/src/session-provider.tsx:125`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/session-provider.tsx#L125) | `export function SessionMaybeProvider({ children }: { children: ReactNode }) {` |
| `loopCtx` | `const` | [`packages/core/agent-loop/src/index.ts:472`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L472) | `const loopCtx = this.runtime.ctx` |
| `agentEvents` | `function` | [`packages/core/agent/src/dispatch.ts:107`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/dispatch.ts#L107) | `export function agentEvents(ctx: Context, agent: Agent, carrier: Scoped<Agent> = agentCarrier(agent)): AgentEventDispatch {` |
| `createScope` | `function` | [`packages/core/scope/src/index.ts:137`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts#L137) | `export function createScope(ctx: Context, key: ScopeKey, options?: CreateScopeOptions): Scope {` |
| `scopeTarget` | `function` | [`packages/core/scope/src/index.ts:170`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts#L170) | `export function scopeTarget<T extends object>(base: T, key: ScopeKey \| undefined): Scoped<T> {` |
| `Events` | `interface` | [`packages/core/session/src/index.ts:42`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L42) | `interface Events {` |

### Tests and executable evidence

- [`packages/core/scope/tests/scope.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/tests/scope.spec.ts) — A test under the owning area exercises or imports `createScope`. A test under the owning area exercises or imports `scopeTarget`.
- [`packages/core/scope/tests/store.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/tests/store.spec.ts) — A test under the owning area exercises or imports `createScope`.
- [`packages/core/agent/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/agent.spec.ts) — A test under the owning area exercises or imports `agentEvents`.
- [`packages/core/session/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/scoped.spec.ts) — A test under the owning area exercises or imports `createScope`.
- [`packages/core/scope/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/tests/invariant.spec.ts) — A test under the owning area exercises or imports `scopeTarget`.
- [`packages/core/session/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/invariant.spec.ts) — A test under the owning area exercises or imports `createScope`. A test under the owning area exercises or imports `scopeTarget`.
- [`packages/core/agent/tests/model-selection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/model-selection.spec.ts) — A test under the owning area exercises or imports `agentEvents`.
- [`packages/host/apiproxy/tests/api-proxy-rename.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-rename.spec.ts) — A test under the owning area exercises or imports `agentFor`.

## How to read the implementation

1. Start with [`packages/core/scope/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `sessionId`, `summarize`, `conversation`, `ConversationRoot`, `SessionStandardProps`, `SessionMaybeStandardProps`, `SessionMaybeProvideInfo`, `standardKit`, `SessionMaybeEntry`, `observableHook`, `SessionMaybeProvider`, `loopCtx`, `agentEvents`, `createScope`
- Regex: `(?i)(sessionId|summarize|conversation|ConversationRoot|SessionStandardProps|SessionMaybeStandardProps|SessionMaybeProvideInfo|standardKit)`

```bash
rg -n --pcre2 "(?i)(sessionId|summarize|conversation|ConversationRoot|SessionStandardProps|SessionMaybeStandardProps|SessionMaybeProvideInfo|standardKit)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0062. Web command business surfaces and assembly (ui-commands / ui-skill / ui-subagent)](0062-web-command-business-surfaces-and-assembly-ui-commands-ui-skill-ui-subag.md): The source note links to this decision directly.
- **`source-link`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): The source note links to this decision directly.
- **`source-link`** — [0121. Remote event delivery (ctx.remote.$on)](0121-remote-event-delivery-ctx-remote-on.md): The source note links to this decision directly.
- **`source-link`** — [0345. Workspace New Session reuse hijacked cwd-matching unaccounted blank sessions](0345-workspace-new-session-reuse-hijacked-cwd-matching-unaccounted-blank-sess.md): The source note links to this decision directly.
- **`shares-code-with`** — [0052. The slot system standard --- single register, four props shares, and the framework store seat](0052-the-slot-system-standard-single-register-four-props-shares-and-the-frame.md): Shares source implementation: `.`, `packages/core/scope`.
- **`shares-code-with`** — [0047. Package-owned invariant service contract](0047-package-owned-invariant-service-contract.md): Shares source implementation: `packages/core/scope/src/index.ts`, `packages/core/scope/src/invariant.ts`.
- **`shares-code-with`** — [0035. Agent-scope runtime design and correctness](0035-agent-scope-runtime-design-and-correctness.md): Shares source implementation: `packages/core/scope/src/index.ts`, `packages/core/scope/src/invariant.ts`.
- **`shares-code-with`** — [0038. Initiating Agent scope over AsyncLocalStorage](0038-initiating-agent-scope-over-asynclocalstorage.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0061-web-client-agent-scope-parity-model-and-the-provisioning-channel-agents.md`.
