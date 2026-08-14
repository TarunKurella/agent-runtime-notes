---
id: "dsh-note-0292"
title: "Web surface for message feedback"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-11-message-feedback-web-surface.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/generation"
aliases:
  - "AssistantMessageNode"
  - "SteeringMessageNode"
  - "list"
  - "MessageIconActions"
  - "TurnTailNodeView"
  - "turn"
  - "messageId"
  - "step"
  - "current"
  - "MessageFeedbackController"
  - "seq"
  - "isAppendSurfaceEvent"
  - "MessageFeedbackVersionConflict"
  - "cold"
search_regex: "(?i)(AssistantMessageNode|SteeringMessageNode|list|MessageIconActions|TurnTailNodeView|turn|messageId|step)"
---

# 0292. Web surface for message feedback — implementation context

## Open this when

PR #2217 landed the durable message-feedback sidecar and its three Host Remote methods, but it was explicitly backend-only: no client package consumed messageFeedback.list, put, or delete, so the Web GUI had no way to record a rating. Its Agent Note deferred "client Remote aggregate mounting and UI" to a separate owner. Issue #1326 asks for the Web surface and was closed by that backend merge without the user-visible half existing.

## Source decision

Three seams, each owned where its authority already lives. Message identity in the client node. AssistantMessageNode gains an optional messageId, copied from event.data.message.id where the node is materialized from a finalized assistant/message. It stays absent on interruption-frozen partials, which were never finalized and address no durable message, and on the synthetic sentinel the trajectory layout builds for an unfinalized partial. The field is optional precisely so those two cases remain unrepresentable as feedback targets rather than being papered over with a placeholder.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-11-message-feedback-web-surface.md](../02-notes/implemented/feature/2026-08-11-message-feedback-web-surface.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-11-message-feedback-web-surface.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-11-message-feedback-web-surface.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/api/remotes/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/src/index.ts) | package entry point | Core file in the package named by the note: `packages/api/remotes`. | `named-package-member` |
| [`packages/api/remotes/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/api/remotes`. | `named-package-member` |
| [`packages/api/remotes/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/api/remotes`. | `named-package-member` |
| [`packages/client/ui-trajectory/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-trajectory`. | `named-package-member` |
| [`packages/client/ui-conversation/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-trajectory/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-trajectory`. | `named-package-member` |
| [`packages/client/ui-conversation/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-message-feedback/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-message-feedback/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-message-feedback`. | `named-package-member` |
| [`packages/client/ui-trajectory/src/client/layout.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-trajectory`. Defines `turn`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-message-feedback/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-message-feedback/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-message-feedback`. | `named-package-member` |
| [`packages/client/ui-conversation/src/client/input/facade.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/facade.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. Defines `current`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/input/blocks.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/blocks.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. Defines `current`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `AssistantMessageNode` | `interface` | [`packages/client/runtime/src/client/sessions/conversation.ts:96`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts#L96) | `export interface AssistantMessageNode {` |
| `SteeringMessageNode` | `interface` | [`packages/client/runtime/src/client/sessions/conversation.ts:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts#L121) | `export interface SteeringMessageNode {` |
| `list` | `const` | [`packages/client/ui-conversation/src/client/chat/ContextBody.tsx:198`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ContextBody.tsx#L198) | `const list = record === null ? undefined : record['changes']` |
| `list` | `const` | [`packages/client/ui-conversation/src/client/chat/ContextBody.tsx:285`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ContextBody.tsx#L285) | `const list = record === null ? undefined : record['entries']` |
| `list` | `const` | [`packages/client/ui-conversation/src/client/chat/ContextBody.tsx:357`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ContextBody.tsx#L357) | `const list = record === null ? undefined : record['sections']` |
| `list` | `const` | [`packages/client/ui-conversation/src/client/chat/ContextBody.tsx:471`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ContextBody.tsx#L471) | `const list = record === null ? undefined : record['references']` |
| `MessageIconActions` | `function` | [`packages/client/ui-conversation/src/client/chat/MessageIconActions.tsx:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/MessageIconActions.tsx#L46) | `export function MessageIconActions({` |
| `TurnTailNodeView` | `const` | [`packages/client/ui-conversation/src/client/chat/TurnTailNodeView.tsx:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/TurnTailNodeView.tsx#L12) | `export const TurnTailNodeView = memo(function TurnTailNodeView({` |
| `turn` | `const` | [`packages/client/ui-conversation/src/client/chat/TurnTailNodeView.tsx:18`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/TurnTailNodeView.tsx#L18) | `const turn = node.location.kind === 'turn' \|\| node.location.kind === 'step'` |
| `messageId` | `const` | [`packages/client/ui-conversation/src/client/chat/TurnTailNodeView.tsx:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/TurnTailNodeView.tsx#L31) | `const messageId = closing.finalNode.messageId` |
| `step` | `const` | [`packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts:78`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts#L78) | `const step = stepKey(coordinates.turn, coordinates.step)` |
| `current` | `const` | [`packages/client/ui-conversation/src/client/conversation-nodes/retry.ts:84`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/retry.ts#L84) | `const current = attempts.at(-1)` |
| `current` | `const` | [`packages/client/ui-conversation/src/client/input/blocks.ts:59`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/blocks.ts#L59) | `const current = store.getSnapshot()` |
| `current` | `const` | [`packages/client/ui-conversation/src/client/input/facade.ts:144`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/facade.ts#L144) | `const current = new Set(this.imageIds)` |
| `current` | `const` | [`packages/client/ui-conversation/src/client/skeleton/PermissionSelect.tsx:88`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/PermissionSelect.tsx#L88) | `const current = value.options.find(option => option.value === currentValue)` |
| `MessageFeedbackController` | `class` | [`packages/client/ui-message-feedback/src/client/controller.ts:105`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-message-feedback/src/client/controller.ts#L105) | `export class MessageFeedbackController implements HostObservable<MessageFeedbackView> {` |

### Tests and executable evidence

- [`packages/core/session/tests/surface.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/surface.spec.ts) — A test under the owning area exercises or imports `isAppendSurfaceEvent`.
- [`packages/client/ui-conversation/tests/chat-view.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-view.client.spec.tsx) — A test under the owning area exercises or imports `AssistantMessageNode`. A test under the owning area exercises or imports `turn-tail`.
- [`packages/client/ui-conversation/tests/chat-stats.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-stats.client.spec.tsx) — A test under the owning area exercises or imports `AssistantMessageNode`.
- [`packages/client/ui-trajectory/tests/client-bundle.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/tests/client-bundle.client.spec.ts) — A test under the owning area exercises or imports `ui-trajectory`.
- [`packages/client/ui-conversation/tests/turn-metrics.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/turn-metrics.client.spec.ts) — A test under the owning area exercises or imports `AssistantMessageNode`.
- [`packages/client/ui-conversation/tests/apply-inject.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/apply-inject.client.spec.tsx) — A test under the owning area exercises or imports `ui-trajectory`.
- [`packages/client/ui-message-feedback/tests/controller.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-message-feedback/tests/controller.client.spec.ts) — A test under the owning area exercises or imports `ifVersion`. A test under the owning area exercises or imports `MessageId`.
- [`packages/client/ui-conversation/tests/chat-snapshot-fixture.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-snapshot-fixture.client.ts) — A test under the owning area exercises or imports `AssistantMessageNode`. A test under the owning area exercises or imports `turn-tail`.

## How to read the implementation

1. Start with [`packages/api/remotes/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `concern/simplification`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/generation`
- Aliases: `AssistantMessageNode`, `SteeringMessageNode`, `list`, `MessageIconActions`, `TurnTailNodeView`, `turn`, `messageId`, `step`, `current`, `MessageFeedbackController`, `seq`, `isAppendSurfaceEvent`, `MessageFeedbackVersionConflict`, `cold`
- Regex: `(?i)(AssistantMessageNode|SteeringMessageNode|list|MessageIconActions|TurnTailNodeView|turn|messageId|step)`

```bash
rg -n --pcre2 "(?i)(AssistantMessageNode|SteeringMessageNode|list|MessageIconActions|TurnTailNodeView|turn|messageId|step)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares source implementation: `packages/api/remotes/src/index.ts`, `packages/api/remotes/src/types.ts`.
- **`shares-code-with`** — [0076. Projected token usage and context occupancy](0076-projected-token-usage-and-context-occupancy.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0182. Session model selection in the Web composer](0182-session-model-selection-in-the-web-composer.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0427. Ordered Build for API Remotes Generated Contracts](0427-ordered-build-for-api-remotes-generated-contracts.md): Shares source implementation: `packages/api/remotes/src/index.ts`, `packages/api/remotes/src/invariant.ts`.
- **`shares-code-with`** — [0208. Ask-question Web presentation](0208-ask-question-web-presentation.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0675. Web UI drops steer entry and interjection chrome](0675-web-ui-drops-steer-entry-and-interjection-chrome.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0294. Web attachment display aligns with DeepSeek Chat via attachment atoms](0294-web-attachment-display-aligns-with-deepseek-chat-via-attachment-atoms.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0292-web-surface-for-message-feedback.md`.
