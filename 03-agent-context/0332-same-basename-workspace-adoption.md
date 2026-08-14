---
id: "dsh-note-0332"
title: "Same-basename Workspace adoption"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-31-same-basename-workspace-adoption.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/schema-types"
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
aliases:
  - "basename"
  - "WorkspaceId"
  - "/a/xx"
  - "/b/xx"
  - "ctx.workspaceRegistry.create"
  - "workspace.create"
  - "workspace.rename"
  - "Same-basename Workspace adoption"
  - "bug fix"
  - "boundary"
  - "compatibility"
  - "discovery routing"
  - "evidence"
  - "ownership"
search_regex: "(?i)(basename|WorkspaceId|/a/xx|/b/xx|ctx\\.workspaceRegistry\\.create|workspace\\.create|workspace\\.rename|Same\\-basename[- ]Workspace[- ]adoption)"
---

# 0332. Same-basename Workspace adoption — implementation context

## Open this when

A Workspace is identified by its stable id and canonical directory path, while its title is mutable display metadata. The registry nevertheless rejected a new canonical path when its basename-derived title matched another Workspace. Common directory layouts such as /a/xx and /b/xx therefore could not coexist in the Web UI, even though the domain design already permits duplicate titles and every client operation addresses a Workspace by id.

## Source decision

ctx.workspaceRegistry.create(path, title?) treats canonical path as the only uniqueness key. Repeating the same path remains idempotent and preserves the registered title. Different canonical paths create different Workspace records and may share a title; when no title is supplied, each record still derives its title from basename(path) without suffixing or rewriting it. The Host's workspace.create({ path }) adoption route inherits that rule.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-31-same-basename-workspace-adoption.md](../02-notes/implemented/bug-fix/2026-07-31-same-basename-workspace-adoption.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-31-same-basename-workspace-adoption.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-31-same-basename-workspace-adoption.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/workspace/workspace/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/types.ts) | public types and contract | Defines `WorkspaceId`, a construct named by the note. | `symbol-definition` |
| [`packages/workspace/workspace/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts) | package entry point | Defines `WorkspaceId`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api/workspace.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/workspace.ts) | runtime implementation | Defines `WorkspaceId`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/acp-snapshot/src/normalize.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/normalize.ts) | runtime implementation | Defines `basename`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-deliverables/src/client/turn-deliverables.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-deliverables/src/client/turn-deliverables.ts) | runtime implementation | Defines `basename`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `basename` | `function` | [`packages/client/ui-deliverables/src/client/turn-deliverables.ts:146`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-deliverables/src/client/turn-deliverables.ts#L146) | `export function basename(path: string): string {` |
| `WorkspaceId` | `type` | [`packages/host/apiproxy/src/api/workspace.ts:18`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/workspace.ts#L18) | `export type WorkspaceId = Branded<'WorkspaceId'>` |
| `basename` | `const` | [`packages/test-support/acp-snapshot/src/normalize.ts:235`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/normalize.ts#L235) | `const basename = cwd.split(/[\\/]/).at(-1)` |
| `WorkspaceId` | `type` | [`packages/workspace/workspace/src/index.ts:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts#L30) | `export type WorkspaceId = WorkspaceIdBrand` |
| `WorkspaceId` | `function` | [`packages/workspace/workspace/src/index.ts:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts#L37) | `export function WorkspaceId(id: string): WorkspaceId {` |
| `WorkspaceId` | `type` | [`packages/workspace/workspace/src/types.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/types.ts#L15) | `export type WorkspaceId = Branded<'WorkspaceId'>` |

### Tests and executable evidence

- [`apps/web/tests/models-settings.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/models-settings.e2e.ts) — A test under the owning area exercises or imports `rename`.
- [`apps/web/tests/message-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/message-actions.e2e.ts) — A test under the owning area exercises or imports `rename`.
- [`apps/web/tests/pwsh-terminal.overlay.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/pwsh-terminal.overlay.yml) — A test under the owning area exercises or imports `rename`.
- [`apps/web/tests/workspace-management.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/workspace-management.e2e.ts) — A test under the owning area exercises or imports `rename`.
- [`packages/lsp/tool-lsp/tests/render.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/tool-lsp/tests/render.spec.ts) — A test under the owning area exercises or imports `rename`.
- [`packages/lsp/tool-lsp/tests/tool-lsp.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/tool-lsp/tests/tool-lsp.spec.ts) — A test under the owning area exercises or imports `rename`.
- [`packages/e2b/fs-e2b/tests/filesystem.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/tests/filesystem.spec.ts) — A test under the owning area exercises or imports `rename`.
- [`packages/typert/registry/tests/typert.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/tests/typert.spec.ts) — A test under the owning area exercises or imports `rename`.
- Source verification intent: Workspace registry and Host API tests create two real directories under different parents with the same final segment and assert distinct ids, paths, and durable order. The picker component renders equal labels as separate id-keyed entries. The keyless Web browser scenario adopts both directories through the composed directory flow and observes two registered and rendered Workspaces.

## How to read the implementation

1. Start with [`packages/workspace/workspace/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/types.ts) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/schema-types`, `domain/filesystem`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `basename`, `WorkspaceId`, `/a/xx`, `/b/xx`, `ctx.workspaceRegistry.create`, `workspace.create`, `workspace.rename`, `Same-basename Workspace adoption`, `bug fix`, `boundary`, `compatibility`, `discovery routing`, `evidence`, `ownership`
- Regex: `(?i)(basename|WorkspaceId|/a/xx|/b/xx|ctx\.workspaceRegistry\.create|workspace\.create|workspace\.rename|Same\-basename[- ]Workspace[- ]adoption)`

```bash
rg -n --pcre2 "(?i)(basename|WorkspaceId|/a/xx|/b/xx|ctx\\.workspaceRegistry\\.create|workspace\\.create|workspace\\.rename|Same\\-basename[- ]Workspace[- ]adoption)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0183. Session List Browsing and Manual Workspace Order](0183-session-list-browsing-and-manual-workspace-order.md): The source note links to this decision directly.
- **`source-link`** — [0185. Workspace UI Complete Product Flow](0185-workspace-ui-complete-product-flow.md): The source note links to this decision directly.
- **`source-link`** — [0191. Native workspace directory picker](0191-native-workspace-directory-picker.md): The source note links to this decision directly.
- **`source-link`** — [0509. Domain KV storage capability seam and the workspace entity](0509-domain-kv-storage-capability-seam-and-the-workspace-entity.md): The source note links to this decision directly.
- **`shares-code-with`** — [0199. Workspace Registration Deletion](0199-workspace-registration-deletion.md): Shares source implementation: `packages/workspace/workspace/src/index.ts`.
- **`shares-code-with`** — [0345. Workspace New Session reuse hijacked cwd-matching unaccounted blank sessions](0345-workspace-new-session-reuse-hijacked-cwd-matching-unaccounted-blank-sess.md): Shares source implementation: `apps/web/tests/workspace-management.e2e.ts`, `packages/workspace/workspace/src/index.ts`.
- **`shares-code-with`** — [0472. One route to add a Workspace](0472-one-route-to-add-a-workspace.md): Shares source implementation: `packages/host/apiproxy/src/api/workspace.ts`.
- **`shares-code-with`** — [0232. Session archive (registry-global set)](0232-session-archive-registry-global-set.md): Shares source implementation: `packages/workspace/workspace/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0332-same-basename-workspace-adoption.md`.
