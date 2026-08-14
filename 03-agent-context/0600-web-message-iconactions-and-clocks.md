---
id: "dsh-note-0600"
title: "Web message IconActions and clocks"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-29-web-message-icon-actions-and-clock.md"
implementation_evidence: "high"
target_anchor: "append-only event and session store"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/agent-loop"
  - "domain/extensions"
  - "domain/session-state"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/streaming"
aliases:
  - "seq"
  - "AssistantMarkdown"
  - "ChatView"
  - "formatMessageClock"
  - "useCalendarDay"
  - "streaming"
  - "time"
  - "Date.now"
  - "HH:mm"
  - "margin-top: 16px"
  - "turn/end"
  - "node.time"
  - "M月D日 HH:mm"
  - "YYYY年M月D日 HH:mm"
search_regex: "(?i)(AssistantMarkdown|ChatView|formatMessageClock|useCalendarDay|streaming|time|Date\\.now|HH:mm)"
---

# 0600. Web message IconActions and clocks — implementation context

## Open this when

The web chat user bubble already had copy / branch / edit IconActions but no clock. Finalized assistant narration had no under-body action chrome at all, even though the Harness design shows a copy / branch / clock row after the answer settles. Streaming replies must not flash that chrome mid-token. Memoized rows also keep stable props across midnight, so a one-shot Date.now() would leave yesterday's messages stuck on HH:mm.

## Source decision

User bubbles prepend a date-aware local clock to the existing IconActions row; the last content-text assistant of each turn appends a copy / branch / clock row with margin-top: 16px; both seats stay visible whenever mounted and re-format at the next local midnight. The assistant seat is narrowed by the completed-turn decision: only a turn with a turn/end grants it, so a turn still producing steps hands the row to nothing. The user seat's branch control is removed outright by the user-bubble branch removal; a user row's IconActions are clock and copy.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-29-web-message-icon-actions-and-clock.md](../02-notes/archived/feature/2026-07-29-web-message-icon-actions-and-clock.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-29-web-message-icon-actions-and-clock.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-29-web-message-icon-actions-and-clock.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Defines `time`, a construct named by the note. Defines `seq`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/repair.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts) | runtime implementation | Defines `time`, a construct named by the note. Defines `seq`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/chunk-rows.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts) | runtime implementation | Defines `time`, a construct named by the note. | `symbol-definition` |
| [`packages/schedule/schedule/src/domain.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/domain.ts) | runtime implementation | Defines `time`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `seq`, a construct named by the note. | `symbol-definition` |
| [`packages/workflow/tool-workflow/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/invariant.ts) | runtime contract checks | Defines `seq`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/layout.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts) | runtime implementation | Defines `streaming`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/ChatView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx) | runtime implementation | Defines `ChatView`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/message-chrome.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/message-chrome.ts) | runtime implementation | Defines `formatMessageClock`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/use-calendar-day.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/use-calendar-day.ts) | runtime implementation | Defines `useCalendarDay`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/AssistantMarkdown.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/AssistantMarkdown.tsx) | runtime implementation | Defines `AssistantMarkdown`, a construct named by the note. | `symbol-definition` |
| [`tsconfig.host.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.host.json) | composition and configuration | Contains the exact code literal `apps/web/tests/message-actions.e2e.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `seq` | `const` | [`packages/client/connection/src/client/fixture.ts:362`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L362) | `const seq = events.length` |
| `AssistantMarkdown` | `const` | [`packages/client/ui-conversation/src/client/chat/AssistantMarkdown.tsx:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/AssistantMarkdown.tsx#L37) | `export const AssistantMarkdown = memo(function AssistantMarkdown({` |
| `ChatView` | `function` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:146`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L146) | `export function ChatView({` |
| `formatMessageClock` | `function` | [`packages/client/ui-conversation/src/client/chat/message-chrome.ts:82`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/message-chrome.ts#L82) | `export function formatMessageClock(time: number, t: ClockTranslate, now: number = Date.now()): string {` |
| `useCalendarDay` | `function` | [`packages/client/ui-conversation/src/client/chat/use-calendar-day.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/use-calendar-day.ts#L12) | `export function useCalendarDay(): number {` |
| `streaming` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:679`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L679) | `const streaming = opts?.streaming === true` |
| `time` | `let` | [`packages/core/session/src/chunk-rows.ts:284`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts#L284) | `let time = value.time0 as number` |
| `time` | `let` | [`packages/core/session/src/chunk-rows.ts:296`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts#L296) | `let time = row.time0` |
| `seq` | `const` | [`packages/core/session/src/index.ts:233`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L233) | `const seq = event['seq']` |
| `time` | `const` | [`packages/core/session/src/index.ts:234`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L234) | `const time = event['time']` |
| `seq` | `let` | [`packages/core/session/src/repair.ts:85`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts#L85) | `let seq = last.seq + 1` |
| `time` | `const` | [`packages/core/session/src/repair.ts:86`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts#L86) | `const time = last.time` |
| `time` | `const` | [`packages/schedule/schedule/src/domain.ts:277`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/domain.ts#L277) | `const time = timeMatch?.groups` |
| `seq` | `const` | [`packages/workflow/tool-workflow/src/invariant.ts:91`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/invariant.ts#L91) | `const seq = memberSeq(data.seq, fail)` |
| `seq` | `const` | [`packages/workflow/tool-workflow/src/invariant.ts:103`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/invariant.ts#L103) | `const seq = memberSeq(data.seq, fail)` |

### Tests and executable evidence

- [`apps/web/tests/message-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/message-actions.e2e.ts) — The source note names this file directly.
- [`packages/client/ui-conversation/tests/chat-view.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-view.client.spec.tsx) — A test under the owning area exercises or imports `MessageItem`. A test under the owning area exercises or imports `ChatView`.
- [`packages/client/ui-conversation/tests/chat-apply.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-apply.client.spec.tsx) — A test under the owning area exercises or imports `ChatView`.
- [`packages/client/ui-conversation/tests/image-labels.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/image-labels.client.spec.tsx) — A test under the owning area exercises or imports `AssistantMarkdown`.
- [`packages/client/ui-conversation/tests/reasoning-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/reasoning-row.client.spec.tsx) — A test under the owning area exercises or imports `AssistantMarkdown`.
- [`packages/client/ui-conversation/tests/coverage-tails.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/coverage-tails.client.spec.tsx) — A test under the owning area exercises or imports `ChatView`. A test under the owning area exercises or imports `AssistantMarkdown`.
- [`packages/client/ui-conversation/tests/chat-branch-tails.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-branch-tails.client.spec.tsx) — A test under the owning area exercises or imports `formatMessageClock`. A test under the owning area exercises or imports `useCalendarDay`.
- [`packages/client/ui-conversation/tests/gate-branch-tails.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/gate-branch-tails.client.spec.tsx) — A test under the owning area exercises or imports `AssistantMarkdown`.

## How to read the implementation

1. Start with [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** append-only event and session store.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `domain/agent-loop`, `domain/extensions`, `domain/session-state`, `domain/testing`, `lifecycle/archived`, `mechanism/policy`, `mechanism/streaming`
- Aliases: `seq`, `AssistantMarkdown`, `ChatView`, `formatMessageClock`, `useCalendarDay`, `streaming`, `time`, `Date.now`, `HH:mm`, `margin-top: 16px`, `turn/end`, `node.time`, `M月D日 HH:mm`, `YYYY年M月D日 HH:mm`
- Regex: `(?i)(AssistantMarkdown|ChatView|formatMessageClock|useCalendarDay|streaming|time|Date\.now|HH:mm)`

```bash
rg -n --pcre2 "(?i)(AssistantMarkdown|ChatView|formatMessageClock|useCalendarDay|streaming|time|Date\\.now|HH:mm)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0110. Bounded session persistence write batching](0110-bounded-session-persistence-write-batching.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/core/session/src/chunk-rows.ts`.
- **`shares-code-with`** — [0113. Client Conversation business-node assembly and keyed Chat snapshots](0113-client-conversation-business-node-assembly-and-keyed-chat-snapshots.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/ChatView.tsx`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0012. Session surface --- an ordered projection over the event log](0012-session-surface-an-ordered-projection-over-the-event-log.md): Shares source implementation: `packages/core/session/src/chunk-rows.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0196. Web session fork actions](0196-web-session-fork-actions.md): Shares source implementation: `apps/web/tests/message-actions.e2e.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0446. Drop the mutable session summary](0446-drop-the-mutable-session-summary.md): Shares source implementation: `packages/core/session/src/chunk-rows.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0194. Trajectory inspection ledger](0194-trajectory-inspection-ledger.md): Shares source implementation: `apps/web/tests/message-actions.e2e.ts`, `packages/client/connection/src/client/fixture.ts`.
- **`shares-code-with`** — [0085. the end-seed log boundary](0085-the-end-seed-log-boundary.md): Shares source implementation: `packages/core/session/src/chunk-rows.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/repair.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0600-web-message-iconactions-and-clocks.md`.
