---
id: "dsh-note-0297"
title: "Workspace Sidebar Order and Folding"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-11-workspace-sidebar-order-and-folding.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "sessionIds"
  - "workspaceIds"
  - "WorkspaceView.sessionIds"
  - "insertBefore"
  - "workspace.insertBefore"
  - "host/workspace-order-changed"
  - "workspace-not-found"
  - "Workspace.sessionIds"
  - "Workspace Sidebar Order and Folding"
  - "feature"
  - "boundary"
  - "discovery routing"
  - "evidence"
  - "recovery"
search_regex: "(?i)(sessionIds|workspaceIds|WorkspaceView\\.sessionIds|insertBefore|workspace\\.insertBefore|host/workspace\\-order\\-changed|workspace\\-not\\-found|Workspace\\.sessionIds)"
---

# 0297. Workspace Sidebar Order and Folding — implementation context

## Open this when

A Workspace with many Sessions can consume the entire sidebar and push other Workspaces out of reach. A compact list needs a bounded default while preserving an explicit route to every Session. The sidebar also needs an activity-oriented order, but WorkspaceView.sessionIds is the durable manual account and must not be rewritten by Session activity. Workspace groups themselves had no user-controlled durable order. Browser-native drag additionally rejects a drop released outside the list and animates the row back even when the application still has a valid insertion marker.

## Source decision

The Workspace registry owns a durable workspaceIds order and exposes insertBefore(id, beforeId?) with DOM insertBefore semantics. The Host RPC workspace.insertBefore returns the complete committed order, and a pure order mutation emits host/workspace-order-changed with the same complete order. Unknown source or anchor ids reject as workspace-not-found; self-anchored and already-positioned moves do not write. The client installs a Workspace drag optimistically.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-11-workspace-sidebar-order-and-folding.md](../02-notes/implemented/feature/2026-08-11-workspace-sidebar-order-and-folding.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-11-workspace-sidebar-order-and-folding.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-11-workspace-sidebar-order-and-folding.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `workspaceIds`, a construct named by the note. Contains the exact code literal `host/workspace-order-changed` named by the note. | `exact-code-occurrence, symbol-definition` |
| [`packages/workspace/workspace/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts) | package entry point | Defines `sessionIds`, a construct named by the note. Defines `workspaceIds`, a construct named by the note. | `symbol-definition` |
| [`packages/workspace/workspace/src/entity.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/entity.ts) | runtime implementation | Defines `sessionIds`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `sessionIds`, a construct named by the note. Contains the exact code literal `host/workspace-order-changed` named by the note. | `exact-code-occurrence, symbol-definition` |
| [`packages/host/apiproxy/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/README.md) | package contract and examples | Contains the exact code literal `host/workspace-order-changed` named by the note. | `exact-code-occurrence` |
| [`packages/host/apiproxy/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/README.zh.md) | package contract and examples | Contains the exact code literal `host/workspace-order-changed` named by the note. | `exact-code-occurrence` |
| [`packages/host/apiproxy/src/api/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/events.ts) | runtime implementation | Contains the exact code literal `host/workspace-order-changed` named by the note. | `exact-code-occurrence` |
| [`packages/client/runtime/src/client/workspaces/manager.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/workspaces/manager.ts) | runtime implementation | Contains the exact code literal `host/workspace-order-changed` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `sessionIds` | `const` | [`packages/client/connection/src/client/fixture.ts:2679`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L2679) | `const sessionIds = [...without.slice(0, at), sessionId, ...without.slice(at)]` |
| `workspaceIds` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2871`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2871) | `const workspaceIds = await ctx.workspaceRegistry.insertBefore(` |
| `sessionIds` | `const` | [`packages/workspace/workspace/src/entity.ts:167`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/entity.ts#L167) | `const sessionIds = [...without.slice(0, at), sessionId, ...without.slice(at)]` |
| `sessionIds` | `const` | [`packages/workspace/workspace/src/entity.ts:207`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/entity.ts#L207) | `const sessionIds = changed.sessionIds.filter(` |
| `workspaceIds` | `const` | [`packages/workspace/workspace/src/index.ts:220`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts#L220) | `const workspaceIds = [...without.slice(0, at), id, ...without.slice(at)]` |
| `sessionIds` | `const` | [`packages/workspace/workspace/src/index.ts:454`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts#L454) | `const sessionIds = group.headers` |
| `sessionIds` | `const` | [`packages/workspace/workspace/src/index.ts:478`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts#L478) | `const sessionIds = [` |
| `workspaceIds` | `const` | [`packages/workspace/workspace/src/index.ts:493`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts#L493) | `const workspaceIds = [...table.entries()]` |

### Tests and executable evidence

- [`scripts/gen-doc-graphs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.spec.ts) — A test under the owning area exercises or imports `drop`.
- [`scripts/translation-brief.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-brief.spec.ts) — A test under the owning area exercises or imports `drop`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `drop`.
- [`apps/web/tests/cordis-tool-round.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/cordis-tool-round.e2e.ts) — A test under the owning area exercises or imports `drop`.
- [`apps/web/tests/image-display.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/image-display.snapshot.ts) — A test under the owning area exercises or imports `drop`.
- [`apps/web/tests/composer-tab-geometry.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/composer-tab-geometry.e2e.ts) — A test under the owning area exercises or imports `drop`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `drop`.
- [`packages/core/agent-loop/tests/cancel.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/cancel.spec.ts) — A test under the owning area exercises or imports `drop`.
- Source verification intent: Domain and Host tests cover durable Workspace moves, no-op and invalid anchors, restart recovery, full-order RPC responses, order frames, and one Workspace snapshot per Host-stream baseline. Runtime tests cover optimistic order, frame/response precedence, overlapping rejection rollback to Host-confirmed order, reconnect baselines, and New Session target priority.

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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `sessionIds`, `workspaceIds`, `WorkspaceView.sessionIds`, `insertBefore`, `workspace.insertBefore`, `host/workspace-order-changed`, `workspace-not-found`, `Workspace.sessionIds`, `Workspace Sidebar Order and Folding`, `feature`, `boundary`, `discovery routing`, `evidence`, `recovery`
- Regex: `(?i)(sessionIds|workspaceIds|WorkspaceView\.sessionIds|insertBefore|workspace\.insertBefore|host/workspace\-order\-changed|workspace\-not\-found|Workspace\.sessionIds)`

```bash
rg -n --pcre2 "(?i)(sessionIds|workspaceIds|WorkspaceView\\.sessionIds|insertBefore|workspace\\.insertBefore|host/workspace\\-order\\-changed|workspace\\-not\\-found|Workspace\\.sessionIds)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0183. Session List Browsing and Manual Workspace Order](0183-session-list-browsing-and-manual-workspace-order.md): The source note links to this decision directly.
- **`shares-code-with`** — [0345. Workspace New Session reuse hijacked cwd-matching unaccounted blank sessions](0345-workspace-new-session-reuse-hijacked-cwd-matching-unaccounted-blank-sess.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0199. Workspace Registration Deletion](0199-workspace-registration-deletion.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/workspace/workspace/src/entity.ts`.
- **`shares-code-with`** — [0194. Trajectory inspection ledger](0194-trajectory-inspection-ledger.md): Shares source implementation: `apps/web/tests/composer-tab-geometry.e2e.ts`, `packages/client/connection/src/client/fixture.ts`.
- **`shares-code-with`** — [0232. Session archive (registry-global set)](0232-session-archive-registry-global-set.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/workspace/workspace/src/entity.ts`.
- **`shares-code-with`** — [0509. Domain KV storage capability seam and the workspace entity](0509-domain-kv-storage-capability-seam-and-the-workspace-entity.md): Shares source implementation: `packages/workspace/workspace/src/entity.ts`, `packages/workspace/workspace/src/index.ts`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0337. Todo-first composer context order](0337-todo-first-composer-context-order.md): Shares source implementation: `packages/workspace/workspace/src/index.ts`, `scripts/translation-brief.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0297-workspace-sidebar-order-and-folding.md`.
