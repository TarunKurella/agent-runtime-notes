---
id: "dsh-note-0511"
title: "Session projections and command lifecycle logging"
status: "proposed"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/architecture/2026-07-27-session-projection-and-command-log.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
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
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/proposed"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "backscanTodos"
  - "apiProxy"
  - "ConversationSnapshot"
  - "invalidate"
  - "subagent"
  - "bindSnapshotSelector"
  - "snapshot"
  - "seq"
  - "apply"
  - "view"
  - "GoalSnapshot"
  - "viewFor"
  - "projections"
  - "active"
search_regex: "(?i)(backscanTodos|apiProxy|ConversationSnapshot|invalidate|subagent|bindSnapshotSelector|snapshot|apply)"
---

# 0511. Session projections and command lifecycle logging — implementation context

## Open this when

Three in-flight web features --- todo (#497), goal (#527), and plan mode (#587) --- each derive per-session state from the session log and surface it in the browser client, and each invented its own copy of the same machinery: The client core class absorbs every domain. All three add private fields, fetch choreography, and event switches to the client runtime's Session class and project their values through ConversationSnapshot.

## Source decision

Four infrastructure pieces, then the domains become pure contributors. A state-carrying log event MUST carry the complete post-change state, never a bare delta. All three domains already comply: todo/write is a whole-list snapshot, plan/mode a whole boolean, goal/change metadata a full GoalSnapshot (or a whole-value clear tombstone). The rule keeps every domain's transition trivially cheap (the framework drives it per event), keeps values self-describing on the wire, and lets any consumer treat the latest pushed value as final --- out-of-order immunity by seq comparison, self-healing because a missed update is.

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/architecture/2026-07-27-session-projection-and-command-log.md](../02-notes/proposed/architecture/2026-07-27-session-projection-and-command-log.md)
- Pinned source: [.agents/notes/proposed/architecture/2026-07-27-session-projection-and-command-log.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/architecture/2026-07-27-session-projection-and-command-log.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. Defines `view`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. Defines `seq`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/tools/src/testing.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/testing.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/session/src/repair.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `seq`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/host/apiproxy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/host/apiproxy`. | `named-package-member` |
| [`packages/core/tools/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/tools`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/todo/tool-todo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/index.ts) | package entry point | Core file in the package named by the note: `packages/todo/tool-todo`. Defines `todos`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/todo/tool-todo/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/todo/tool-todo`. Defines `SessionProjectionMap`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `backscanTodos` | `function` | [`packages/client/connection/src/client/fixture.ts:1360`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1360) | `function backscanTodos(log: readonly SessionEvent[]): TodoItem[] \| undefined {` |
| `apiProxy` | `const` | [`packages/client/connection/src/index.ts:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/index.ts#L156) | `const apiProxy = ctx.get('apiProxy')` |
| `ConversationSnapshot` | `interface` | [`packages/client/runtime/src/client/sessions/conversation.ts:433`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts#L433) | `export interface ConversationSnapshot {` |
| `invalidate` | `const` | [`packages/client/ui-skill/src/client/index.ts:117`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-skill/src/client/index.ts#L117) | `const invalidate = (key: SessionId): void => {` |
| `subagent` | `const` | [`packages/client/ui-subagent/src/client/index.ts:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-subagent/src/client/index.ts#L45) | `const subagent = owner.session?.subagent` |
| `bindSnapshotSelector` | `function` | [`packages/client/web-react/src/bind.ts:18`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/bind.ts#L18) | `export function bindSnapshotSelector<T>(w: HostObservable<T>): SnapshotSelectorHook<T> {` |
| `snapshot` | `const` | [`packages/core/session/src/index.ts:154`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L154) | `const snapshot = snapshotJsonValue(input)` |
| `seq` | `const` | [`packages/core/session/src/index.ts:233`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L233) | `const seq = event['seq']` |
| `snapshot` | `const` | [`packages/core/session/src/index.ts:519`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L519) | `const snapshot = mode === 'restore' ? source : snapshotJsonValue(source)` |
| `apply` | `const` | [`packages/core/session/src/invariant.ts:249`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts#L249) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `seq` | `let` | [`packages/core/session/src/repair.ts:85`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts#L85) | `let seq = last.seq + 1` |
| `view` | `const` | [`packages/core/tools/src/index.ts:981`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L981) | `const view = this.view(scope)` |
| `apply` | `const` | [`packages/core/tools/src/invariant.ts:127`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts#L127) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `GoalSnapshot` | `interface` | [`packages/goal/goal/src/types.ts:59`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/types.ts#L59) | `export interface GoalSnapshot extends GoalRef {` |
| `viewFor` | `function` | [`packages/host/apiproxy/src/api-proxy.ts:744`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L744) | `function viewFor(` |
| `view` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:758`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L758) | `const view = ctx.tools.get(name, scope)?.presentCall?.(JSON.parse(raw))` |

### Tests and executable evidence

- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `projections`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `todos`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `restore`.
- [`packages/core/session/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/invariant.spec.ts) — A test under the owning area exercises or imports `todos`.
- [`packages/core/session/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/properties.spec.ts) — A test under the owning area exercises or imports `projections`.
- [`packages/todo/tool-todo/tests/tool-todo.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/tool-todo.spec.ts) — A test under the owning area exercises or imports `todos`. A test under the owning area exercises or imports `tool-todo`.
- [`packages/todo/tool-todo/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/invariant.spec.ts) — A test under the owning area exercises or imports `todos`.
- [`packages/todo/tool-todo/tests/projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/projection.spec.ts) — A test under the owning area exercises or imports `todos`. A test under the owning area exercises or imports `dsh-session-projection`.
- Source verification intent: A domain plugin ships per-session log-derived state to React by writing only: the whole-value event declaration, one host unit register, its SessionProjectionMap merge, and inject callbacks --- zero client-side code, no edits to the client Session class, ConversationSnapshot, api-proxy, or the wire schema files. The history tail page carries projections with asOfSeq equal to the window tail seq; loadOlder pages never carry it; a deployment without the registry serves histories without the block and clients treat every key as absent.

## How to read the implementation

1. Start with [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/agent-loop`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/proposed`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `backscanTodos`, `apiProxy`, `ConversationSnapshot`, `invalidate`, `subagent`, `bindSnapshotSelector`, `snapshot`, `seq`, `apply`, `view`, `GoalSnapshot`, `viewFor`, `projections`, `active`
- Regex: `(?i)(backscanTodos|apiProxy|ConversationSnapshot|invalidate|subagent|bindSnapshotSelector|snapshot|apply)`

```bash
rg -n --pcre2 "(?i)(backscanTodos|apiProxy|ConversationSnapshot|invalidate|subagent|bindSnapshotSelector|snapshot|apply)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0106. Subagent list identity via the projection unit](0106-subagent-list-identity-via-the-projection-unit.md): The source note links to this decision directly.
- **`source-link`** — [0466. Remove synthetic turns for log-only events](0466-remove-synthetic-turns-for-log-only-events.md): The source note links to this decision directly.
- **`shares-code-with`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0165. Typed tool returns in Code Mode](0165-typed-tool-returns-in-code-mode.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0137. The `todo_write` tool --- model task list as event-sourced session state](0137-the-todo-write-tool-model-task-list-as-event-sourced-session-state.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0032. Tool output spill policy](0032-tool-output-spill-policy.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0113. Client Conversation business-node assembly and keyed Chat snapshots](0113-client-conversation-business-node-assembly-and-keyed-chat-snapshots.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/repair.ts`.
- **`shares-code-with`** — [0187. Code Mode UI foundation --- run_code description and native-parity dispatch logging](0187-code-mode-ui-foundation-run-code-description-and-native-parity-dispatch.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/core/tools/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0511-session-projections-and-command-lifecycle-logging.md`.
