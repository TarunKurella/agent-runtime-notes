---
id: "dsh-note-0173"
title: "Durable subagent catalog and list_agents"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-22-durable-subagent-catalog-and-list-agents.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
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
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "readEvent"
  - "agents"
  - "subagents"
  - "activity"
  - "subagent"
  - "AgentStatus"
  - "scope"
  - "SessionId"
  - "SessionHeader"
  - "inspect"
  - "createdAt"
  - "sessionQuery"
  - "traceSession"
  - "running"
search_regex: "(?i)(readEvent|agents|subagents|activity|subagent|AgentStatus|scope|SessionId)"
---

# 0173. Durable subagent catalog and list_agents — implementation context

## Open this when

Continuable background subagents expose a stable child id and persist reconstruction data in that child's session, so send_message can resume a known child without any listing operation. Discovery has two consumers with different needs: a UI may show both one-shot work and continuable conversations, while the model should receive only children on which send_message is meaningful. The durable Session and Activation design is owned by continuable subagents; this note owns the shared durable inventory and the model-facing projection.

## Source decision

Superseded read path. Subagent list identity via the projection unit replaces this note's enumeration and per-child read design: listChildren now merges the live session store with optional session persistence directly and serves each child's mode/label from the registered subagent projection unit --- no session-query dependency, no list-time descriptor scan --- and that note owns the current listing semantics, including the diagnostic mapping.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-22-durable-subagent-catalog-and-list-agents.md](../02-notes/implemented/feature/2026-07-22-durable-subagent-catalog-and-list-agents.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-22-durable-subagent-catalog-and-list-agents.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-22-durable-subagent-catalog-and-list-agents.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/subagent/subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts) | package entry point | The source note names this file directly. Core file in the package named by the note: `packages/subagent/subagent`. | `named-file, named-package-member, symbol-definition` |
| [`packages/core/scope/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/scope`. Defines `scope`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/scope/src/store.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/store.ts) | runtime implementation | Core file in the package named by the note: `packages/core/scope`. Defines `scope`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/scope/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/scope`. | `named-package-member` |
| [`packages/subagent/subagent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/src/descriptor.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/descriptor.ts) | runtime implementation | Core file in the package named by the note: `packages/subagent/subagent`. Defines `label`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/subagent/src/continuation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/continuation.ts) | runtime implementation | Core file in the package named by the note: `packages/subagent/subagent`. Defines `idle`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/subagent/src/list-children.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/list-children.ts) | runtime implementation | Core file in the package named by the note: `packages/subagent/subagent`. Defines `listChildren`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/tool-subagent-control/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/tool-subagent-control`. | `named-package-member` |
| [`packages/subagent/tool-subagent-control/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/tool-subagent-control`. | `named-package-member` |
| [`packages/subagent/tool-subagent-control/src/list-agents.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control/src/list-agents.ts) | runtime implementation | Core file in the package named by the note: `packages/subagent/tool-subagent-control`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `readEvent` | `function` | [`.github/issue-management/policy.mjs:689`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/issue-management/policy.mjs#L689) | `function readEvent() {` |
| `agents` | `const` | [`packages/acp/acp/src/index.ts:108`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L108) | `const agents = ctx.agents` |
| `subagents` | `const` | [`packages/acp/acp/src/index.ts:376`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L376) | `const subagents = ctx.get('subagents') as ContinuableDrain \| undefined` |
| `activity` | `const` | [`packages/client/runtime/src/client/sessions/manager.ts:930`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts#L930) | `const activity = running ? 'running' as const : 'inactive' as const` |
| `subagent` | `const` | [`packages/client/ui-subagent/src/client/index.ts:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-subagent/src/client/index.ts#L45) | `const subagent = owner.session?.subagent` |
| `AgentStatus` | `type` | [`packages/core/agent/src/runtime-types.ts:50`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/runtime-types.ts#L50) | `export type AgentStatus = 'idle' \| 'running'` |
| `scope` | `function` | [`packages/core/scope/src/index.ts:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts#L121) | `function scope(): void {}` |
| `scope` | `const` | [`packages/core/scope/src/store.ts:231`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/store.ts#L231) | `const scope = scopeOf(ctx)` |
| `SessionId` | `type` | [`packages/core/session/src/types.ts:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L22) | `export type SessionId = Branded<'SessionId'>` |
| `SessionHeader` | `interface` | [`packages/core/session/src/types.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L61) | `export interface SessionHeader {` |
| `inspect` | `const` | [`packages/extensions/cordis-client-runner/src/client/index.ts:189`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-client-runner/src/client/index.ts#L189) | `const inspect = new ClientCordisInspectRegistry({` |
| `createdAt` | `const` | [`packages/goal/goal/src/fold.ts:160`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L160) | `const createdAt = nonNegativeInteger(value['createdAt'], 'createdAt')` |
| `sessionQuery` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2043`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2043) | `const sessionQuery = ctx.get('sessionQuery')` |
| `traceSession` | `function` | [`packages/session-query/session-query/src/tracing.ts:113`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/tracing.ts#L113) | `export function traceSession(` |
| `running` | `const` | [`packages/shell/bash-local/src/index.ts:257`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts#L257) | `const running = this.ctx.subprocess.spawn(this.spawnSpec(spec, argv, this.config.maxOutputBytes, spec.signal))` |
| `description` | `const` | [`packages/skill/skill/src/index.ts:751`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L751) | `const description = skill.description` |

### Tests and executable evidence

- [`packages/subagent/subagent/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/service.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `SubagentError`.
- [`packages/subagent/subagent/tests/list-children.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/list-children.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `listChildren`.
- [`packages/subagent/tool-subagent-control/tests/list-agents.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control/tests/list-agents.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `send_message`.
- [`packages/subagent/subagent-in-process-driver/tests/subagent-in-process-driver.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-in-process-driver/tests/subagent-in-process-driver.spec.ts) — The source note names this file directly.
- [`packages/core/agent/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/agent.spec.ts) — A test under the owning area exercises or imports `AgentStatus`.
- [`packages/host/apiproxy/tests/api-proxy-fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-fork.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/host/apiproxy/tests/session-export.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/session-export.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/client/runtime/tests/manager.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/manager.client.spec.ts) — A test under the owning area exercises or imports `activity`.
- Source verification intent: packages/subagent/subagent/tests/service.spec.ts pins descriptor v2 parsing for both modes and proves an unlabeled raw start resolves a one-shot descriptor before provider dispatch. packages/subagent/subagent-in-process-driver/tests/subagent-in-process-driver.spec.ts proves the local driver appends that descriptor inside the initial turn, returns the published id when cancellation lands in the factory-to-run handoff, and keeps result and handle-disposal failures on separate channels.

## How to read the implementation

1. Start with [`packages/subagent/subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `readEvent`, `agents`, `subagents`, `activity`, `subagent`, `AgentStatus`, `scope`, `SessionId`, `SessionHeader`, `inspect`, `createdAt`, `sessionQuery`, `traceSession`, `running`
- Regex: `(?i)(readEvent|agents|subagents|activity|subagent|AgentStatus|scope|SessionId)`

```bash
rg -n --pcre2 "(?i)(readEvent|agents|subagents|activity|subagent|AgentStatus|scope|SessionId)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0106. Subagent list identity via the projection unit](0106-subagent-list-identity-via-the-projection-unit.md): The source note links to this decision directly.
- **`source-link`** — [0348. `list_agents` uses `ready` for resumable children](0348-list-agents-uses-ready-for-resumable-children.md): The source note links to this decision directly.
- **`source-link`** — [0200. Continuable subagents](0200-continuable-subagents.md): The source note links to this decision directly.
- **`source-link`** — [0264. Continuable subagent current-turn interrupt](0264-continuable-subagent-current-turn-interrupt.md): The source note links to this decision directly.
- **`shares-code-with`** — [0035. Agent-scope runtime design and correctness](0035-agent-scope-runtime-design-and-correctness.md): Shares source implementation: `packages/core/scope/src/index.ts`, `packages/core/scope/src/invariant.ts`.
- **`shares-code-with`** — [0049. Canonical tool output contract](0049-canonical-tool-output-contract.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/invariant.ts`.
- **`shares-code-with`** — [0036. Shared scoped-layer storage](0036-shared-scoped-layer-storage.md): Shares source implementation: `packages/core/scope/src/index.ts`, `packages/core/scope/src/invariant.ts`.
- **`shares-code-with`** — [0357. Child agents join their parent's preset composition](0357-child-agents-join-their-parent-s-preset-composition.md): Shares source implementation: `packages/core/scope/src/index.ts`, `packages/core/scope/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0173-durable-subagent-catalog-and-list-agents.md`.
