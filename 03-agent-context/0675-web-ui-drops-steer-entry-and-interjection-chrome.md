---
id: "dsh-note-0675"
title: "Web UI drops steer entry and interjection chrome"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-07-31-web-ui-no-steer-entry-or-interjection-chrome.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/simplification"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
aliases:
  - "inject"
  - "send"
  - "queue"
  - "SessionInput"
  - "InputMachine"
  - "submit"
  - "context"
  - "user/message"
  - "conversation.send"
  - "InputActions.submit"
  - "defaultSink"
  - "session.prompt"
  - "ConversationService.send"
  - "message.steering"
search_regex: "(?i)(inject|send|queue|SessionInput|InputMachine|submit|context|user/message)"
---

# 0675. Web UI drops steer entry and interjection chrome — implementation context

## Open this when

Mid-turn steering is a host/agent-loop capability (mode:'steer', a durable user/message). The Web product already locked the composer while a turn runs and never shipped a queue/steer menu, yet the client still threaded 'queue' | 'steer' through the input machine, conversation.send, and locale keys, and rendered consumed steering as a badged 「插话」/「Interjection」 bubble. That left a half-built UI surface: an unused submit mode, a product label for a gesture users cannot perform, and e2e goldens that pinned chrome the product does not own.

## Source decision

Keep host and runtime steering intact. Remove only the Web UI entry and chrome: InputMachine / SessionInput / InputActions.submit / hub defaultSink are queue-only; they always call session.prompt(..., 'queue'). ConversationService.send(text) drops its mode argument and always queues. Durable steer content renders as a plain right-aligned bubble (no badge, no user IconActions) so external/host steers stay visible on replay. Delete message.steering locale strings and the unused badge CSS.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-07-31-web-ui-no-steer-entry-or-interjection-chrome.md](../02-notes/archived/simplification/2026-07-31-web-ui-no-steer-entry-or-interjection-chrome.md)
- Pinned source: [.agents/notes/archived/simplification/2026-07-31-web-ui-no-steer-entry-or-interjection-chrome.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-07-31-web-ui-no-steer-entry-or-interjection-chrome.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/host/apiproxy/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/host/apiproxy`. | `named-directory-member` |
| [`packages/host/apiproxy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/host/apiproxy`. | `named-directory-member` |
| [`packages/host/apiproxy/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/host/apiproxy`. | `named-directory-member` |
| [`packages/host/apiproxy/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/host/apiproxy`. | `named-directory-member` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/host/apiproxy`. Defines `queue`, a construct named by the note. | `named-directory-member, symbol-definition` |
| [`packages/client/ui-conversation/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/client/ui-conversation`. Contains the exact code literal `session/queue` named by the note. | `exact-code-occurrence, named-directory-member` |
| [`packages/client/ui-conversation/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/client/ui-conversation`. | `named-directory-member` |
| [`packages/client/ui-conversation/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/client/ui-conversation`. Contains the exact code literal `packages/client/ui-conversation` named by the note. | `exact-code-occurrence, named-directory-member` |
| [`packages/client/ui-conversation/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/client/ui-conversation`. | `named-directory-member` |
| [`packages/client/ui-conversation/src/client/input/machine.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/machine.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client/ui-conversation`. Defines `InputMachine`, a construct named by the note. | `named-directory-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/input/contract.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/contract.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client/ui-conversation`. Defines `SessionInput`, a construct named by the note. | `named-directory-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/queue/QueueDock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/queue/QueueDock.tsx) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client/ui-conversation`. | `named-directory-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `inject` | `const` | [`packages/acp/acp/src/index.ts:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L44) | `export const inject = ['agents']` |
| `send` | `function` | [`packages/client/connection/src/websocket-downlink.ts:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/websocket-downlink.ts#L23) | `function send(socket: WebSocket, frame: RpcRequest<Frame>): Promise<void> {` |
| `queue` | `let` | [`packages/client/hmr/src/client/index.ts:144`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/client/index.ts#L144) | `let queue: Promise<void> = Promise.resolve()` |
| `inject` | `const` | [`packages/client/hmr/src/index.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts#L28) | `export const inject = ['clientModules', 'webServer']` |
| `SessionInput` | `interface` | [`packages/client/ui-conversation/src/client/input/contract.ts:33`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/contract.ts#L33) | `export interface SessionInput extends InputTarget {` |
| `InputMachine` | `class` | [`packages/client/ui-conversation/src/client/input/machine.ts:105`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/machine.ts#L105) | `export class InputMachine {` |
| `submit` | `const` | [`packages/client/ui-conversation/src/client/skeleton/PermissionSelect.tsx:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/PermissionSelect.tsx#L98) | `const submit = (id: string): void => {` |
| `context` | `const` | [`packages/core/agent-loop/src/agent.ts:233`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L233) | `const context = this.runtimeContext.project(joinContextSections(sections), sections)` |
| `inject` | `const` | [`packages/fs/fs/src/invariant.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/invariant.ts#L12) | `export const inject = ['invariants']` |
| `context` | `const` | [`packages/hooks/hooks-codex/src/index.ts:191`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/index.ts#L191) | `const context = contextFrom(merged)` |
| `queue` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:3431`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L3431) | `const queue = new FrameQueue<RpcRequest<MuxFrame>>()` |
| `queue` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:3535`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L3535) | `const queue = new FrameQueue<RpcRequest<HostFrame>>()` |
| `send` | `const` | [`packages/host/directory-picker-native/src/win32-dialog-worker.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-native/src/win32-dialog-worker.ts#L28) | `const send = process.send.bind(process)` |
| `context` | `const` | [`packages/llm/llm-pi-ai/src/adapter.ts:310`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts#L310) | `const context = attachments === undefined` |
| `context` | `const` | [`packages/llm/llm/src/index.ts:648`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L648) | `const context = resolved.context` |
| `context` | `const` | [`packages/llm/llm/src/index.ts:783`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L783) | `const context = resolved.context === undefined` |

### Tests and executable evidence

- [`apps/web/tests/steering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/steering.e2e.ts) — The source note names this file directly. A test under the owning area exercises or imports `steering`.
- [`apps/web/tests/settings-chrome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/settings-chrome.e2e.ts) — A test under the owning area exercises or imports `steer`.
- [`packages/core/agent/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/agent.spec.ts) — A test under the owning area exercises or imports `steer`.
- [`packages/e2b/e2b/tests/composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/tests/composition.e2e.ts) — A test under the owning area exercises or imports `steer`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `steering`. A test under the owning area exercises or imports `steer`.
- [`packages/jobs/jobs-local/tests/jobs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs-local/tests/jobs.spec.ts) — A test under the owning area exercises or imports `steer`.
- [`packages/core/agent-loop/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/agent.spec.ts) — A test under the owning area exercises or imports `steer`.
- [`packages/core/agent-loop/tests/resume.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/resume.spec.ts) — A test under the owning area exercises or imports `steering`.
- Source verification intent: packages/client/ui-conversation unit/jsdom coverage: input machine enter/sink, ConversationService routing, the MessageItem steering arm, InputBar submit. apps/web/tests/steering.e2e.ts keyless replay plus its goldens, which pin the caption. packages/host/apiproxy session/queue projection test asserts user-origin next-step items stay steering while plugin-origin items land as context.

## How to read the implementation

1. Start with [`packages/host/apiproxy/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/README.md) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/simplification`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/projection`
- Aliases: `inject`, `send`, `queue`, `SessionInput`, `InputMachine`, `submit`, `context`, `user/message`, `conversation.send`, `InputActions.submit`, `defaultSink`, `session.prompt`, `ConversationService.send`, `message.steering`
- Regex: `(?i)(inject|send|queue|SessionInput|InputMachine|submit|context|user/message)`

```bash
rg -n --pcre2 "(?i)(inject|send|queue|SessionInput|InputMachine|submit|context|user/message)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0486. Remove the steering interjection caption](0486-remove-the-steering-interjection-caption.md): Shares source implementation: `apps/web/tests/steering.e2e.ts`, `packages/client/ui-conversation/README.md`.
- **`shares-code-with`** — [0375. Bound cold blank-session verification](0375-bound-cold-blank-session-verification.md): Shares source implementation: `packages/host/apiproxy/README.md`, `packages/host/apiproxy/package.json`.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0513. Record last activity in the session index](0513-record-last-activity-in-the-session-index.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0197. Web past-session search](0197-web-past-session-search.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0224. Web result card --- a structured render intent for web_search and web_fetch](0224-web-result-card-a-structured-render-intent-for-web-search-and-web-fetch.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0070. A capability-discriminated directory-picker seam for the web-GUI host](0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0675-web-ui-drops-steer-entry-and-interjection-chrome.md`.
