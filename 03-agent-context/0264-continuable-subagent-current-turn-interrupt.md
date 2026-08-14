---
id: "dsh-note-0264"
title: "Continuable subagent current-turn interrupt"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-06-continuable-subagent-interrupt.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
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
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "interrupt"
  - "user"
  - "internal"
  - "ancestry"
  - "cancel"
  - "AgentLoop"
  - "dispose"
  - "parent"
  - "CancelOptions"
  - "ancestor"
  - "catalogChild"
  - "sessionQuery"
  - "SubagentAddress"
  - "prompt"
search_regex: "(?i)(interrupt|user|internal|ancestry|cancel|AgentLoop|dispose|parent)"
---

# 0264. Continuable subagent current-turn interrupt — implementation context

## Open this when

A running continuable subagent could not be stopped without destroying it. The continuation manager cancels child Agents only inside whole-Activation teardown (settlement, drain, scoped drain), send_message/subagent.prompt only add work, and the Web composer's Stop button was deliberately limited to ordinary sessions. A human watching a continuable child burn tokens on a wrong path had no lever short of killing the parent tree, and when the direct parent Agent was offline the child was entirely untouchable even though its Activation stayed live.

## Source decision

ctx.subagents.interrupt(targetSessionId, authority) stops only the live target's current turn. The manager primitive authorizes synchronously, calls the existing Agent.cancel(cause, { keepInbox: true }), and returns void --- fire-and-return: the cancel signal is guaranteed issued, target quiescence is not awaited. Nothing else changes: no Activation disposal, no handle release, no descendant cascade, no inbox clearing, and no AgentLoop or CancelOptions change.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-06-continuable-subagent-interrupt.md](../02-notes/implemented/feature/2026-08-06-continuable-subagent-interrupt.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-06-continuable-subagent-interrupt.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-06-continuable-subagent-interrupt.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. Defines `parent`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/runtime-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/runtime-types.ts) | runtime implementation | Core file in the package named by the note: `packages/core/agent`. Defines `CancelOptions`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/tool-subagent-control/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/tool-subagent-control`. Defines `parent`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/tool-subagent-control/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/tool-subagent-control`. | `named-package-member` |
| [`packages/subagent/tool-subagent-control/src/list-agents.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control/src/list-agents.ts) | runtime implementation | Core file in the package named by the note: `packages/subagent/tool-subagent-control`. Defines `parent`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/host/apiproxy/tests`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/subagent/tool-subagent-control/tests`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control/tests) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/core/agent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/subagent/tool-subagent-control`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `depth`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `interrupt` | `const` | [`apps/cli/src/profile-boot.ts:212`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L212) | `const interrupt = (code: number): void => {` |
| `user` | `const` | [`packages/boot/app-boot/src/index.ts:185`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L185) | `const user = home === resolve(cwd) ? undefined : readEnvLayer(binName, home, warn)` |
| `internal` | `const` | [`packages/boot/app-boot/src/index.ts:498`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L498) | `const internal = this.ctx.loader.internal` |
| `ancestry` | `const` | [`packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx:69`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx#L69) | `const ancestry = useSessions(s => deriveAncestry(s, sessionId), equalBreadcrumbs)` |
| `cancel` | `const` | [`packages/client/ui-primitives/src/pointer-grace.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/pointer-grace.ts#L36) | `const cancel = useCallback(() => {` |
| `AgentLoop` | `class` | [`packages/core/agent-loop/src/index.ts:296`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L296) | `export class AgentLoop extends Service implements AgentFactory {` |
| `dispose` | `const` | [`packages/core/agent/src/index.ts:373`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L373) | `const dispose = this.ctx.effect(() => {` |
| `dispose` | `const` | [`packages/core/agent/src/index.ts:451`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L451) | `const dispose = this.ctx.effect(function* (this: AgentRegistry) {` |
| `parent` | `const` | [`packages/core/agent/src/index.ts:677`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L677) | `const parent = fiber.parent.fiber` |
| `CancelOptions` | `interface` | [`packages/core/agent/src/runtime-types.ts:34`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/runtime-types.ts#L34) | `export interface CancelOptions {` |
| `ancestor` | `let` | [`packages/fs/fs-local/src/fsio.ts:165`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L165) | `let ancestor = dirname(displayPath)` |
| `catalogChild` | `function` | [`packages/host/apiproxy/src/api-proxy.ts:951`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L951) | `async function catalogChild(` |
| `sessionQuery` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2043`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2043) | `const sessionQuery = ctx.get('sessionQuery')` |
| `SubagentAddress` | `type` | [`packages/host/apiproxy/src/api/subagents.ts:49`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/subagents.ts#L49) | `export type SubagentAddress =` |
| `prompt` | `const` | [`packages/schedule/schedule/src/domain.ts:389`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/domain.ts#L389) | `const prompt = value['prompt']` |
| `parentId` | `const` | [`packages/sdk/client/src/client.ts:365`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/client.ts#L365) | `const parentId = params.parentSessionId` |

### Tests and executable evidence

- [`apps/web/tests/subagent-interrupt.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/subagent-interrupt.e2e.ts) — The source note names this file directly.
- [`packages/subagent/subagent/tests/continuation.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/continuation.spec.ts) — The source note names this file directly.
- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — A test under the owning area exercises or imports `interrupt`.
- [`apps/cli/tests/process-shutdown.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/process-shutdown.spec.ts) — A test under the owning area exercises or imports `interrupt`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `AgentLoop`.
- [`packages/core/agent-loop/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/agent.spec.ts) — A test under the owning area exercises or imports `AgentLoop`.
- [`packages/core/agent-loop/tests/resume.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/resume.spec.ts) — A test under the owning area exercises or imports `AgentLoop`.
- [`packages/core/agent-loop/tests/cancel.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/cancel.spec.ts) — A test under the owning area exercises or imports `AgentLoop`.
- Source verification intent: Core coverage in packages/subagent/subagent/tests/continuation.spec.ts proves the durable turn/end abort, parked-then-FIFO-resumed queue, untouched descendant, both authority kinds with their cancel causes, self/sibling/stale/non-ancestor rejection, absent/one-shot/disposal-race no-ops, and the unchanged keepInbox loop behavior. Host coverage in packages/host/apiproxy/tests proves the RPC calls only the core primitive (no agents/catalog/history reads), the subagent-unauthorized/internal mappings, the wire schema's continuable-mode fence, and carrier round-trips.

## How to read the implementation

1. Start with [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/jobs-tasks`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`
- Aliases: `interrupt`, `user`, `internal`, `ancestry`, `cancel`, `AgentLoop`, `dispose`, `parent`, `CancelOptions`, `ancestor`, `catalogChild`, `sessionQuery`, `SubagentAddress`, `prompt`
- Regex: `(?i)(interrupt|user|internal|ancestry|cancel|AgentLoop|dispose|parent)`

```bash
rg -n --pcre2 "(?i)(interrupt|user|internal|ancestry|cancel|AgentLoop|dispose|parent)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0173. Durable subagent catalog and list_agents](0173-durable-subagent-catalog-and-list-agents.md): The source note links to this decision directly.
- **`source-link`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): The source note links to this decision directly.
- **`shares-code-with`** — [0211. Continuable subagent report tool](0211-continuable-subagent-report-tool.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0167. Continuable background subagents](0167-continuable-background-subagents.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0135. ACP subagent backend (out-of-process delegation)](0135-acp-subagent-backend-out-of-process-delegation.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0363. Bounded background job admission](0363-bounded-background-job-admission.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0042. Explicit turn cancellation capability](0042-explicit-turn-cancellation-capability.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0038. Initiating Agent scope over AsyncLocalStorage](0038-initiating-agent-scope-over-asynclocalstorage.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0264-continuable-subagent-current-turn-interrupt.md`.
