---
id: "dsh-note-0183"
title: "Session List Browsing and Manual Workspace Order"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-25-session-list-browsing-and-manual-order.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/configuration"
  - "domain/filesystem"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "HoverCard"
  - "Menu"
  - "WorkspacePickFlow"
  - "WorkspacePicker"
  - "updatedAt"
  - "view"
  - "workspaces"
  - "workspace"
  - "parentId"
  - "WorkspaceMoveInvalidError"
  - "sessionIds"
  - "session/event"
  - "WorkspaceView.sessionIds"
  - "dsh.workspace.view"
search_regex: "(?i)(HoverCard|Menu|WorkspacePickFlow|WorkspacePicker|updatedAt|view|workspaces|workspace)"
---

# 0183. Session List Browsing and Manual Workspace Order — implementation context

## Open this when

Workspace UI Complete Product Flow shipped the first form of the grouped session list and explicitly scoped out operations such as Rename and drag ordering. The design file (figma 239-10458 and its companion screens) has since filled in those interactions: the list must switch to an ungrouped flat view, session rows need a hover detail card and an action menu, workspaces need renaming, and sessions need manual ordering inside their group. Two existing mechanisms stood in the way.

## Source decision

The group-by menu offers two modes, WorkSpace / In one list. WorkSpace mode renders peer session rows within each group in the manual order from WorkspaceView.sessionIds; In one list combines every session and sorts them strictly newest-first by updatedAt. Neither mode projects parentId into a list hierarchy; fork lineage remains session data only. Web session fork actions define the complete fork behavior. The mode choice persists in the browser (dsh.workspace.view) across reloads.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-25-session-list-browsing-and-manual-order.md](../02-notes/implemented/feature/2026-07-25-session-list-browsing-and-manual-order.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-25-session-list-browsing-and-manual-order.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-25-session-list-browsing-and-manual-order.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/web/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `apps/web`. | `named-directory-member` |
| [`apps/web`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/web) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `updatedAt`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts) | package entry point | Defines `view`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/client/src/client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/client.ts) | runtime implementation | Defines `parentId`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `workspaces`, a construct named by the note. Defines `workspace`, a construct named by the note. | `symbol-definition` |
| [`packages/workspace/workspace/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts) | package entry point | Defines `sessionIds`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/Menu.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Menu.tsx) | runtime implementation | Defines `Menu`, a construct named by the note. | `symbol-definition` |
| [`packages/workspace/workspace/src/entity.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/entity.ts) | runtime implementation | Defines `WorkspaceMoveInvalidError`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/HoverCard.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx) | runtime implementation | Defines `HoverCard`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-workspace/src/client/WorkspacePicker.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/WorkspacePicker.tsx) | runtime implementation | Defines `WorkspacePickFlow`, a construct named by the note. Defines `WorkspacePicker`, a construct named by the note. | `symbol-definition` |
| [`knip.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/knip.json) | composition and configuration | Contains the exact code literal `apps/web` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `HoverCard` | `function` | [`packages/client/ui-primitives/src/HoverCard.tsx:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx#L31) | `export function HoverCard({` |
| `Menu` | `function` | [`packages/client/ui-primitives/src/Menu.tsx:90`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Menu.tsx#L90) | `export function Menu({ open, anchor, items, selectedId, selectedIds, onSelect, onClose, align = 'start', side = 'bottom', portal = false, closeOnPointerLeave = false, dense = false, compact = false, getAnchorRect, footer` |
| `WorkspacePickFlow` | `function` | [`packages/client/ui-workspace/src/client/WorkspacePicker.tsx:58`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/WorkspacePicker.tsx#L58) | `export function WorkspacePickFlow({` |
| `WorkspacePicker` | `function` | [`packages/client/ui-workspace/src/client/WorkspacePicker.tsx:225`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/WorkspacePicker.tsx#L225) | `export function WorkspacePicker({` |
| `updatedAt` | `const` | [`packages/goal/goal/src/fold.ts:161`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L161) | `const updatedAt = nonNegativeInteger(value['updatedAt'], 'updatedAt')` |
| `view` | `const` | [`packages/goal/goal/src/index.ts:535`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L535) | `const view = this.view(cache)` |
| `workspaces` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1513`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1513) | `const workspaces = ctx.workspaceRegistry.list()` |
| `workspace` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1519`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1519) | `const workspace = workspaces.find(candidate => candidate.sessionIds.includes(ancestor.header.id))` |
| `parentId` | `const` | [`packages/sdk/client/src/client.ts:365`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/client.ts#L365) | `const parentId = params.parentSessionId` |
| `WorkspaceMoveInvalidError` | `class` | [`packages/workspace/workspace/src/entity.ts:19`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/entity.ts#L19) | `export class WorkspaceMoveInvalidError extends Error {` |
| `sessionIds` | `const` | [`packages/workspace/workspace/src/index.ts:454`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts#L454) | `const sessionIds = group.headers` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model`.
- [`packages/workspace/workspace/tests/workspace.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/tests/workspace.spec.ts) — A test under the owning area exercises or imports `WorkspaceMoveInvalidError`.
- [`packages/client/ui-workspace/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `WorkspacePicker`.
- [`packages/client/ui-primitives/tests/atoms.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/atoms.client.spec.tsx) — A test under the owning area exercises or imports `Menu`.
- [`packages/client/ui-primitives/tests/hover-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/hover-card.client.spec.tsx) — A test under the owning area exercises or imports `HoverCard`.
- [`packages/client/ui-workspace/tests/workspace-picker.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/tests/workspace-picker.client.spec.tsx) — A test under the owning area exercises or imports `WorkspacePicker`.
- [`scripts/release/families.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/release/families.spec.ts) — Contains the exact code literal `apps/web` named by the note.
- Source verification intent: Package-level suites cover the derivations (deriveGroups/deriveFlat), peer session rows, both apply registrations and passthroughs, host entity move semantics, and the rename/insertSessionBefore RPC implementations with their fixture stubs; the apps/web keyless snapshots regress the assembled application and pin that a fork does not introduce session expansion controls.

## How to read the implementation

1. Start with [`apps/web/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/package.json) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/configuration`, `domain/filesystem`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`
- Aliases: `HoverCard`, `Menu`, `WorkspacePickFlow`, `WorkspacePicker`, `updatedAt`, `view`, `workspaces`, `workspace`, `parentId`, `WorkspaceMoveInvalidError`, `sessionIds`, `session/event`, `WorkspaceView.sessionIds`, `dsh.workspace.view`
- Regex: `(?i)(HoverCard|Menu|WorkspacePickFlow|WorkspacePicker|updatedAt|view|workspaces|workspace)`

```bash
rg -n --pcre2 "(?i)(HoverCard|Menu|WorkspacePickFlow|WorkspacePicker|updatedAt|view|workspaces|workspace)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0332. Same-basename Workspace adoption](0332-same-basename-workspace-adoption.md): The source note links to this decision directly.
- **`source-link`** — [0185. Workspace UI Complete Product Flow](0185-workspace-ui-complete-product-flow.md): The source note links to this decision directly.
- **`source-link`** — [0196. Web session fork actions](0196-web-session-fork-actions.md): The source note links to this decision directly.
- **`source-link`** — [0297. Workspace Sidebar Order and Folding](0297-workspace-sidebar-order-and-folding.md): The source note links to this decision directly.
- **`shares-code-with`** — [0199. Workspace Registration Deletion](0199-workspace-registration-deletion.md): Shares source implementation: `packages/client/ui-primitives/src/Menu.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0232. Session archive (registry-global set)](0232-session-archive-registry-global-set.md): Shares source implementation: `packages/client/ui-primitives/src/Menu.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0472. One route to add a Workspace](0472-one-route-to-add-a-workspace.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0509. Domain KV storage capability seam and the workspace entity](0509-domain-kv-storage-capability-seam-and-the-workspace-entity.md): Shares source implementation: `packages/workspace/workspace/src/entity.ts`, `packages/workspace/workspace/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0183-session-list-browsing-and-manual-workspace-order.md`.
