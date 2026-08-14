---
id: "dsh-note-0472"
title: "One route to add a Workspace"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-07-31-one-route-to-add-a-workspace.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/filesystem"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/registry"
aliases:
  - "WorkspaceCreateInput"
  - "intentName"
  - "pending"
  - "WorkspacePickFlow"
  - "flowBusy"
  - "addIsTheOnlyEntry"
  - "phase"
  - "workspace"
  - "WorkspaceApi"
  - "workspaceRoot"
  - "<workspaceRoot>/<name>"
  - "menu.addWorkspace"
  - "addWorkspace"
  - "create.*"
search_regex: "(?i)(WorkspaceCreateInput|intentName|pending|WorkspacePickFlow|flowBusy|addIsTheOnlyEntry|phase|workspace)"
---

# 0472. One route to add a Workspace — implementation context

## Open this when

Both Workspace surfaces --- the sidebar region header's + and the conversation hero's chip --- offered two ways to get a Workspace: Open local folder…, which raised the composed directory flow, and Create a new workspace, which took a name and created /. The two overlapped: the browse occupant carries its own New folder affordance, so picking a directory already covered creating one. Two entries meant two vocabularies for one outcome, a name dialog with its own duplicate-name rule, and a create target the operator could neither see nor choose.

## Source decision

Adding a Workspace has one route: pick a host directory through the composed directory flow, new or existing. menu.addWorkspace ("添加工作区…" / "Add workspace…") is the entry; the create-by-name dialog and its create. / menu.createWorkspace / workspace.new strings are gone. The label names the outcome, not the mechanism, because it is now the only door to that outcome --- a user looking for "新建" must find it. A menu exists to disambiguate between targets.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-07-31-one-route-to-add-a-workspace.md](../02-notes/implemented/simplification/2026-07-31-one-route-to-add-a-workspace.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-07-31-one-route-to-add-a-workspace.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-07-31-one-route-to-add-a-workspace.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-workspace/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-workspace`. | `named-package-member` |
| [`packages/client/ui-workspace/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-workspace`. | `named-package-member` |
| [`packages/client/ui-workspace/src/client/WorkspacePicker.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/WorkspacePicker.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-workspace`. Defines `addIsTheOnlyEntry`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-workspace/src/client/WorkspaceBrowser.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/WorkspaceBrowser.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-workspace`. Defines `pending`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-workspace`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `phase`, a construct named by the note. | `symbol-definition` |
| [`packages/lsp/tool-lsp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/tool-lsp/src/index.ts) | package entry point | Defines `workspaceRoot`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `workspace`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api/workspace.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/workspace.ts) | runtime implementation | Defines `WorkspaceApi`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/workspaces/workspace.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/workspaces/workspace.ts) | runtime implementation | Defines `WorkspaceCreateInput`, a construct named by the note. Defines `intentName`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-workspace/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/README.md) | package contract and examples | Core file in the package named by the note: `packages/client/ui-workspace`. | `named-package-member` |
| [`packages/client/ui-workspace/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/package.json) | composition and configuration | Core file in the package named by the note: `packages/client/ui-workspace`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `WorkspaceCreateInput` | `type` | [`packages/client/runtime/src/client/workspaces/workspace.ts:11`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/workspaces/workspace.ts#L11) | `export type WorkspaceCreateInput = { path: string }` |
| `intentName` | `function` | [`packages/client/runtime/src/client/workspaces/workspace.ts:139`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/workspaces/workspace.ts#L139) | `function intentName(input: WorkspaceCreateInput): string {` |
| `pending` | `const` | [`packages/client/ui-workspace/src/client/WorkspaceBrowser.tsx:697`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/WorkspaceBrowser.tsx#L697) | `const pending = currentRemote.status === 'loading'` |
| `WorkspacePickFlow` | `function` | [`packages/client/ui-workspace/src/client/WorkspacePicker.tsx:58`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/WorkspacePicker.tsx#L58) | `export function WorkspacePickFlow({` |
| `flowBusy` | `const` | [`packages/client/ui-workspace/src/client/WorkspacePicker.tsx:86`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/WorkspacePicker.tsx#L86) | `const flowBusy = flowOpen \|\| pickingFolder` |
| `addIsTheOnlyEntry` | `const` | [`packages/client/ui-workspace/src/client/WorkspacePicker.tsx:152`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/WorkspacePicker.tsx#L152) | `const addIsTheOnlyEntry = !pinAdd && listSettled && addEntries.length === 1` |
| `phase` | `const` | [`packages/goal/goal/src/fold.ts:100`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L100) | `const phase = value['phase'] as GoalPhase` |
| `workspace` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1519`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1519) | `const workspace = workspaces.find(candidate => candidate.sessionIds.includes(ancestor.header.id))` |
| `WorkspaceApi` | `interface` | [`packages/host/apiproxy/src/api/workspace.ts:39`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/workspace.ts#L39) | `export interface WorkspaceApi {` |
| `workspaceRoot` | `const` | [`packages/lsp/tool-lsp/src/index.ts:182`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/tool-lsp/src/index.ts#L182) | `const workspaceRoot = sessionCwd(exec)` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model`.
- [`packages/lsp/tool-lsp/tests/tool-lsp.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/tool-lsp/tests/tool-lsp.spec.ts) — A test under the owning area exercises or imports `workspaceRoot`.
- [`packages/client/ui-workspace/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `createWorkspace`. A test under the owning area exercises or imports `ui-workspace`.
- [`packages/client/ui-workspace/tests/workspace-picker.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/tests/workspace-picker.client.spec.tsx) — A test under the owning area exercises or imports `createWorkspace`.
- [`packages/client/ui-workspace/tests/workspace-browser.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/tests/workspace-browser.client.spec.tsx) — A test under the owning area exercises or imports `createWorkspace`.
- Source verification intent: connectFreshWorkspace --- the helper every web e2e scenario boots through --- stages /workspace and adopts it through the dialog's path editor, so the produced session cwd stays identical to what create-by-name produced and scenario goldens stay valid. Staging rather than creating in-dialog keeps the helper idempotent across the repeated connects a scenario may make (a second create of the same folder fails, and the create dialog holds the flow open on that failure).

## How to read the implementation

1. Start with [`packages/client/ui-workspace/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/filesystem`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/registry`
- Aliases: `WorkspaceCreateInput`, `intentName`, `pending`, `WorkspacePickFlow`, `flowBusy`, `addIsTheOnlyEntry`, `phase`, `workspace`, `WorkspaceApi`, `workspaceRoot`, `<workspaceRoot>/<name>`, `menu.addWorkspace`, `addWorkspace`, `create.*`
- Regex: `(?i)(WorkspaceCreateInput|intentName|pending|WorkspacePickFlow|flowBusy|addIsTheOnlyEntry|phase|workspace)`

```bash
rg -n --pcre2 "(?i)(WorkspaceCreateInput|intentName|pending|WorkspacePickFlow|flowBusy|addIsTheOnlyEntry|phase|workspace)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0183. Session List Browsing and Manual Workspace Order](0183-session-list-browsing-and-manual-workspace-order.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0185. Workspace UI Complete Product Flow](0185-workspace-ui-complete-product-flow.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0312. The scrollbar tokens get their consumer, and the workspace list reserves its gutter](0312-the-scrollbar-tokens-get-their-consumer-and-the-workspace-list-reserves.md): Shares source implementation: `packages/client/ui-workspace`, `packages/client/ui-workspace/src/index.ts`.
- **`shares-code-with`** — [0629. Collapsed sidebar upper controls share one entry motion](0629-collapsed-sidebar-upper-controls-share-one-entry-motion.md): Shares source implementation: `packages/client/ui-workspace`, `packages/client/ui-workspace/src/index.ts`.
- **`shares-code-with`** — [0275. The no-Workspace composer opens the existing picker](0275-the-no-workspace-composer-opens-the-existing-picker.md): Shares source implementation: `packages/client/ui-workspace/src/client/WorkspacePicker.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0360. The session-row identity guard covers the preset](0360-the-session-row-identity-guard-covers-the-preset.md): Shares source implementation: `packages/client/ui-workspace/src/client/WorkspaceBrowser.tsx`, `packages/goal/goal/src/fold.ts`.
- **`shares-code-with`** — [0070. A capability-discriminated directory-picker seam for the web-GUI host](0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0513. Record last activity in the session index](0513-record-last-activity-in-the-session-index.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/host/apiproxy/src/api-proxy.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0472-one-route-to-add-a-workspace.md`.
