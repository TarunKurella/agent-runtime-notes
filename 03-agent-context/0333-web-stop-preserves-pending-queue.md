---
id: "dsh-note-0333"
title: "Web stop preserves pending Queue"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-31-web-stop-preserves-queue.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "cancel"
  - "session.cancel"
  - "agent.cancel"
  - "InboxItemId"
  - "agent-busy"
  - "agent/inbox/dequeue"
  - "session/queue"
  - "Agent.cancel"
  - "AgentHandle.dispose"
  - "keepInbox"
  - "Web stop preserves pending Queue"
  - "bug fix"
  - "boundary"
  - "cancellation timeout"
search_regex: "(?i)(cancel|session\\.cancel|agent\\.cancel|InboxItemId|agent\\-busy|agent/inbox/dequeue|session/queue|AgentHandle\\.dispose)"
---

# 0333. Web stop preserves pending Queue — implementation context

## Open this when

The Web stop button reached session.cancel, which mapped to broad agent.cancel({ kind: 'user' }). During an active turn, ordinary composer submissions are already accepted as independently addressable Queue occurrences. Broad cancellation discarded every occurrence when the user intended to stop only the current generation, conflating turn interruption with the Queue's explicit delete operation. The browser cannot repair that loss by resending visible rows. It does not own their live InboxItemId, wake policy, or claim race, and a resend can duplicate work that the Host has already claimed.

## Source decision

session.cancel is the Web Host API's active-turn stop for ordinary sessions. It rejects session-backed subagents with agent-busy; otherwise it calls agent.cancel({ kind: 'user' }, { keepInbox: true }), preserving pending inbox work while cooperatively aborting the current turn. The underlying option preserves queued and steering entries; the Web Queue projection continues to expose only queued entries. The AgentLoop starts no concurrent replacement turn. It closes and flushes the interrupted turn, reaches cancellation quiescence, and then claims the next waking queued occurrence through its existing FIFO driver.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-31-web-stop-preserves-queue.md](../02-notes/implemented/bug-fix/2026-07-31-web-stop-preserves-queue.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-31-web-stop-preserves-queue.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-31-web-stop-preserves-queue.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-primitives/src/pointer-grace.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/pointer-grace.ts) | runtime implementation | Defines `cancel`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/README.md) | package contract and examples | Contains the exact code literal `session/queue` named by the note. | `exact-code-occurrence` |
| [`packages/client/runtime/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/README.zh.md) | package contract and examples | Contains the exact code literal `session/queue` named by the note. | `exact-code-occurrence` |
| [`packages/client/ui-conversation/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/README.md) | package contract and examples | Contains the exact code literal `session/queue` named by the note. | `exact-code-occurrence` |
| [`packages/client/ui-conversation/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/README.zh.md) | package contract and examples | Contains the exact code literal `session/queue` named by the note. | `exact-code-occurrence` |
| [`packages/client/runtime/src/client/sessions/queue-mirror.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/queue-mirror.ts) | runtime implementation | Contains the exact code literal `session/queue` named by the note. | `exact-code-occurrence` |
| [`packages/client/runtime/src/client/sessions/conversation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts) | runtime implementation | Contains the exact code literal `session/queue` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `cancel` | `const` | [`packages/client/ui-primitives/src/pointer-grace.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/pointer-grace.ts#L36) | `const cancel = useCallback(() => {` |

### Tests and executable evidence

- [`packages/core/agent-loop/tests/cancel.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/cancel.spec.ts) — A test under the owning area exercises or imports `keepInbox`.
- [`packages/api/remotes/tests/agent-lookup.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/tests/agent-lookup.spec.ts) — A test under the owning area exercises or imports `agent-busy`.
- [`packages/api/gateway/tests/gateway.host.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/tests/gateway.host.spec.ts) — A test under the owning area exercises or imports `agent-busy`.
- [`packages/host/apiproxy/tests/rpc-schemas.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/rpc-schemas.spec.ts) — A test under the owning area exercises or imports `agent-busy`.
- [`packages/host/apiproxy/tests/api-proxy-cold.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-cold.spec.ts) — A test under the owning area exercises or imports `agent-busy`.
- [`packages/client/runtime/tests/session.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/session.client.spec.ts) — A test under the owning area exercises or imports `agent-busy`.
- [`packages/client/ui-goal/tests/goalbar.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-goal/tests/goalbar.client.spec.tsx) — A test under the owning area exercises or imports `agent-busy`.
- [`packages/subagent/subagent/tests/continuation.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/continuation.spec.ts) — A test under the owning area exercises or imports `keepInbox`.
- Source verification intent: AgentLoop coverage holds an active model stream, queues two waking turns, cancels with keepInbox, and pins the aborted-then-completed turn reasons, FIFO user-message order, absence of discard events, and eventual idle state. The keyless Web scenario drives the built composition over HTTP/SSE: it stops one hung turn, observes the next queued occurrence start while the tail remains visible, stops that turn, and observes the final queued occurrence complete. Its accessibility snapshot pins the intermediate preserved-Queue state.

## How to read the implementation

1. Start with [`packages/client/ui-primitives/src/pointer-grace.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/pointer-grace.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `cancel`, `session.cancel`, `agent.cancel`, `InboxItemId`, `agent-busy`, `agent/inbox/dequeue`, `session/queue`, `Agent.cancel`, `AgentHandle.dispose`, `keepInbox`, `Web stop preserves pending Queue`, `bug fix`, `boundary`, `cancellation timeout`
- Regex: `(?i)(cancel|session\.cancel|agent\.cancel|InboxItemId|agent\-busy|agent/inbox/dequeue|session/queue|AgentHandle\.dispose)`

```bash
rg -n --pcre2 "(?i)(cancel|session\\.cancel|agent\\.cancel|InboxItemId|agent\\-busy|agent/inbox/dequeue|session/queue|AgentHandle\\.dispose)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`same-design-pressure`** — [0598. Address pending queue occurrences for edit and removal](0598-address-pending-queue-occurrences-for-edit-and-removal.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`.
- **`shares-code-with`** — [0167. Continuable background subagents](0167-continuable-background-subagents.md): Shares source implementation: `packages/subagent/subagent/tests/continuation.spec.ts`.
- **`same-design-pressure`** — [0278. Web background-job display](0278-web-background-job-display.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`.
- **`same-design-pressure`** — [0197. Web past-session search](0197-web-past-session-search.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`.
- **`shares-code-with`** — [0264. Continuable subagent current-turn interrupt](0264-continuable-subagent-current-turn-interrupt.md): Shares source implementation: `packages/subagent/subagent/tests/continuation.spec.ts`.
- **`same-design-pressure`** — [0219. Steer a queued Web message into the active turn](0219-steer-a-queued-web-message-into-the-active-turn.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`.
- **`same-design-pressure`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0333-web-stop-preserves-pending-queue.md`.
