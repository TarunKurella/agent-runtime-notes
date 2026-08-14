---
id: "dsh-note-0345"
title: "Workspace New Session reuse hijacked cwd-matching unaccounted blank sessions"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-05-workspace-blank-session-reuse-membership.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "domain/agent-loop"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
aliases:
  - "sessionIds"
  - "blank"
  - "cwd"
  - "defaults.cwd = process.cwd"
  - "connectWorkspace"
  - "main-session-*"
  - "session.create"
  - "defaults.cwd"
  - "turn/start"
  - "summary.cwd === workspace.path"
  - "workspace.sessionIds.includes"
  - "workspace.attachSession"
  - "attachSession"
  - "New Session"
search_regex: "(?i)(sessionIds|blank|defaults\\.cwd[- ]=[- ]process\\.cwd|connectWorkspace|main\\-session\\-\\*|session\\.create|defaults\\.cwd|turn/start)"
---

# 0345. Workspace New Session reuse hijacked cwd-matching unaccounted blank sessions — implementation context

## Open this when

Clicking the + on a Workspace group in the sidebar sometimes opened a session that the sidebar showed under Ungrouped instead of under the clicked Workspace --- "entered a new session but the Workspace was not selected". The failure was specific to Workspaces registered at the directory the CLI runs from (in practice the harness checkout itself, i.e. defaults.cwd = process.cwd()), and appeared once a CLI-born blank session existed there. Root cause: connectWorkspace's blank-session reuse scanned the session list mirror on cwd equality alone.

## Source decision

The reuse scan now requires workspace membership: blank AND summary.cwd === workspace.path AND workspace.sessionIds.includes(summary.id) AND not archived. A cwd-only match falls through to session.create({ workspaceId }), which attaches the fresh session so the Workspace owns it --- the same arm the flow already used for "no blank session exists".

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-05-workspace-blank-session-reuse-membership.md](../02-notes/implemented/bug-fix/2026-08-05-workspace-blank-session-reuse-membership.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-05-workspace-blank-session-reuse-membership.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-05-workspace-blank-session-reuse-membership.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/fs/tool-fs/src/session-cwd.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts) | runtime implementation | Defines `cwd`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `cwd`, a construct named by the note. Defines `blank`, a construct named by the note. | `symbol-definition` |
| [`packages/workspace/workspace/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts) | package entry point | Defines `sessionIds`, a construct named by the note. | `symbol-definition` |
| [`packages/workspace/workspace/src/entity.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/entity.ts) | runtime implementation | Defines `sessionIds`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs-search/src/search-core.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/search-core.ts) | runtime implementation | Defines `cwd`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `sessionIds`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx) | runtime implementation | Defines `blank`, a construct named by the note. | `symbol-definition` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `turn/start` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `turn/start` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `turn/start` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Contains the exact code literal `turn/start` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.zh.md) | package contract and examples | Contains the exact code literal `turn/start` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `sessionIds` | `const` | [`packages/client/connection/src/client/fixture.ts:2679`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L2679) | `const sessionIds = [...without.slice(0, at), sessionId, ...without.slice(at)]` |
| `blank` | `const` | [`packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx#L71) | `const blank = useSession(s => s.blank)` |
| `blank` | `const` | [`packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx:147`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx#L147) | `const blank = useSession(s => s.blank)` |
| `cwd` | `const` | [`packages/fs/tool-fs-search/src/search-core.ts:223`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/search-core.ts#L223) | `const cwd = exec.agent?.session.header.cwd` |
| `cwd` | `const` | [`packages/fs/tool-fs/src/session-cwd.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts#L24) | `const cwd = exec.agent?.session.header.cwd` |
| `cwd` | `const` | [`packages/fs/tool-fs/src/session-cwd.ts:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts#L41) | `const cwd = policyWorkspaceRoot ?? sessionCwd(exec, requestedPath)` |
| `blank` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:513`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L513) | `const blank = state.blank && event.type !== 'turn/start'` |
| `cwd` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2180`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2180) | `const cwd = workspace?.path ?? request.payload.cwd ?? defaults.cwd` |
| `cwd` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:3224`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L3224) | `const cwd = session.header.cwd` |
| `sessionIds` | `const` | [`packages/workspace/workspace/src/entity.ts:167`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/entity.ts#L167) | `const sessionIds = [...without.slice(0, at), sessionId, ...without.slice(at)]` |
| `sessionIds` | `const` | [`packages/workspace/workspace/src/entity.ts:207`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/entity.ts#L207) | `const sessionIds = changed.sessionIds.filter(` |
| `sessionIds` | `const` | [`packages/workspace/workspace/src/index.ts:454`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts#L454) | `const sessionIds = group.headers` |
| `sessionIds` | `const` | [`packages/workspace/workspace/src/index.ts:478`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts#L478) | `const sessionIds = [` |

### Tests and executable evidence

- [`packages/client/runtime/tests/workspaces-service.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/workspaces-service.client.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `connectWorkspace`.
- [`apps/web/tests/schedule-after.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/schedule-after.e2e.ts) — A test under the owning area exercises or imports `attachSession`.
- [`apps/web/tests/workspace-management.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/workspace-management.e2e.ts) — A test under the owning area exercises or imports `attachSession`.
- [`apps/web/tests/sidebar-subagent-activity.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/sidebar-subagent-activity.e2e.ts) — A test under the owning area exercises or imports `attachSession`.
- [`packages/host/apiproxy/tests/api-proxy-fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-fork.spec.ts) — A test under the owning area exercises or imports `attachSession`.
- [`packages/workspace/workspace/tests/workspace.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/tests/workspace.spec.ts) — A test under the owning area exercises or imports `attachSession`.
- [`packages/host/apiproxy/tests/api-proxy-workspace.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-workspace.spec.ts) — A test under the owning area exercises or imports `attachSession`.
- [`packages/client/ui-conversation/tests/apply-inject.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/apply-inject.client.spec.tsx) — A test under the owning area exercises or imports `connectWorkspace`.
- Source verification intent: packages/client/runtime/tests/workspaces-service.client.spec.ts covers the four outcomes: a member blank session is reused (no create RPC); a stray blank with matching cwd is not reused and a fresh accounted session is created (regression case); an archived blank is not reused; a rejected first prompt keeps a member blank eligible. The full client suite (pnpm run test:gui) stays green.

## How to read the implementation

1. Start with [`packages/fs/tool-fs/src/session-cwd.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `domain/agent-loop`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`
- Aliases: `sessionIds`, `blank`, `cwd`, `defaults.cwd = process.cwd`, `connectWorkspace`, `main-session-*`, `session.create`, `defaults.cwd`, `turn/start`, `summary.cwd === workspace.path`, `workspace.sessionIds.includes`, `workspace.attachSession`, `attachSession`, `New Session`
- Regex: `(?i)(sessionIds|blank|defaults\.cwd[- ]=[- ]process\.cwd|connectWorkspace|main\-session\-\*|session\.create|defaults\.cwd|turn/start)`

```bash
rg -n --pcre2 "(?i)(sessionIds|blank|defaults\\.cwd[- ]=[- ]process\\.cwd|connectWorkspace|main\\-session\\-\\*|session\\.create|defaults\\.cwd|turn/start)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0185. Workspace UI Complete Product Flow](0185-workspace-ui-complete-product-flow.md): The source note links to this decision directly.
- **`shares-code-with`** — [0297. Workspace Sidebar Order and Folding](0297-workspace-sidebar-order-and-folding.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0319. Web details follow the current Session lifecycle](0319-web-details-follow-the-current-session-lifecycle.md): Shares source implementation: `apps/web/tests/schedule-after.e2e.ts`, `apps/web/tests/workspace-management.e2e.ts`.
- **`shares-code-with`** — [0199. Workspace Registration Deletion](0199-workspace-registration-deletion.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/workspace/workspace/src/entity.ts`.
- **`shares-code-with`** — [0183. Session List Browsing and Manual Workspace Order](0183-session-list-browsing-and-manual-workspace-order.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/workspace/workspace/src/entity.ts`.
- **`shares-code-with`** — [0622. Web conversation UI polish sweep](0622-web-conversation-ui-polish-sweep.md): Shares source implementation: `packages/fs/tool-fs-search/src/search-core.ts`, `packages/fs/tool-fs/src/session-cwd.ts`.
- **`shares-code-with`** — [0509. Domain KV storage capability seam and the workspace entity](0509-domain-kv-storage-capability-seam-and-the-workspace-entity.md): Shares source implementation: `packages/workspace/workspace/src/entity.ts`, `packages/workspace/workspace/src/index.ts`.
- **`shares-code-with`** — [0130. Multiplex concurrent ACP sessions over one connection](0130-multiplex-concurrent-acp-sessions-over-one-connection.md): Shares source implementation: `packages/fs/tool-fs/src/session-cwd.ts`, `packages/host/apiproxy/src/api-proxy.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0345-workspace-new-session-reuse-hijacked-cwd-matching-unaccounted-blank-sess.md`.
