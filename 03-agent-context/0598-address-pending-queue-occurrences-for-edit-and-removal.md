---
id: "dsh-note-0598"
title: "Address pending queue occurrences for edit and removal"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-29-addressable-queue-operations.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "SessionFace"
  - "MessageId"
  - "UserMessage"
  - "InboxItemId"
  - "InboxItem"
  - "Agent.updateInbox"
  - "not-found"
  - "agent/inbox/enqueue"
  - "session/queue"
  - "session.updateQueue"
  - "updateQueue"
  - "queue-item-not-found"
  - "aria-expanded"
  - "aria-controls"
search_regex: "(?i)(SessionFace|MessageId|UserMessage|InboxItemId|InboxItem|Agent\\.updateInbox|not\\-found|agent/inbox/enqueue)"
---

# 0598. Address pending queue occurrences for edit and removal — implementation context

## Open this when

The Web queue rendered pending messages but could not edit or delete one row. MessageId was insufficient as an address because callers may enqueue the same immutable message more than once. The browser also inferred queue retirement from turn and status events, so a row operation racing with driver claim had no authoritative outcome.

## Source decision

Each accepted FIFO occurrence has its own identity. AgentLoop mints an opaque InboxItemId and publishes an InboxItem containing that id, the identified UserMessage, and its acceptance-time queued | steering placement. Reusing one MessageId creates distinct inbox identities. Injection bypasses the FIFOs and receives no inbox identity. Mutation ends at driver claim. Agent.updateInbox(id, action) synchronously searches the pending queued FIFO. Edit replaces frozen content while preserving InboxItemId, MessageId, source, wake policy, and position. Remove emits the occurrence's terminal discard.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-29-addressable-queue-operations.md](../02-notes/archived/feature/2026-07-29-addressable-queue-operations.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-29-addressable-queue-operations.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-29-addressable-queue-operations.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm/src/brand.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts) | runtime implementation | Defines `MessageId`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/message.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/message.ts) | runtime implementation | Defines `UserMessage`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/contract/session.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/session.ts) | runtime implementation | Defines `SessionFace`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/README.md) | package contract and examples | Contains the exact code literal `session/queue` named by the note. | `exact-code-occurrence` |
| [`packages/client/runtime/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/README.zh.md) | package contract and examples | Contains the exact code literal `session/queue` named by the note. | `exact-code-occurrence` |
| [`packages/client/ui-conversation/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/README.md) | package contract and examples | Contains the exact code literal `session/queue` named by the note. | `exact-code-occurrence` |
| [`packages/client/ui-conversation/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/README.zh.md) | package contract and examples | Contains the exact code literal `session/queue` named by the note. | `exact-code-occurrence` |
| [`packages/client/runtime/src/client/sessions/queue-mirror.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/queue-mirror.ts) | runtime implementation | Contains the exact code literal `session/queue` named by the note. | `exact-code-occurrence` |
| [`packages/client/runtime/src/client/sessions/conversation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts) | runtime implementation | Contains the exact code literal `session/queue` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `SessionFace` | `type` | [`packages/client/runtime/src/client/contract/session.ts:89`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/session.ts#L89) | `export type SessionFace = ISession & ObservableSnapshot<ConversationSnapshot>` |
| `MessageId` | `type` | [`packages/llm/llm/src/brand.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts#L16) | `export type MessageId = Branded<'MessageId'>` |
| `MessageId` | `function` | [`packages/llm/llm/src/brand.ts:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts#L23) | `export function MessageId(id: string): MessageId {` |
| `UserMessage` | `interface` | [`packages/llm/llm/src/message.ts:141`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/message.ts#L141) | `export interface UserMessage extends Message {` |

### Tests and executable evidence

- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `aria-expanded`.
- [`apps/web/tests/workflow-run.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/workflow-run.e2e.ts) — A test under the owning area exercises or imports `aria-expanded`.
- [`apps/web/tests/plugin-config.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/plugin-config.e2e.ts) — A test under the owning area exercises or imports `discard`.
- [`apps/web/tests/queue-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/queue-actions.e2e.ts) — A test under the owning area exercises or imports `aria-expanded`.
- [`apps/web/tests/todo-row.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/todo-row.snapshot.ts) — A test under the owning area exercises or imports `aria-expanded`.
- [`apps/web/tests/pwsh-terminal.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/pwsh-terminal.e2e.ts) — A test under the owning area exercises or imports `aria-expanded`.
- [`apps/web/tests/schedule-after.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/schedule-after.e2e.ts) — A test under the owning area exercises or imports `aria-expanded`.
- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — A test under the owning area exercises or imports `aria-expanded`.
- Source verification intent: AgentLoop contract tests hold prompt admission while editing and removing exact queued occurrences, reject mutations of steering occurrences, and verify the resulting independent turn and terminal lifecycle events. Host schema and proxy tests cover queued-only authoritative snapshots, synchronous re-entrant mutation order, reconnect, cold-Agent rejection, typed not-found errors, and the RPC transport.

## How to read the implementation

1. Start with [`packages/llm/llm/src/brand.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/archived`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `SessionFace`, `MessageId`, `UserMessage`, `InboxItemId`, `InboxItem`, `Agent.updateInbox`, `not-found`, `agent/inbox/enqueue`, `session/queue`, `session.updateQueue`, `updateQueue`, `queue-item-not-found`, `aria-expanded`, `aria-controls`
- Regex: `(?i)(SessionFace|MessageId|UserMessage|InboxItemId|InboxItem|Agent\.updateInbox|not\-found|agent/inbox/enqueue)`

```bash
rg -n --pcre2 "(?i)(SessionFace|MessageId|UserMessage|InboxItemId|InboxItem|Agent\\.updateInbox|not\\-found|agent/inbox/enqueue)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0296. Status-driven disclosure for workflow runs](0296-status-driven-disclosure-for-workflow-runs.md): Shares source implementation: `apps/web/tests/pwsh-terminal.e2e.ts`, `apps/web/tests/queue-actions.e2e.ts`.
- **`shares-code-with`** — [0219. Steer a queued Web message into the active turn](0219-steer-a-queued-web-message-into-the-active-turn.md): Shares source implementation: `packages/llm/llm/src/brand.ts`, `packages/llm/llm/src/message.ts`.
- **`shares-code-with`** — [0200. Continuable subagents](0200-continuable-subagents.md): Shares source implementation: `packages/llm/llm/src/brand.ts`, `packages/llm/llm/src/message.ts`.
- **`shares-code-with`** — [0088. Claim inbox input before one pre-step decision](0088-claim-inbox-input-before-one-pre-step-decision.md): Shares source implementation: `packages/llm/llm/src/brand.ts`, `packages/llm/llm/src/message.ts`.
- **`shares-code-with`** — [0053. Unify agent delivery on send(target × wakeup) and coalesce injected context into user/message](0053-unify-agent-delivery-on-send-target-wakeup-and-coalesce-injected-context.md): Shares source implementation: `packages/llm/llm/src/brand.ts`, `packages/llm/llm/src/message.ts`.
- **`shares-code-with`** — [0057. Project-grouped session directories](0057-project-grouped-session-directories.md): Shares source implementation: `apps/web/tests/pwsh-terminal.e2e.ts`, `apps/web/tests/queue-actions.e2e.ts`.
- **`shares-code-with`** — [0037. Provider-routed LLM adapters and a generic pi-ai backend](0037-provider-routed-llm-adapters-and-a-generic-pi-ai-backend.md): Shares source implementation: `packages/llm/llm/src/message.ts`.
- **`shares-code-with`** — [0071. Create every message as an identified immutable value](0071-create-every-message-as-an-identified-immutable-value.md): Shares source implementation: `packages/llm/llm/src/brand.ts`, `packages/llm/llm/src/message.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0598-address-pending-queue-occurrences-for-edit-and-removal.md`.
