---
id: "dsh-note-0308"
title: "Recursive Python SDK session notifications"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-24-recursive-python-sdk-session-notifications.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "sessionId"
  - "started"
  - "finished"
  - "event"
  - "events"
  - "notifications"
  - "HarnessClient"
  - "session.event"
  - "subagent.started"
  - "subagent.finished"
  - "Session.run"
  - "TurnResult.notifications"
  - "on_notification"
  - "TurnResult.events"
search_regex: "(?i)(sessionId|started|finished|event|events|notifications|HarnessClient|session\\.event)"
---

# 0308. Recursive Python SDK session notifications — implementation context

## Open this when

The Python SDK filtered turn notifications by comparing each payload directly with the root session id. This admitted a direct child's lifecycle because its parent id named the root, but rejected a grandchild's lifecycle and every descendant session.event. The JSON-RPC server still emitted those notifications, so they accumulated on the low-level global queue while high-level consumers lost nested trajectory relationships and completion states.

## Source decision

HarnessClient records every valid subagent.started child-to-parent edge before dispatching the notification. A later subagent.finished routes by its immutable parent id but never rewrites current ancestry, so an older run that settles after its child id has been reused cannot displace the replacement session. Other session notifications resolve their session id by walking that client-lifetime ancestry graph to the requested root.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-24-recursive-python-sdk-session-notifications.md](../02-notes/implemented/bug-fix/2026-07-24-recursive-python-sdk-session-notifications.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-24-recursive-python-sdk-session-notifications.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-24-recursive-python-sdk-session-notifications.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `sessionId`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/client/src/api.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/api.ts) | runtime implementation | Defines `event`, a construct named by the note. Defines `notifications`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent/src/inbox.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/inbox.ts) | runtime implementation | Defines `event`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/client/src/client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/client.ts) | runtime implementation | Defines `HarnessClient`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Defines `finished`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Defines `event`, a construct named by the note. Defines `events`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/repair.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts) | runtime implementation | Defines `started`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts) | package entry point | Defines `events`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-deepseek/src/sse.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/sse.ts) | runtime implementation | Defines `events`, a construct named by the note. | `symbol-definition` |
| [`packages/bundle/headless/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts) | package entry point | Defines `started`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-pi-ai/src/adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts) | provider/backend adapter | Defines `events`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `sessionId`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `sessionId` | `const` | [`packages/acp/acp/src/index.ts:254`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L254) | `const sessionId = SessionId(randomUUID())` |
| `started` | `let` | [`packages/bundle/headless/src/index.ts:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts#L62) | `let started = false` |
| `finished` | `const` | [`packages/client/ui-jobs/src/client/JobListAction.tsx:82`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-jobs/src/client/JobListAction.tsx#L82) | `const finished = (right.finishedAt ?? right.startedAt) - (left.finishedAt ?? left.startedAt)` |
| `finished` | `const` | [`packages/client/ui-settings-models/src/client/WelcomeNotice.tsx:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/WelcomeNotice.tsx#L37) | `const finished = useRef(false)` |
| `finished` | `const` | [`packages/code-runtime/code-runtime-worker-thread/src/index.ts:423`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/index.ts#L423) | `const finished = new Promise<void>((done) => { finishResolve = done })` |
| `started` | `let` | [`packages/core/agent-loop/src/tool-calls.ts:137`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/tool-calls.ts#L137) | `let started = 0` |
| `event` | `const` | [`packages/core/agent/src/inbox.ts:186`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/inbox.ts#L186) | `const event = this.session.append('agent/inbox/spliced', splice)` |
| `event` | `const` | [`packages/core/session/src/index.ts:214`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L214) | `const event = value` |
| `event` | `const` | [`packages/core/session/src/index.ts:627`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L627) | `const event = deepFreeze({` |
| `events` | `const` | [`packages/core/session/src/index.ts:1098`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L1098) | `const events = session.events` |
| `started` | `const` | [`packages/core/session/src/repair.ts:92`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts#L92) | `const started = callSeq !== undefined` |
| `sessionId` | `const` | [`packages/e2b/subprocess-e2b/src/terminal.ts:517`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/terminal.ts#L517) | `const sessionId = await terminalSessionId(sandbox, handle.pid, controlEnvs, spec.signal)` |
| `started` | `const` | [`packages/hooks/hook-protocol/src/runner.ts:73`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/runner.ts#L73) | `const started = now()` |
| `sessionId` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1371`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1371) | `const sessionId = request.agent?.id` |
| `sessionId` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2168`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2168) | `const sessionId = request.payload.sessionId ?? \`session-${randomUUID()}\` as SessionId` |
| `events` | `const` | [`packages/llm/llm-deepseek/src/sse.ts:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/sse.ts#L32) | `const events = stream` |

### Tests and executable evidence

- [`python/sdk/tests/test_client.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_client.py) — A test under the owning area exercises or imports `HarnessClient`. A test under the owning area exercises or imports `on_notification`.
- [`python/sdk/tests/test_bundled_runtime.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_bundled_runtime.py) — A test under the owning area exercises or imports `HarnessClient`.
- [`packages/sdk/client/tests/sdk-client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/tests/sdk-client.spec.ts) — A test under the owning area exercises or imports `HarnessClient`.

## How to read the implementation

1. Start with [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `sessionId`, `started`, `finished`, `event`, `events`, `notifications`, `HarnessClient`, `session.event`, `subagent.started`, `subagent.finished`, `Session.run`, `TurnResult.notifications`, `on_notification`, `TurnResult.events`
- Regex: `(?i)(sessionId|started|finished|event|events|notifications|HarnessClient|session\.event)`

```bash
rg -n --pcre2 "(?i)(sessionId|started|finished|event|events|notifications|HarnessClient|session\\.event)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0364. Owned-run finish reason reporting](0364-owned-run-finish-reason-reporting.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0084. Follow-up enqueue and owned run boundaries](0084-follow-up-enqueue-and-owned-run-boundaries.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/agent/src/inbox.ts`.
- **`shares-code-with`** — [0100. Large-session JSONL restore pipeline](0100-large-session-jsonl-restore-pipeline.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/llm/llm-deepseek/src/sse.ts`.
- **`shares-code-with`** — [0196. Web session fork actions](0196-web-session-fork-actions.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/repair.ts`.
- **`shares-code-with`** — [0195. TypeScript SDK client and the SDK subagent backend](0195-typescript-sdk-client-and-the-sdk-subagent-backend.md): Shares source implementation: `packages/core/agent/src/inbox.ts`, `packages/sdk/client/src/api.ts`.
- **`shares-code-with`** — [0141. SessionStore fork API](0141-sessionstore-fork-api.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0494. Property-based testing for protocol-shaped code](0494-property-based-testing-for-protocol-shaped-code.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/repair.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0308-recursive-python-sdk-session-notifications.md`.
