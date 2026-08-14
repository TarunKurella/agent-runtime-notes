---
id: "dsh-note-0084"
title: "Follow-up enqueue and owned run boundaries"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-30-followup-enqueue-and-owned-runs.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/agent-loop"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "stopReason"
  - "idle"
  - "finished"
  - "cancelled"
  - "event"
  - "run"
  - "reason"
  - "MessageId"
  - "RunResult"
  - "finish_reason"
  - "Agent.followup"
  - "turn/end"
  - "Agent.followup(message): void"
  - "Agent.whenIdle"
search_regex: "(?i)(stopReason|idle|finished|cancelled|event|reason|MessageId|RunResult)"
---

# 0084. Follow-up enqueue and owned run boundaries — implementation context

## Open this when

Agent.followup() identifies and queues a user message, but one follow-up does not own the activity that follows it. Steering, injected context, tool continuations, recovery, and later queued messages can all contribute before the agent next becomes idle. A MessageId can therefore prove inbox admission, but it cannot identify which assistant message or turn/end is the result of that input. The one-send-one-turn decision already rejects a per-send completion handle in the core API. Protocol and SDK layers that pair one prompt request with a turn result manufacture that missing relationship downstream.

## Source decision

Keep Agent.followup(message): void as an enqueue-only operation. Agent.whenIdle() and agent/status remain whole-agent lifecycle observations; neither settles an individual message. Inbox durability records the identified message and its admission or cancellation, without assigning later output to it. The low-level SDK protocol answers session/prompt as soon as enqueue succeeds with { messageId }. It streams durable facts through session.event, publishes whole-agent transitions through session.status, and has no session.finished.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-30-followup-enqueue-and-owned-runs.md](../02-notes/implemented/architecture/2026-07-30-followup-enqueue-and-owned-runs.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-30-followup-enqueue-and-owned-runs.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-30-followup-enqueue-and-owned-runs.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/include/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts) | package entry point | Defines `run`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/brand.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts) | runtime implementation | Defines `MessageId`, a construct named by the note. | `symbol-definition` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `stopReason`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/client/src/api.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/api.ts) | runtime implementation | Defines `event`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent/src/inbox.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/inbox.ts) | runtime implementation | Defines `event`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/client/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/types.ts) | public types and contract | Defines `RunResult`, a construct named by the note. | `symbol-definition` |
| [`packages/e2b/fs-e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts) | package entry point | Defines `run`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Defines `run`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `reason`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Defines `finished`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Defines `event`, a construct named by the note. | `symbol-definition` |
| [`packages/util/timeout/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/index.ts) | package entry point | Defines `reason`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `stopReason` | `const` | [`packages/acp/acp/src/index.ts:297`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L297) | `const stopReason = await new Promise<StopReason>((resolve, reject) => {` |
| `idle` | `const` | [`packages/client/connection/src/client/connection.ts:166`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/connection.ts#L166) | `const idle = new AbortController()` |
| `finished` | `const` | [`packages/client/ui-jobs/src/client/JobListAction.tsx:82`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-jobs/src/client/JobListAction.tsx#L82) | `const finished = (right.finishedAt ?? right.startedAt) - (left.finishedAt ?? left.startedAt)` |
| `finished` | `const` | [`packages/client/ui-settings-models/src/client/WelcomeNotice.tsx:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/WelcomeNotice.tsx#L37) | `const finished = useRef(false)` |
| `finished` | `const` | [`packages/code-runtime/code-runtime-worker-thread/src/index.ts:423`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/index.ts#L423) | `const finished = new Promise<void>((done) => { finishResolve = done })` |
| `cancelled` | `function` | [`packages/context/session-reference/src/index.ts:299`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/index.ts#L299) | `function cancelled(signal: AbortSignal): SessionReferenceError {` |
| `event` | `const` | [`packages/core/agent/src/inbox.ts:186`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/inbox.ts#L186) | `const event = this.session.append('agent/inbox/spliced', splice)` |
| `run` | `const` | [`packages/core/agent/src/index.ts:642`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L642) | `const run: InitiatorRun = {` |
| `run` | `let` | [`packages/core/agent/src/index.ts:689`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L689) | `let run = this.initiatorRuns.getStore()` |
| `event` | `const` | [`packages/core/session/src/index.ts:214`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L214) | `const event = value` |
| `event` | `const` | [`packages/core/session/src/index.ts:627`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L627) | `const event = deepFreeze({` |
| `reason` | `const` | [`packages/core/tools/src/index.ts:749`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L749) | `const reason = guard(exec)` |
| `reason` | `const` | [`packages/core/tools/src/index.ts:1124`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1124) | `const reason = layer.guardReason(exec)` |
| `reason` | `const` | [`packages/core/tools/src/index.ts:1901`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1901) | `const reason: unknown = source.reason` |
| `run` | `const` | [`packages/e2b/fs-e2b/src/index.ts:433`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L433) | `const run = prior.then(operation, operation)` |
| `stopReason` | `const` | [`packages/hooks/hook-protocol/src/codec.ts:100`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/codec.ts#L100) | `const stopReason = str(parsed, 'stopReason')` |

### Tests and executable evidence

- [`python/sdk/tests/test_client.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_client.py) — A test under the owning area exercises or imports `finish_reason`. Contains the exact code literal `session/prompt` named by the note.
- [`examples/acp-agent/tests/acp.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/acp.e2e.ts) — A test under the owning area exercises or imports `end_turn`.
- [`packages/acp/acp/tests/edges.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/edges.spec.ts) — A test under the owning area exercises or imports `stopReason`. A test under the owning area exercises or imports `end_turn`.
- [`packages/acp/acp/tests/turns.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/turns.spec.ts) — A test under the owning area exercises or imports `stopReason`. A test under the owning area exercises or imports `end_turn`.
- [`packages/acp/acp/tests/codec.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/codec.spec.ts) — A test under the owning area exercises or imports `end_turn`.
- [`packages/acp/acp/tests/bridge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/bridge.spec.ts) — A test under the owning area exercises or imports `stopReason`. A test under the owning area exercises or imports `end_turn`.
- [`examples/acp-agent/tests/hooks.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/hooks.e2e.ts) — A test under the owning area exercises or imports `end_turn`.
- [`packages/acp/acp/tests/dispose.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/dispose.spec.ts) — A test under the owning area exercises or imports `stopReason`.
- Source verification intent: Agent and inbox tests pin enqueue-only follow-up, durable admission or cancellation, and whole-agent idle observation. SDK protocol, TypeScript SDK, and Python SDK tests pin the { messageId } receipt, session.status, the absence of session.finished, and receipt-to-idle RunResult collection without prompt-level status or reason; Python SDK tests separately pin its run-level finish_reason observation. ACP, one-shot CLI, goal continuation, and subagent tests pin the distinct activity ownership each integration possesses.

## How to read the implementation

1. Start with [`vendor/include/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/agent-loop`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `stopReason`, `idle`, `finished`, `cancelled`, `event`, `run`, `reason`, `MessageId`, `RunResult`, `finish_reason`, `Agent.followup`, `turn/end`, `Agent.followup(message): void`, `Agent.whenIdle`
- Regex: `(?i)(stopReason|idle|finished|cancelled|event|reason|MessageId|RunResult)`

```bash
rg -n --pcre2 "(?i)(stopReason|idle|finished|cancelled|event|reason|MessageId|RunResult)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0364. Owned-run finish reason reporting](0364-owned-run-finish-reason-reporting.md): The source note links to this decision directly.
- **`source-link`** — [0455. Remove implicit batching from ordinary sends](0455-remove-implicit-batching-from-ordinary-sends.md): The source note links to this decision directly.
- **`shares-code-with`** — [0195. TypeScript SDK client and the SDK subagent backend](0195-typescript-sdk-client-and-the-sdk-subagent-backend.md): Shares source implementation: `packages/core/agent/src/inbox.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0308. Recursive Python SDK session notifications](0308-recursive-python-sdk-session-notifications.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/agent/src/inbox.ts`.
- **`shares-code-with`** — [0144. The approval seam --- one-shot permission decisions over a waterfall of answerers](0144-the-approval-seam-one-shot-permission-decisions-over-a-waterfall-of-answ.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0200. Continuable subagents](0200-continuable-subagents.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0071. Create every message as an identified immutable value](0071-create-every-message-as-an-identified-immutable-value.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/llm/llm/src/brand.ts`.
- **`shares-code-with`** — [0538. Collapse workflows to the exercised foreground core](0538-collapse-workflows-to-the-exercised-foreground-core.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/agent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0084-follow-up-enqueue-and-owned-run-boundaries.md`.
