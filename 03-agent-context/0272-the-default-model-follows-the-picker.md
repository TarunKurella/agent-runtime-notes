---
id: "dsh-note-0272"
title: "the default model follows the picker"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-07-default-model-follows-the-picker.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/adapter"
aliases:
  - "AgentDefaultModelConfig"
  - "ModelSelection"
  - "reasoningEffort"
  - "describe"
  - "ApiProxyDefaults"
  - "createApiProxy"
  - "selectionFor"
  - "routable"
  - "ApiProxyService"
  - "blocks"
  - "models"
  - "prompt"
  - "yaml"
  - "ctx.agentDefaultModel"
search_regex: "(?i)(AgentDefaultModelConfig|ModelSelection|reasoningEffort|describe|ApiProxyDefaults|createApiProxy|selectionFor|routable)"
---

# 0272. the default model follows the picker — implementation context

## Open this when

A session model picker and a deployment default are two layers of the same preference. If the picker affects only its addressed session, the next blank session can select a different model with no user-facing way to align the default. If the default lives inside a Host gateway, direct Agent entry points cannot share it without depending on Host or duplicating state. Reasoning effort makes the persistence shape significant: a model selection without an effort must clear a stored effort, or the next Agent may apply an effort that its selected model does not accept.

## Source decision

AgentDefaultModelConfig provides ctx.agentDefaultModel and registers {provider, model, reasoningEffort?} as the agent-default-model Settings section. Its {provider, model} composition entry is the base layer and settings.yaml supplies the user layer. The service is entry-point-neutral, so direct creation and ApiProxy-backed creation share one default (headless direct core entry point). reasoningEffort belongs to the Settings section but not to the plugin config. Settings layers merge by field, so a configured effort would survive a user selection that omits it.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-07-default-model-follows-the-picker.md](../02-notes/implemented/feature/2026-08-07-default-model-follows-the-picker.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-07-default-model-follows-the-picker.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-07-default-model-follows-the-picker.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/agent-default-model/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-default-model/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-default-model`. Defines `AgentDefaultModelConfig`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent-default-model/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-default-model/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent-default-model`. | `named-package-member` |
| [`packages/core/agent-default-model`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-default-model) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `models`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/assembler.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/assembler.ts) | runtime implementation | Defines `blocks`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Defines `reasoningEffort`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/index.ts) | package entry point | Defines `ApiProxyService`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Defines `describe`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `ApiProxyDefaults`, a construct named by the note. Defines `createApiProxy`, a construct named by the note. | `symbol-definition` |
| [`packages/schedule/schedule/src/domain.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/domain.ts) | runtime implementation | Defines `prompt`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent/src/model-selection.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/model-selection.ts) | runtime implementation | Defines `ModelSelection`, a construct named by the note. | `symbol-definition` |
| [`packages/skill/skill-filesystem/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts) | package entry point | Defines `yaml`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `AgentDefaultModelConfig` | `class` | [`packages/core/agent-default-model/src/index.ts:64`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-default-model/src/index.ts#L64) | `export class AgentDefaultModelConfig extends Service {` |
| `ModelSelection` | `interface` | [`packages/core/agent/src/model-selection.ts:10`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/model-selection.ts#L10) | `export interface ModelSelection {` |
| `reasoningEffort` | `const` | [`packages/core/session/src/index.ts:266`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L266) | `const reasoningEffort = configRecord['reasoningEffort']` |
| `describe` | `function` | [`packages/core/tools/src/py-types.ts:223`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L223) | `function describe(schema: object): string \| undefined {` |
| `ApiProxyDefaults` | `interface` | [`packages/host/apiproxy/src/api-proxy.ts:636`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L636) | `export interface ApiProxyDefaults {` |
| `createApiProxy` | `function` | [`packages/host/apiproxy/src/api-proxy.ts:1106`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1106) | `export function createApiProxy(ctx: Context, defaults: ApiProxyDefaults): ApiProxy {` |
| `selectionFor` | `function` | [`packages/host/apiproxy/src/api-proxy.ts:1154`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1154) | `function selectionFor(agent: Agent): WebModelSelectionRef {` |
| `routable` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2278`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2278) | `const routable = routeServed(current.provider)` |
| `ApiProxyService` | `class` | [`packages/host/apiproxy/src/index.ts:69`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/index.ts#L69) | `export class ApiProxyService extends Service implements ApiProxy {` |
| `blocks` | `const` | [`packages/llm/llm/src/assembler.ts:135`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/assembler.ts#L135) | `const blocks = this.order.map(index => this.assemble(this.mustGet(index), index))` |
| `models` | `const` | [`packages/llm/llm/src/index.ts:547`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L547) | `const models: LlmDiscoveredModel[] = []` |
| `prompt` | `const` | [`packages/schedule/schedule/src/domain.ts:389`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/domain.ts#L389) | `const prompt = value['prompt']` |
| `yaml` | `const` | [`packages/skill/skill-filesystem/src/index.ts:917`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts#L917) | `const yaml = raw.slice(start, closing.start)` |

### Tests and executable evidence

- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `reasoningEffort`.
- [`packages/host/apiproxy/tests/rpc-schemas.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/rpc-schemas.spec.ts) — A test under the owning area exercises or imports `routable`.
- [`packages/core/session/tests/request-header.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/request-header.spec.ts) — A test under the owning area exercises or imports `reasoningEffort`.
- [`packages/host/apiproxy/tests/fetch-carrier.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/fetch-carrier.spec.ts) — A test under the owning area exercises or imports `routable`.
- [`packages/host/apiproxy/tests/api-proxy-fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-fork.spec.ts) — A test under the owning area exercises or imports `createApiProxy`.
- [`packages/host/apiproxy/tests/api-proxy-jobs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-jobs.spec.ts) — A test under the owning area exercises or imports `createApiProxy`.
- [`packages/host/apiproxy/tests/session-export.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/session-export.spec.ts) — A test under the owning area exercises or imports `createApiProxy`. A test under the owning area exercises or imports `ApiProxyService`.
- [`packages/host/apiproxy/tests/api-proxy-view.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-view.spec.ts) — A test under the owning area exercises or imports `createApiProxy`.

## How to read the implementation

1. Start with [`packages/core/agent-default-model/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-default-model/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/storage`, `lifecycle/implemented`, `mechanism/adapter`
- Aliases: `AgentDefaultModelConfig`, `ModelSelection`, `reasoningEffort`, `describe`, `ApiProxyDefaults`, `createApiProxy`, `selectionFor`, `routable`, `ApiProxyService`, `blocks`, `models`, `prompt`, `yaml`, `ctx.agentDefaultModel`
- Regex: `(?i)(AgentDefaultModelConfig|ModelSelection|reasoningEffort|describe|ApiProxyDefaults|createApiProxy|selectionFor|routable)`

```bash
rg -n --pcre2 "(?i)(AgentDefaultModelConfig|ModelSelection|reasoningEffort|describe|ApiProxyDefaults|createApiProxy|selectionFor|routable)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0115. headless is a direct core entry point](0115-headless-is-a-direct-core-entry-point.md): The source note links to this decision directly.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/tools/src/py-types.ts`.
- **`shares-code-with`** — [0651. Drop unconsumed assembled LLM convenience surfaces](0651-drop-unconsumed-assembled-llm-convenience-surfaces.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/llm/llm/src/assembler.ts`.
- **`shares-code-with`** — [0494. Property-based testing for protocol-shaped code](0494-property-based-testing-for-protocol-shaped-code.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/llm/llm/src/assembler.ts`.
- **`shares-code-with`** — [0205. Todo plan strip clears on the next turn](0205-todo-plan-strip-clears-on-the-next-turn.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0145. Explicit model-facing tool order](0145-explicit-model-facing-tool-order.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0079. Adapter-owned max-token defaults](0079-adapter-owned-max-token-defaults.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/llm/llm/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0272-the-default-model-follows-the-picker.md`.
