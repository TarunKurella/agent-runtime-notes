---
id: "dsh-note-0185"
title: "Workspace UI Complete Product Flow"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-25-workspace-ui-product-flow.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "SessionManager"
  - "SessionRuntime"
  - "WorkspaceManager"
  - "WorkspaceRuntime"
  - "useSessions"
  - "inspect"
  - "cwd"
  - "createdAt"
  - "updatedAt"
  - "list"
  - "workspace"
  - "sessionIds"
  - "workspaceId"
  - "Workspace"
search_regex: "(?i)(SessionManager|SessionRuntime|WorkspaceManager|WorkspaceRuntime|useSessions|inspect|createdAt|updatedAt)"
---

# 0185. Workspace UI Complete Product Flow — implementation context

## Open this when

Domain KV Storage and the Workspace Entity defines the persistent Workspace entity, path conventions, and ordered Session ledger, but not the Host wiring, historical-data initialization, or GUI flow. The GUI presents both Workspaces and Sessions; users must be able to type immediately after entering New Session, even when no Host Session or Host Workspace exists yet. Pending Workspaces, pending Sessions, retained input, and Host entity publication need clear owners and must preserve the same page identity when RPC completions and Host frames arrive in either order.

## Source decision

The Host provides the following GUI wiring on the Workspace entity: The Host stream pushes Workspace and Session deltas, including host/workspace-removed, and the Client refreshes the workspace.list and session.list baselines separately after reconnecting. Registration-deletion ownership and safety are defined in the Workspace registration deletion Agent Note. A Workspace's sessionIds is an ordered candidate index. A membership projection requires both that an id appear in the index and that the corresponding canonicalized SessionHeader.cwd equal the Workspace path; SessionHeader does not gain a workspaceId.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-25-workspace-ui-product-flow.md](../02-notes/implemented/feature/2026-07-25-workspace-ui-product-flow.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-25-workspace-ui-product-flow.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-25-workspace-ui-product-flow.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/session) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `createdAt`, a construct named by the note. Defines `updatedAt`, a construct named by the note. | `symbol-definition` |
| [`packages/client/web/src/app.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/app.tsx) | runtime implementation | Defines `useSessions`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/src/session-cwd.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts) | runtime implementation | Defines `cwd`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `workspace`, a construct named by the note. | `symbol-definition` |
| [`packages/workspace/workspace/src/spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/spec.ts) | runtime implementation | Defines `workspaceId`, a construct named by the note. | `symbol-definition` |
| [`packages/hooks/hook-protocol/src/merge.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/workspace/workspace/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts) | package entry point | Defines `sessionIds`, a construct named by the note. | `symbol-definition` |
| [`packages/workspace/workspace/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/types.ts) | public types and contract | Defines `Workspace`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `SessionManager` | `class` | [`packages/client/runtime/src/client/sessions/manager.ts:106`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts#L106) | `export class SessionManager {` |
| `SessionRuntime` | `class` | [`packages/client/runtime/src/client/sessions/service.ts:229`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/service.ts#L229) | `export class SessionRuntime implements ISessions {` |
| `WorkspaceManager` | `class` | [`packages/client/runtime/src/client/workspaces/manager.ts:35`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/workspaces/manager.ts#L35) | `export class WorkspaceManager {` |
| `WorkspaceRuntime` | `class` | [`packages/client/runtime/src/client/workspaces/service.ts:51`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/workspaces/service.ts#L51) | `export class WorkspaceRuntime implements IWorkspaces {` |
| `useSessions` | `const` | [`packages/client/web/src/app.tsx:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/app.tsx#L30) | `const useSessions = bindSnapshotSelector(sessions.list)` |
| `inspect` | `const` | [`packages/extensions/cordis-client-runner/src/client/index.ts:189`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-client-runner/src/client/index.ts#L189) | `const inspect = new ClientCordisInspectRegistry({` |
| `cwd` | `const` | [`packages/fs/tool-fs/src/session-cwd.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts#L24) | `const cwd = exec.agent?.session.header.cwd` |
| `createdAt` | `const` | [`packages/goal/goal/src/fold.ts:160`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L160) | `const createdAt = nonNegativeInteger(value['createdAt'], 'createdAt')` |
| `updatedAt` | `const` | [`packages/goal/goal/src/fold.ts:161`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L161) | `const updatedAt = nonNegativeInteger(value['updatedAt'], 'updatedAt')` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `workspace` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1519`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1519) | `const workspace = workspaces.find(candidate => candidate.sessionIds.includes(ancestor.header.id))` |
| `sessionIds` | `const` | [`packages/workspace/workspace/src/index.ts:454`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts#L454) | `const sessionIds = group.headers` |
| `workspaceId` | `const` | [`packages/workspace/workspace/src/spec.ts:14`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/spec.ts#L14) | `const workspaceId = z.string().transform(value => value as WorkspaceId)` |
| `Workspace` | `interface` | [`packages/workspace/workspace/src/types.ts:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/types.ts#L23) | `export interface Workspace {` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model`.
- [`packages/client/runtime/tests/manager.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/manager.client.spec.ts) — A test under the owning area exercises or imports `SessionManager`.
- [`packages/client/runtime/tests/queue-store.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/queue-store.client.spec.ts) — A test under the owning area exercises or imports `SessionManager`.
- [`packages/client/runtime/tests/client-apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/client-apply.client.spec.ts) — A test under the owning area exercises or imports `SessionRuntime`. A test under the owning area exercises or imports `WorkspaceRuntime`.
- [`packages/client/runtime/tests/projection-store.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/projection-store.client.spec.ts) — A test under the owning area exercises or imports `SessionManager`.
- [`packages/client/runtime/tests/sessions-service.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/sessions-service.client.spec.ts) — A test under the owning area exercises or imports `SessionRuntime`.
- [`packages/client/runtime/tests/workspaces-service.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/workspaces-service.client.spec.ts) — A test under the owning area exercises or imports `WorkspaceManager`. A test under the owning area exercises or imports `SessionRuntime`.
- Source verification intent: The zero state with no Workspace writes nothing to the Host and accepts input; explicit Create Workspace immediately creates and displays an empty Workspace. Frontend Sessions and Workspaces preserve object identity across materialization; input, errors, focus, and sidebar projections always originate from the object layer. The first send advances through Workspace, Session, and prompt in order; successful stages are not rolled back, input is not lost before the prompt is accepted, and creation retries use the same SessionId.

## How to read the implementation

1. Start with [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `SessionManager`, `SessionRuntime`, `WorkspaceManager`, `WorkspaceRuntime`, `useSessions`, `inspect`, `cwd`, `createdAt`, `updatedAt`, `list`, `workspace`, `sessionIds`, `workspaceId`, `Workspace`
- Regex: `(?i)(SessionManager|SessionRuntime|WorkspaceManager|WorkspaceRuntime|useSessions|inspect|createdAt|updatedAt)`

```bash
rg -n --pcre2 "(?i)(SessionManager|SessionRuntime|WorkspaceManager|WorkspaceRuntime|useSessions|inspect|createdAt|updatedAt)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0332. Same-basename Workspace adoption](0332-same-basename-workspace-adoption.md): The source note links to this decision directly.
- **`source-link`** — [0199. Workspace Registration Deletion](0199-workspace-registration-deletion.md): The source note links to this decision directly.
- **`source-link`** — [0297. Workspace Sidebar Order and Folding](0297-workspace-sidebar-order-and-folding.md): The source note links to this decision directly.
- **`source-link`** — [0472. One route to add a Workspace](0472-one-route-to-add-a-workspace.md): The source note links to this decision directly.
- **`shares-code-with`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0446. Drop the mutable session summary](0446-drop-the-mutable-session-summary.md): Shares source implementation: `packages/client/web/src/app.tsx`, `packages/core/session`.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0061. Web client Agent-scope parity model and the provisioning channel (agents/scope / blank reuse / provide)](0061-web-client-agent-scope-parity-model-and-the-provisioning-channel-agents.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0185-workspace-ui-complete-product-flow.md`.
