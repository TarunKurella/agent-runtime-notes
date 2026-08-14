---
id: "dsh-note-0617"
title: "Intent draft echoes in the same tick"
status: "archived"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/bug-fix/2026-07-26-intent-draft-same-tick-echo.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "ConversationRoot"
  - "InputBar"
  - "onChange"
  - "useSessions"
  - "prompt"
  - "EmptyState"
  - "intent.prompt"
  - "SessionManager.updateIntent → Session.updatePendingPrompt"
  - "startIntent"
  - "markDirty"
  - "updateSessionPrompt"
  - "SessionManager.updateIntent"
  - "updateIntent"
  - "this.notifier.notifyNow"
search_regex: "(?i)(ConversationRoot|InputBar|onChange|useSessions|prompt|EmptyState|intent\\.prompt|SessionManager\\.updateIntent[- ]→[- ]Session\\.updatePendingPrompt)"
---

# 0617. Intent draft echoes in the same tick — implementation context

## Open this when

The hero composer ("Let's start building") is a controlled textarea whose value is the frontend Session Intent's retained prompt, read from the sessions list snapshot (EmptyState binds intent.prompt via useSessions). Typing routed through SessionManager.updateIntent → Session.updatePendingPrompt, which flushes the Session's own notifier synchronously --- but the list snapshot the composer actually renders from only heard about the change through the intent watch subscription in startIntent, which calls markDirty(), a microtask-deferred flush.

## Source decision

SessionManager.updateIntent calls this.notifier.notifyNow() after updatePendingPrompt, flushing the list snapshot in the same tick as the change event. This matches the Notifier's channel rule: a direct echo of a user gesture whose controlled input renders from this snapshot uses notifyNow; the intent watch keeps markDirty for every other (async) intent transition.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/bug-fix/2026-07-26-intent-draft-same-tick-echo.md](../02-notes/archived/bug-fix/2026-07-26-intent-draft-same-tick-echo.md)
- Pinned source: [.agents/notes/archived/bug-fix/2026-07-26-intent-draft-same-tick-echo.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/bug-fix/2026-07-26-intent-draft-same-tick-echo.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `onChange`, a construct named by the note. | `symbol-definition` |
| [`packages/client/web/src/app.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/app.tsx) | runtime implementation | Defines `useSessions`, a construct named by the note. | `symbol-definition` |
| [`packages/schedule/schedule/src/domain.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/domain.ts) | runtime implementation | Defines `prompt`, a construct named by the note. | `symbol-definition` |
| [`packages/workflow/tool-ralph/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts) | package entry point | Defines `prompt`, a construct named by the note. | `symbol-definition` |
| [`packages/subagent/subagent-acp/src/run.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/src/run.ts) | runtime implementation | Defines `prompt`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-theme/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/client/index.ts) | package entry point | Defines `onChange`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx) | runtime implementation | Defines `onChange`, a construct named by the note. Defines `InputBar`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx) | runtime implementation | Defines `ConversationRoot`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ConversationRoot` | `function` | [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx#L15) | `export function ConversationRoot({` |
| `InputBar` | `function` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L46) | `export function InputBar({` |
| `onChange` | `const` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:342`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L342) | `const onChange = (e: ChangeEvent<HTMLTextAreaElement>): void => {` |
| `onChange` | `const` | [`packages/client/ui-theme/src/client/index.ts:176`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/client/index.ts#L176) | `const onChange = (): void => {` |
| `useSessions` | `const` | [`packages/client/web/src/app.tsx:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/app.tsx#L30) | `const useSessions = bindSnapshotSelector(sessions.list)` |
| `prompt` | `const` | [`packages/schedule/schedule/src/domain.ts:389`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/domain.ts#L389) | `const prompt = value['prompt']` |
| `prompt` | `const` | [`packages/schedule/schedule/src/domain.ts:411`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/domain.ts#L411) | `const prompt = value['prompt']` |
| `prompt` | `const` | [`packages/schedule/schedule/src/domain.ts:429`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/domain.ts#L429) | `const prompt = value['prompt']` |
| `prompt` | `const` | [`packages/subagent/subagent-acp/src/run.ts:327`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/src/run.ts#L327) | `const prompt = async (): Promise<SubagentResult> => {` |
| `prompt` | `const` | [`packages/workflow/tool-ralph/src/index.ts:155`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts#L155) | `const prompt = [` |
| `onChange` | `const` | [`vendor/hmr/src/index.ts:151`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L151) | `const onChange = (path: string) => {` |
| `onChange` | `const` | [`vendor/hmr/src/index.ts:244`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L244) | `const onChange = (kind: 'add' \| 'change' \| 'unlink', path: string) => {` |

### Tests and executable evidence

- [`packages/client/runtime/tests/notifier.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/notifier.client.spec.ts) — A test under the owning area exercises or imports `markDirty`. A test under the owning area exercises or imports `notifyNow`.
- [`packages/client/ui-conversation/tests/skeleton.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/skeleton.client.spec.tsx) — A test under the owning area exercises or imports `ConversationRoot`. A test under the owning area exercises or imports `InputBar`.
- [`packages/client/ui-conversation/tests/input-bar.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-bar.client.spec.tsx) — A test under the owning area exercises or imports `InputBar`.
- [`packages/client/ui-conversation/tests/input-matrix.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-matrix.client.spec.tsx) — A test under the owning area exercises or imports `InputBar`.
- [`packages/client/ui-conversation/tests/input-scenarios.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-scenarios.client.spec.tsx) — A test under the owning area exercises or imports `InputBar`.
- [`packages/client/ui-conversation/tests/views-type-chain.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/views-type-chain.client.spec.tsx) — A test under the owning area exercises or imports `ConversationRoot`.

## How to read the implementation

1. Start with [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/archived`, `mechanism/policy`, `mechanism/projection`
- Aliases: `ConversationRoot`, `InputBar`, `onChange`, `useSessions`, `prompt`, `EmptyState`, `intent.prompt`, `SessionManager.updateIntent → Session.updatePendingPrompt`, `startIntent`, `markDirty`, `updateSessionPrompt`, `SessionManager.updateIntent`, `updateIntent`, `this.notifier.notifyNow`
- Regex: `(?i)(ConversationRoot|InputBar|onChange|useSessions|prompt|EmptyState|intent\.prompt|SessionManager\.updateIntent[- ]→[- ]Session\.updatePendingPrompt)`

```bash
rg -n --pcre2 "(?i)(ConversationRoot|InputBar|onChange|useSessions|prompt|EmptyState|intent\\.prompt|SessionManager\\.updateIntent[- ]\u2192[- ]Session\\.updatePendingPrompt)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): Shares source implementation: `packages/client/web/src/app.tsx`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0611. Keep the Code Mode result card complete](0611-keep-the-code-mode-result-card-complete.md): Shares source implementation: `packages/workflow/tool-ralph/src/index.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0583. Docked web goal bar](0583-docked-web-goal-bar.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/client/web/src/app.tsx`.
- **`shares-code-with`** — [0594. Fixed `Tool / <name>` header for tool-call cards](0594-fixed-tool-name-header-for-tool-call-cards.md): Shares source implementation: `packages/client/web/src/app.tsx`, `packages/workflow/tool-ralph/src/index.ts`.
- **`shares-code-with`** — [0239. Goal command input projection](0239-goal-command-input-projection.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/client/ui-conversation/tests/skeleton.client.spec.tsx`.
- **`shares-code-with`** — [0320. The approval takeover shares the composer's text cap](0320-the-approval-takeover-shares-the-composer-s-text-cap.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/client/ui-conversation/src/client/skeleton/InputBar.tsx`.
- **`shares-code-with`** — [0318. Fixed header, sticky composer inside the transcript scrollport](0318-fixed-header-sticky-composer-inside-the-transcript-scrollport.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/client/ui-conversation/tests/skeleton.client.spec.tsx`.
- **`shares-code-with`** — [0275. The no-Workspace composer opens the existing picker](0275-the-no-workspace-composer-opens-the-existing-picker.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/client/ui-conversation/tests/skeleton.client.spec.tsx`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0617-intent-draft-echoes-in-the-same-tick.md`.
