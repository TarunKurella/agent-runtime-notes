---
id: "dsh-note-0232"
title: "Session archive (registry-global set)"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-31-session-archive-global-set.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/session-state"
  - "domain/storage"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "archivedSessionIds"
  - "sessionIds"
  - "archived"
  - "list"
  - "sessionVisible"
  - "deriveGroups"
  - "deriveFlat"
  - "workspaceView"
  - "archive"
  - "WorkspaceUnknownSessionError"
  - "workspaceDomainState.archivedSessionIds"
  - "tree.ts"
  - "archivedSessionIds: z.array(sessionId).default"
  - "ctx.workspaceRegistry.archiveSession"
search_regex: "(?i)(archivedSessionIds|sessionIds|archived|list|sessionVisible|deriveGroups|deriveFlat|workspaceView)"
---

# 0232. Session archive (registry-global set) — implementation context

## Open this when

The session row menu in the sidebar workspace browser carried a purely visual "Delete session" placeholder (no handler). The product decision is archive, not delete: the session log and its workspace accounting stay untouched; the session merely disappears from every grouping surface (workspace groups, Ungrouped, search, the flat list). The archive record needs a home: an Ungrouped session belongs to no workspace entity, so a per-workspace field cannot carry it.

## Source decision

The archive set is a new field on the workspace domain's global singleton (workspaceDomainState.archivedSessionIds), layered over workspace accounting; display filtering converges entirely in the client's tree.ts derivation layer; the wire surface uses the full-snapshot posture. Storage: archivedSessionIds: z.array(sessionId).default([]), domain version stays 2 --- a purely additive field; pre-field media parse to an empty set through the schema default, no migration code.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-31-session-archive-global-set.md](../02-notes/implemented/feature/2026-07-31-session-archive-global-set.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-31-session-archive-global-set.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-31-session-archive-global-set.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/archived-agent-notes.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/archived-agent-notes.ts) | repository automation | Defines `archived`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `archivedSessionIds`, a construct named by the note. Defines `workspaceView`, a construct named by the note. | `symbol-definition` |
| [`packages/workspace/workspace/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts) | package entry point | Defines `sessionIds`, a construct named by the note. Defines `WorkspaceUnknownSessionError`, a construct named by the note. | `symbol-definition` |
| [`packages/hooks/hook-protocol/src/merge.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/workspace/workspace/src/entity.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/entity.ts) | runtime implementation | Defines `sessionIds`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/Menu.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Menu.tsx) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/session-export.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/session-export.ts) | runtime implementation | Defines `archive`, a construct named by the note. | `symbol-definition` |
| [`packages/client/web-react/src/scoped-slots.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/scoped-slots.tsx) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-workspace/src/client/tree.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/tree.ts) | runtime implementation | Defines `sessionVisible`, a construct named by the note. Defines `deriveGroups`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `archivedSessionIds`, a construct named by the note. Defines `sessionIds`, a construct named by the note. | `exact-code-occurrence, symbol-definition` |
| [`packages/client/ui-commands/src/client/service.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/service.ts) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `archivedSessionIds` | `const` | [`packages/client/connection/src/client/fixture.ts:1565`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1565) | `const archivedSessionIds: SessionId[] = []` |
| `sessionIds` | `const` | [`packages/client/connection/src/client/fixture.ts:2679`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L2679) | `const sessionIds = [...without.slice(0, at), sessionId, ...without.slice(at)]` |
| `archived` | `const` | [`packages/client/runtime/src/client/workspaces/service.ts:104`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/workspaces/service.ts#L104) | `const archived = this.list.getSnapshot().archivedSessionIds` |
| `list` | `const` | [`packages/client/ui-commands/src/client/service.ts:244`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/service.ts#L244) | `const list = await this.directory.ensureReady(session.sessionId, req.signal)` |
| `list` | `const` | [`packages/client/ui-primitives/src/Menu.tsx:265`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Menu.tsx#L265) | `const list = open && (` |
| `archivedSessionIds` | `const` | [`packages/client/ui-workspace/src/client/WorkspaceBrowser.tsx:766`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/WorkspaceBrowser.tsx#L766) | `const archivedSessionIds = useWorkspaces(state => state.archivedSessionIds)` |
| `sessionVisible` | `function` | [`packages/client/ui-workspace/src/client/tree.ts:118`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/tree.ts#L118) | `function sessionVisible(session: SessionSummary, current: SessionId \| undefined, archived: ReadonlySet<SessionId>): boolean {` |
| `deriveGroups` | `function` | [`packages/client/ui-workspace/src/client/tree.ts:244`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/tree.ts#L244) | `export function deriveGroups(` |
| `archived` | `const` | [`packages/client/ui-workspace/src/client/tree.ts:250`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/tree.ts#L250) | `const archived = new Set(archivedSessionIds)` |
| `deriveFlat` | `function` | [`packages/client/ui-workspace/src/client/tree.ts:284`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/tree.ts#L284) | `export function deriveFlat(` |
| `archived` | `const` | [`packages/client/ui-workspace/src/client/tree.ts:288`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/tree.ts#L288) | `const archived = new Set(archivedSessionIds)` |
| `archived` | `const` | [`packages/client/ui-workspace/src/client/tree.ts:331`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/tree.ts#L331) | `const archived = new Set(archivedSessionIds)` |
| `list` | `let` | [`packages/client/web-react/src/scoped-slots.tsx:839`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/scoped-slots.tsx#L839) | `let list = [...rows].sort((a, b) => a.order - b.order)` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `workspaceView` | `function` | [`packages/host/apiproxy/src/api-proxy.ts:1076`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1076) | `function workspaceView(workspace: Workspace): WorkspaceView {` |
| `archivedSessionIds` | `let` | [`packages/host/apiproxy/src/api-proxy.ts:3544`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L3544) | `let archivedSessionIds = ctx.workspaceRegistry.archivedSessionIds` |

### Tests and executable evidence

- [`scripts/clean.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/clean.spec.ts) — A test under the owning area exercises or imports `delete`.
- [`scripts/test-fixture-cleanup.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-fixture-cleanup.ts) — A test under the owning area exercises or imports `delete`.
- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — A test under the owning area exercises or imports `delete`.
- [`apps/web/tests/assembled-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/assembled-boot.ts) — A test under the owning area exercises or imports `delete`.
- [`scripts/archived-agent-notes.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/archived-agent-notes.spec.ts) — A test under the owning area exercises or imports `delete`.
- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — A test under the owning area exercises or imports `delete`.
- [`apps/web/tests/models-settings.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/models-settings.e2e.ts) — A test under the owning area exercises or imports `delete`.
- [`apps/web/tests/scaffold-hermetic.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold-hermetic.e2e.ts) — A test under the owning area exercises or imports `delete`.

## How to read the implementation

1. Start with [`scripts/archived-agent-notes.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/archived-agent-notes.ts) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
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

- Tags: `class/feature`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/extensions`, `domain/filesystem`, `domain/session-state`, `domain/storage`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/projection`, `mechanism/registry`
- Aliases: `archivedSessionIds`, `sessionIds`, `archived`, `list`, `sessionVisible`, `deriveGroups`, `deriveFlat`, `workspaceView`, `archive`, `WorkspaceUnknownSessionError`, `workspaceDomainState.archivedSessionIds`, `tree.ts`, `archivedSessionIds: z.array(sessionId).default`, `ctx.workspaceRegistry.archiveSession`
- Regex: `(?i)(archivedSessionIds|sessionIds|archived|list|sessionVisible|deriveGroups|deriveFlat|workspaceView)`

```bash
rg -n --pcre2 "(?i)(archivedSessionIds|sessionIds|archived|list|sessionVisible|deriveGroups|deriveFlat|workspaceView)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0199. Workspace Registration Deletion](0199-workspace-registration-deletion.md): Shares source implementation: `packages/client/ui-primitives/src/Menu.tsx`, `packages/client/web-react/src/scoped-slots.tsx`.
- **`shares-code-with`** — [0303. Bind JSONL session identity before mutation](0303-bind-jsonl-session-identity-before-mutation.md): Shares source implementation: `packages/client/ui-primitives/src/Menu.tsx`, `packages/client/web-react/src/scoped-slots.tsx`.
- **`shares-code-with`** — [0183. Session List Browsing and Manual Workspace Order](0183-session-list-browsing-and-manual-workspace-order.md): Shares source implementation: `packages/client/ui-primitives/src/Menu.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0652. Prune dead methods from the persistence seam](0652-prune-dead-methods-from-the-persistence-seam.md): Shares source implementation: `packages/client/ui-primitives/src/Menu.tsx`, `packages/client/web-react/src/scoped-slots.tsx`.
- **`shares-code-with`** — [0297. Workspace Sidebar Order and Folding](0297-workspace-sidebar-order-and-folding.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/workspace/workspace/src/entity.ts`.
- **`shares-code-with`** — [0509. Domain KV storage capability seam and the workspace entity](0509-domain-kv-storage-capability-seam-and-the-workspace-entity.md): Shares source implementation: `packages/workspace/workspace/src/entity.ts`, `packages/workspace/workspace/src/index.ts`.
- **`shares-code-with`** — [0197. Web past-session search](0197-web-past-session-search.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/session-export.ts`.
- **`shares-code-with`** — [0345. Workspace New Session reuse hijacked cwd-matching unaccounted blank sessions](0345-workspace-new-session-reuse-hijacked-cwd-matching-unaccounted-blank-sess.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/workspace/workspace/src/entity.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0232-session-archive-registry-global-set.md`.
