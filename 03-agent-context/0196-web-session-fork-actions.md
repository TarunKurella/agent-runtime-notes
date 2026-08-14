---
id: "dsh-note-0196"
title: "Web session fork actions"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-27-web-session-fork-actions.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
aliases:
  - "ctx"
  - "sessionId"
  - "parentId"
  - "seq"
  - "sessionIds"
  - "updatedAt"
  - "sessions.fork"
  - "{ sessionId, increaseTitle: true }"
  - "{ sessionId, atSeq: node.seq, increaseTitle: true }"
  - "increaseTitle"
  - "（N）"
  - "atSeq"
  - "forkAt"
  - "WorkspaceView.sessionIds"
search_regex: "(?i)(sessionId|parentId|sessionIds|updatedAt|sessions\\.fork|\\{[- ]sessionId,[- ]increaseTitle:[- ]true[- ]\\}|\\{[- ]sessionId,[- ]atSeq:[- ]node\\.seq,[- ]increaseTitle:[- ]true[- ]\\}|increaseTitle)"
---

# 0196. Web session fork actions — implementation context

## Open this when

The Session store already provides a fork primitive that creates a child session from a completed-turn prefix, but the Web client has no unified interaction contract. The Session-row menu can express only "branch from the latest completed turn," while message IconActions need to express "branch from the turn containing this message"; if the two entry points independently interpret the boundary, switching, and failure behavior, the same user action acquires two sets of semantics.

## Source decision

The message-eligibility portion of this decision is narrowed by the completed-turn-tail decision; the shared runtime action, injection ownership, title handling, and peer-list decisions remain current. The Web Session-row menu and message IconActions share the client runtime's sessions.fork action. A Session row passes { sessionId, increaseTitle: true }, so it forks at the source session's last completed turn; an eligible completed-turn-tail message passes { sessionId, atSeq: node.seq, increaseTitle: true }, so it forks at the turn ending at that message.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-27-web-session-fork-actions.md](../02-notes/implemented/feature/2026-07-27-web-session-fork-actions.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-27-web-session-fork-actions.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-27-web-session-fork-actions.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/cli/src/profile-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts) | runtime implementation | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `sessionId`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `updatedAt`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts) | package entry point | Defines `updatedAt`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`packages/client/web/src/boot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx) | runtime implementation | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/client/src/client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/client.ts) | runtime implementation | Defines `parentId`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Defines `seq`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/repair.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts) | runtime implementation | Defines `seq`, a construct named by the note. | `symbol-definition` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `sessionId`, a construct named by the note. | `symbol-definition` |
| [`packages/terminal/terminal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/index.ts) | package entry point | Defines `sessionId`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ctx` | `const` | [`apps/cli/src/profile-boot.ts:248`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L248) | `const ctx = await boot(NAME, rootConfig, structuredClone(allPatches(composed)), (hostCtx) => {` |
| `sessionId` | `const` | [`packages/acp/acp/src/index.ts:254`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L254) | `const sessionId = SessionId(randomUUID())` |
| `parentId` | `const` | [`packages/api/remotes/src/agent-lookup.ts:68`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/src/agent-lookup.ts#L68) | `const parentId = session.header.parentSession` |
| `ctx` | `const` | [`packages/boot/app-boot/src/index.ts:764`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L764) | `const ctx = new Context()` |
| `seq` | `const` | [`packages/client/connection/src/client/fixture.ts:362`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L362) | `const seq = events.length` |
| `sessionIds` | `const` | [`packages/client/connection/src/client/fixture.ts:2679`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L2679) | `const sessionIds = [...without.slice(0, at), sessionId, ...without.slice(at)]` |
| `updatedAt` | `const` | [`packages/client/ui-workspace/src/client/WorkspaceBrowser.tsx:133`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/WorkspaceBrowser.tsx#L133) | `const updatedAt: Record<string, number> = {}` |
| `ctx` | `const` | [`packages/client/web/src/boot.tsx:162`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L162) | `const ctx = this.ctx` |
| `ctx` | `const` | [`packages/client/web/src/boot.tsx:217`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L217) | `const ctx = this.ctx` |
| `seq` | `const` | [`packages/core/session/src/index.ts:233`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L233) | `const seq = event['seq']` |
| `seq` | `let` | [`packages/core/session/src/repair.ts:85`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts#L85) | `let seq = last.seq + 1` |
| `ctx` | `const` | [`packages/core/tools/src/index.ts:947`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L947) | `const ctx = this.ctx` |
| `sessionId` | `const` | [`packages/e2b/subprocess-e2b/src/terminal.ts:517`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/terminal.ts#L517) | `const sessionId = await terminalSessionId(sandbox, handle.pid, controlEnvs, spec.signal)` |
| `updatedAt` | `const` | [`packages/goal/goal/src/fold.ts:161`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L161) | `const updatedAt = nonNegativeInteger(value['updatedAt'], 'updatedAt')` |
| `updatedAt` | `const` | [`packages/goal/goal/src/index.ts:508`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L508) | `const updatedAt = cache.state.updatedAt` |
| `updatedAt` | `const` | [`packages/goal/goal/src/index.ts:564`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L564) | `const updatedAt = cache.state.updatedAt` |

### Tests and executable evidence

- [`apps/web/tests/message-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/message-actions.e2e.ts) — The source note names this file directly. A test under the owning area exercises or imports `fork`.
- [`apps/cli/tests/headless-shutdown.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/headless-shutdown.e2e.ts) — A test under the owning area exercises or imports `fork`.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `fork`.
- [`apps/web/tests/subagent-conversation.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/subagent-conversation.e2e.ts) — A test under the owning area exercises or imports `fork`.
- [`apps/web/tests/chat-long-interactions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/chat-long-interactions.e2e.ts) — A test under the owning area exercises or imports `fork`.
- [`packages/host/apiproxy/tests/api-proxy-fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-fork.spec.ts) — A test under the owning area exercises or imports `atSeq`.
- [`packages/host/apiproxy/tests/client-handler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/client-handler.spec.ts) — A test under the owning area exercises or imports `atSeq`.
- [`packages/client/ui-workspace/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `increaseTitle`.

## How to read the implementation

1. Start with [`apps/cli/src/profile-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`
- Aliases: `ctx`, `sessionId`, `parentId`, `seq`, `sessionIds`, `updatedAt`, `sessions.fork`, `{ sessionId, increaseTitle: true }`, `{ sessionId, atSeq: node.seq, increaseTitle: true }`, `increaseTitle`, `（N）`, `atSeq`, `forkAt`, `WorkspaceView.sessionIds`
- Regex: `(?i)(sessionId|parentId|sessionIds|updatedAt|sessions\.fork|\{[- ]sessionId,[- ]increaseTitle:[- ]true[- ]\}|\{[- ]sessionId,[- ]atSeq:[- ]node\.seq,[- ]increaseTitle:[- ]true[- ]\}|increaseTitle)`

```bash
rg -n --pcre2 "(?i)(sessionId|parentId|sessionIds|updatedAt|sessions\\.fork|\\{[- ]sessionId,[- ]increaseTitle:[- ]true[- ]\\}|\\{[- ]sessionId,[- ]atSeq:[- ]node\\.seq,[- ]increaseTitle:[- ]true[- ]\\}|increaseTitle)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0336. Message fork actions require a completed turn tail](0336-message-fork-actions-require-a-completed-turn-tail.md): The source note links to this decision directly.
- **`shares-code-with`** — [0308. Recursive Python SDK session notifications](0308-recursive-python-sdk-session-notifications.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0659. Remove the `agent/steering` mirror emit](0659-remove-the-agent-steering-mirror-emit.md): Shares source implementation: `apps/cli/src/profile-boot.ts`, `packages/client/web/src/boot.tsx`.
- **`shares-code-with`** — [0141. SessionStore fork API](0141-sessionstore-fork-api.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/repair.ts`.
- **`shares-code-with`** — [0554. dsh-tui chat channel module split](0554-dsh-tui-chat-channel-module-split.md): Shares source implementation: `apps/cli/src/profile-boot.ts`, `packages/client/web/src/boot.tsx`.
- **`shares-code-with`** — [0189. Code Mode live dispatch lifecycle and native-contract parallelism](0189-code-mode-live-dispatch-lifecycle-and-native-contract-parallelism.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/repair.ts`.
- **`shares-code-with`** — [0183. Session List Browsing and Manual Workspace Order](0183-session-list-browsing-and-manual-workspace-order.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/goal/goal/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0196-web-session-fork-actions.md`.
