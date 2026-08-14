---
id: "dsh-note-0270"
title: "Steer the whole Web queue with an empty-draft Cmd/Ctrl+Enter"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-06-web-queue-steer-all-gesture.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/configuration"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "running"
  - "onKeyDown"
  - "queued"
  - "submit"
  - "InputBar.onKeyDown"
  - "ComposerKeyboard.steerQueue"
  - "SessionInputShell.steerQueue"
  - "session/queue"
  - "session.updateQueue"
  - "steer-unavailable"
  - "queue-item-not-found"
  - "session.prompt"
  - "updateQueue"
  - "Steer the whole Web queue with an empty-draft Cmd/Ctrl+Enter"
search_regex: "(?i)(running|onKeyDown|queued|submit|InputBar\\.onKeyDown|ComposerKeyboard\\.steerQueue|SessionInputShell\\.steerQueue|session/queue)"
---

# 0270. Steer the whole Web queue with an empty-draft Cmd/Ctrl+Enter — implementation context

## Open this when

While a primary session runs, the Web queue accumulates messages the user typed with plain Enter (or queued while the busy-Enter preference was Queue). Flushing them into the current turn required clicking the per-row 插话发送 button once per message; an empty composer draft had no keyboard gesture at all --- the input machine rejects empty drafts, so Enter and Cmd/Ctrl+Enter were both no-ops. With several queued messages, steering them one by one is the obvious multi-click friction, and the empty-draft accelerated chord is the natural slot for "steer everything".

## Source decision

Empty-draft Cmd/Ctrl+Enter now steers every still-pending queued-placement inbox row into the running turn, in FIFO order, on a primary session that reports running. The gesture decodes in InputBar.onKeyDown: accelerated Enter with a trimmed-empty draft, running, no subagent address, and at least one queued row calls the new ComposerKeyboard.steerQueue() verb instead of submit().

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-06-web-queue-steer-all-gesture.md](../02-notes/implemented/feature/2026-08-06-web-queue-steer-all-gesture.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-06-web-queue-steer-all-gesture.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-06-web-queue-steer-all-gesture.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/sdk/client/src/client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/client.ts) | runtime implementation | Defines `queued`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/bash-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts) | package entry point | Defines `running`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/pwsh-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/index.ts) | package entry point | Defines `running`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `queued`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/Menu.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Menu.tsx) | runtime implementation | Defines `onKeyDown`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/Modal.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Modal.tsx) | runtime implementation | Defines `onKeyDown`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `running`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-attachment/src/ImageLightbox.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/ImageLightbox.tsx) | runtime implementation | Defines `onKeyDown`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-jobs/src/client/JobListAction.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-jobs/src/client/JobListAction.tsx) | runtime implementation | Defines `onKeyDown`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/input/hub.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/hub.ts) | runtime implementation | Defines `queued`, a construct named by the note. | `symbol-definition` |
| [`packages/extensions/ui-cordis/src/client/CordisPanel.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/CordisPanel.tsx) | runtime implementation | Defines `running`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-commands/src/client/PopupSelectView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/PopupSelectView.tsx) | runtime implementation | Defines `onKeyDown`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `running` | `const` | [`packages/client/connection/src/client/fixture.ts:1792`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1792) | `const running = summaryOf(id)?.running === true` |
| `onKeyDown` | `const` | [`packages/client/ui-attachment/src/ImageLightbox.tsx:39`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/ImageLightbox.tsx#L39) | `const onKeyDown = (event: globalThis.KeyboardEvent): void => {` |
| `onKeyDown` | `const` | [`packages/client/ui-commands/src/client/PopupSelectView.tsx:82`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/PopupSelectView.tsx#L82) | `const onKeyDown = (ev: React.KeyboardEvent<HTMLDivElement>): void => {` |
| `running` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L156) | `const running = useSession(s => s.running)` |
| `queued` | `const` | [`packages/client/ui-conversation/src/client/input/hub.ts:183`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/hub.ts#L183) | `const queued = session.getSnapshot().queue.filter(item => item.placement === 'queued')` |
| `submit` | `const` | [`packages/client/ui-conversation/src/client/skeleton/PermissionSelect.tsx:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/PermissionSelect.tsx#L98) | `const submit = (id: string): void => {` |
| `onKeyDown` | `const` | [`packages/client/ui-jobs/src/client/JobListAction.tsx:136`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-jobs/src/client/JobListAction.tsx#L136) | `const onKeyDown = (event: KeyboardEvent<HTMLDivElement>): void => {` |
| `onKeyDown` | `const` | [`packages/client/ui-primitives/src/Menu.tsx:179`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Menu.tsx#L179) | `const onKeyDown = (e: KeyboardEvent) => {` |
| `onKeyDown` | `const` | [`packages/client/ui-primitives/src/Modal.tsx:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Modal.tsx#L46) | `const onKeyDown = (e: KeyboardEvent) => {` |
| `running` | `const` | [`packages/extensions/ui-cordis/src/client/CordisPanel.tsx:154`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/CordisPanel.tsx#L154) | `const running = all.filter(view => visiblePanelStatus(` |
| `queued` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:3125`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L3125) | `const queued = presetSwitches.get(sessionId) ?? Promise.resolve()` |
| `queued` | `const` | [`packages/sdk/client/src/client.ts:108`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/client.ts#L108) | `const queued = this.state.queue.shift()` |
| `running` | `const` | [`packages/shell/bash-local/src/index.ts:257`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts#L257) | `const running = this.ctx.subprocess.spawn(this.spawnSpec(spec, argv, this.config.maxOutputBytes, spec.signal))` |
| `running` | `const` | [`packages/shell/pwsh-local/src/index.ts:286`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/index.ts#L286) | `const running = this.ctx.subprocess.spawn(this.spawnSpec(spec, this.config.maxOutputBytes, spec.signal, argv))` |

### Tests and executable evidence

- [`apps/web/tests/steering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/steering.e2e.ts) — A test under the owning area exercises or imports `steer-unavailable`. Contains the exact code literal `session/queue` named by the note.
- [`packages/host/apiproxy/tests/rpc-schemas.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/rpc-schemas.spec.ts) — A test under the owning area exercises or imports `queue-item-not-found`.
- [`packages/client/runtime/tests/fake-api.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/fake-api.client.ts) — A test under the owning area exercises or imports `updateQueue`.
- [`packages/host/apiproxy/tests/fetch-carrier.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/fetch-carrier.spec.ts) — A test under the owning area exercises or imports `updateQueue`.
- [`packages/client/connection/tests/fake-api.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/tests/fake-api.client.ts) — A test under the owning area exercises or imports `updateQueue`.
- [`packages/host/apiproxy/tests/client-handler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/client-handler.spec.ts) — A test under the owning area exercises or imports `updateQueue`.
- [`packages/host/apiproxy/tests/api-proxy-cold.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-cold.spec.ts) — A test under the owning area exercises or imports `updateQueue`.
- [`packages/client/runtime/tests/queue-store.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/queue-store.client.spec.ts) — A test under the owning area exercises or imports `updateQueue`. Contains the exact code literal `session/queue` named by the note.

## How to read the implementation

1. Start with [`packages/sdk/client/src/client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/client.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `domain/configuration`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/storage`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`
- Aliases: `running`, `onKeyDown`, `queued`, `submit`, `InputBar.onKeyDown`, `ComposerKeyboard.steerQueue`, `SessionInputShell.steerQueue`, `session/queue`, `session.updateQueue`, `steer-unavailable`, `queue-item-not-found`, `session.prompt`, `updateQueue`, `Steer the whole Web queue with an empty-draft Cmd/Ctrl+Enter`
- Regex: `(?i)(running|onKeyDown|queued|submit|InputBar\.onKeyDown|ComposerKeyboard\.steerQueue|SessionInputShell\.steerQueue|session/queue)`

```bash
rg -n --pcre2 "(?i)(running|onKeyDown|queued|submit|InputBar\\.onKeyDown|ComposerKeyboard\\.steerQueue|SessionInputShell\\.steerQueue|session/queue)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0219. Steer a queued Web message into the active turn](0219-steer-a-queued-web-message-into-the-active-turn.md): The source note links to this decision directly.
- **`shares-code-with`** — [0348. `list_agents` uses `ready` for resumable children](0348-list-agents-uses-ready-for-resumable-children.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/shell/bash-local/src/index.ts`.
- **`shares-code-with`** — [0183. Session List Browsing and Manual Workspace Order](0183-session-list-browsing-and-manual-workspace-order.md): Shares source implementation: `packages/client/ui-primitives/src/Menu.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0199. Workspace Registration Deletion](0199-workspace-registration-deletion.md): Shares source implementation: `packages/client/ui-primitives/src/Menu.tsx`, `packages/client/ui-primitives/src/Modal.tsx`.
- **`shares-code-with`** — [0461. Collapse agent-loop events around the observable state machine](0461-collapse-agent-loop-events-around-the-observable-state-machine.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/shell/bash-local/src/index.ts`.
- **`shares-code-with`** — [0336. Message fork actions require a completed turn tail](0336-message-fork-actions-require-a-completed-turn-tail.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/shell/bash-local/src/index.ts`.
- **`shares-code-with`** — [0344. Turn-tail IconActions require a completed turn](0344-turn-tail-iconactions-require-a-completed-turn.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/shell/bash-local/src/index.ts`.
- **`shares-code-with`** — [0591. Code Mode sub-calls in the trajectory and waterfall views](0591-code-mode-sub-calls-in-the-trajectory-and-waterfall-views.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/shell/bash-local/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0270-steer-the-whole-web-queue-with-an-empty-draft-cmd-ctrl-enter.md`.
