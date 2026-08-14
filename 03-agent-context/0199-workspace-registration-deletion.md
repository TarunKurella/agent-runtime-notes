---
id: "dsh-note-0199"
title: "Workspace Registration Deletion"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-27-workspace-registration-deletion.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/filesystem"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "workspaces"
  - "sessionIds"
  - "WorkspaceManager"
  - "list"
  - "Modal"
  - "workspaceIds"
  - "SessionPersistence"
  - "ctx.workspaceRegistry.delete"
  - "workspace.delete"
  - "workspace-not-found"
  - "workspace.list"
  - "host/workspace-removed"
  - "pendingMutation"
  - "host/workspace-changed"
search_regex: "(?i)(workspaces|sessionIds|WorkspaceManager|list|Modal|workspaceIds|SessionPersistence|ctx\\.workspaceRegistry\\.delete)"
---

# 0199. Workspace Registration Deletion — implementation context

## Open this when

A Workspace registers an existing code directory so the GUI can name it and order its Sessions. That record does not say that Harness created or owns the directory, and the Session log is an independent persistence object. Treating the row's Delete action as recursive source deletion or Session deletion would destroy data outside the record's ownership boundary. The existing visual-only menu row also left deletion semantics undefined across durable order, the Workspace table, Host streams, concurrent browser tabs, reconnect baselines, and a list request racing the mutation.

## Source decision

ctx.workspaceRegistry.delete(id) deletes only the Workspace registration: its id leaves durable workspaceIds, its workspaces table row and entity-cache entry disappear, and its ordered sessionIds account disappears with that row. It never calls filesystem removal or SessionPersistence; the directory, every user file, every live Session, and every persisted Session log remain. Because sidebar grouping is the complement of all surviving Workspace accounts, those Sessions immediately appear under Ungrouped, including the current Session. Unknown ids return false at the domain contract.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-27-workspace-registration-deletion.md](../02-notes/implemented/feature/2026-07-27-workspace-registration-deletion.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-27-workspace-registration-deletion.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-27-workspace-registration-deletion.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `workspaceIds`, a construct named by the note. Defines `workspaces`, a construct named by the note. | `symbol-definition` |
| [`packages/workspace/workspace/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts) | package entry point | Defines `workspaceIds`, a construct named by the note. Defines `sessionIds`, a construct named by the note. | `symbol-definition` |
| [`packages/hooks/hook-protocol/src/merge.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/workspace/workspace/src/entity.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/entity.ts) | runtime implementation | Defines `sessionIds`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/Menu.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Menu.tsx) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/slots.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/slots.ts) | runtime implementation | Defines `workspaces`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/index.ts) | package entry point | Defines `workspaces`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/Modal.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Modal.tsx) | runtime implementation | Defines `Modal`, a construct named by the note. | `symbol-definition` |
| [`packages/client/web-react/src/scoped-slots.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/scoped-slots.tsx) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `workspaces`, a construct named by the note. Defines `sessionIds`, a construct named by the note. | `exact-code-occurrence, symbol-definition` |
| [`packages/session/session-persistence/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/index.ts) | package entry point | Defines `SessionPersistence`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `workspaces` | `const` | [`packages/client/connection/src/client/fixture.ts:1554`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1554) | `const workspaces: WorkspaceView[] = options.empty ? [] : [{` |
| `sessionIds` | `const` | [`packages/client/connection/src/client/fixture.ts:2679`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L2679) | `const sessionIds = [...without.slice(0, at), sessionId, ...without.slice(at)]` |
| `workspaces` | `const` | [`packages/client/runtime/src/client/index.ts:199`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/index.ts#L199) | `const workspaces = new WorkspaceRuntime(ctx, connection.api, sessions)` |
| `workspaces` | `const` | [`packages/client/runtime/src/client/slots.ts:392`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/slots.ts#L392) | `const workspaces = this.ctx.get('workspaces')` |
| `WorkspaceManager` | `class` | [`packages/client/runtime/src/client/workspaces/manager.ts:35`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/workspaces/manager.ts#L35) | `export class WorkspaceManager {` |
| `list` | `const` | [`packages/client/ui-commands/src/client/service.ts:244`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/service.ts#L244) | `const list = await this.directory.ensureReady(session.sessionId, req.signal)` |
| `workspaces` | `const` | [`packages/client/ui-conversation/src/client/apply.ts:117`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts#L117) | `const workspaces = ctx.workspaces` |
| `list` | `const` | [`packages/client/ui-primitives/src/Menu.tsx:265`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Menu.tsx#L265) | `const list = open && (` |
| `Modal` | `function` | [`packages/client/ui-primitives/src/Modal.tsx:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Modal.tsx#L30) | `export function Modal({` |
| `list` | `let` | [`packages/client/web-react/src/scoped-slots.tsx:839`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/scoped-slots.tsx#L839) | `let list = [...rows].sort((a, b) => a.order - b.order)` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `workspaces` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1513`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1513) | `const workspaces = ctx.workspaceRegistry.list()` |
| `workspaceIds` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2871`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2871) | `const workspaceIds = await ctx.workspaceRegistry.insertBefore(` |
| `SessionPersistence` | `class` | [`packages/session/session-persistence/src/index.ts:84`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/index.ts#L84) | `export abstract class SessionPersistence extends Service {` |
| `list` | `let` | [`packages/typert/generator/src/cordis-catalog.ts:433`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts#L433) | `let list: string[] = []` |
| `sessionIds` | `const` | [`packages/workspace/workspace/src/entity.ts:167`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/entity.ts#L167) | `const sessionIds = [...without.slice(0, at), sessionId, ...without.slice(at)]` |

### Tests and executable evidence

- [`packages/host/apiproxy/tests/rpc-schemas.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/rpc-schemas.spec.ts) — A test under the owning area exercises or imports `workspaceIds`. A test under the owning area exercises or imports `workspace-not-found`.
- [`packages/host/apiproxy/tests/fetch-carrier.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/fetch-carrier.spec.ts) — A test under the owning area exercises or imports `workspaceIds`.
- [`packages/host/apiproxy/tests/client-handler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/client-handler.spec.ts) — A test under the owning area exercises or imports `workspaceIds`.
- [`packages/workspace/workspace/tests/workspace.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/tests/workspace.spec.ts) — A test under the owning area exercises or imports `workspaceIds`. A test under the owning area exercises or imports `pendingMutation`.
- [`packages/session/session-persistence/tests/contract.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/tests/contract.ts) — A test under the owning area exercises or imports `SessionPersistence`.
- [`packages/client/connection/tests/fixture.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/tests/fixture.client.spec.ts) — A test under the owning area exercises or imports `workspace-not-found`. Contains the exact code literal `host/workspace-removed` named by the note.
- [`packages/host/apiproxy/tests/api-proxy-workspace.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-workspace.spec.ts) — A test under the owning area exercises or imports `workspaceIds`. A test under the owning area exercises or imports `workspace-not-found`.
- [`packages/client/ui-primitives/tests/atoms.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/atoms.client.spec.tsx) — A test under the owning area exercises or imports `Modal`.
- Source verification intent: Workspace package tests pin successful metadata-only deletion, same-path re-registration, unknown-id idempotence, table-failure rollback, explicit-marker restart recovery, unexplained-corruption rejection, and cache/table invariant behavior. Apiproxy and carrier tests pin the schema, handler, workspace-not-found, retained Session/folder, fresh-id re-registration, and committed host/workspace-removed frame. Client tests pin unary direct echo, duplicate removal, late changed frames, and deletion racing an in-flight baseline.

## How to read the implementation

1. Start with [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/filesystem`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `workspaces`, `sessionIds`, `WorkspaceManager`, `list`, `Modal`, `workspaceIds`, `SessionPersistence`, `ctx.workspaceRegistry.delete`, `workspace.delete`, `workspace-not-found`, `workspace.list`, `host/workspace-removed`, `pendingMutation`, `host/workspace-changed`
- Regex: `(?i)(workspaces|sessionIds|WorkspaceManager|list|Modal|workspaceIds|SessionPersistence|ctx\.workspaceRegistry\.delete)`

```bash
rg -n --pcre2 "(?i)(workspaces|sessionIds|WorkspaceManager|list|Modal|workspaceIds|SessionPersistence|ctx\\.workspaceRegistry\\.delete)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0232. Session archive (registry-global set)](0232-session-archive-registry-global-set.md): Shares source implementation: `packages/client/ui-primitives/src/Menu.tsx`, `packages/client/web-react/src/scoped-slots.tsx`.
- **`shares-code-with`** — [0183. Session List Browsing and Manual Workspace Order](0183-session-list-browsing-and-manual-workspace-order.md): Shares source implementation: `packages/client/ui-primitives/src/Menu.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0303. Bind JSONL session identity before mutation](0303-bind-jsonl-session-identity-before-mutation.md): Shares source implementation: `packages/client/ui-primitives/src/Menu.tsx`, `packages/client/web-react/src/scoped-slots.tsx`.
- **`shares-code-with`** — [0652. Prune dead methods from the persistence seam](0652-prune-dead-methods-from-the-persistence-seam.md): Shares source implementation: `packages/client/ui-primitives/src/Menu.tsx`, `packages/client/web-react/src/scoped-slots.tsx`.
- **`shares-code-with`** — [0509. Domain KV storage capability seam and the workspace entity](0509-domain-kv-storage-capability-seam-and-the-workspace-entity.md): Shares source implementation: `packages/workspace/workspace/src/entity.ts`, `packages/workspace/workspace/src/index.ts`.
- **`shares-code-with`** — [0297. Workspace Sidebar Order and Folding](0297-workspace-sidebar-order-and-folding.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/workspace/workspace/src/entity.ts`.
- **`shares-code-with`** — [0345. Workspace New Session reuse hijacked cwd-matching unaccounted blank sessions](0345-workspace-new-session-reuse-hijacked-cwd-matching-unaccounted-blank-sess.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/workspace/workspace/src/entity.ts`.
- **`shares-code-with`** — [0080. Full client copy rollout onto the typed locale seat, and the non-translation boundary](0080-full-client-copy-rollout-onto-the-typed-locale-seat-and-the-non-translat.md): Shares source implementation: `packages/client/ui-primitives/src/Modal.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0199-workspace-registration-deletion.md`.
