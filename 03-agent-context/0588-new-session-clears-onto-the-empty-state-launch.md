---
id: "dsh-note-0588"
title: "New Session clears onto the empty-state launch"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-24-new-session-clears-to-empty-state.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/ui-interaction"
  - "lifecycle/archived"
aliases:
  - "ConversationRoot"
  - "InputBar"
  - "empty"
  - "AppFrame"
  - "clear"
  - "current"
  - "EmptyState"
  - "sessions.current"
  - "SessionsService.clear"
  - "list.current"
  - "onCreate"
  - "conversation.empty"
  - "conversation.startSession"
  - "startSession"
search_regex: "(?i)(ConversationRoot|InputBar|empty|AppFrame|clear|current|EmptyState|sessions\\.current)"
---

# 0588. New Session clears onto the empty-state launch — implementation context

## Open this when

Sidebar "New Session" created and opened a blank session immediately, so the center column showed ConversationRoot with an empty transcript and the resident composer. The Figma NEW SESSION screen (EmptyState + shared InputBar hero) only rendered when sessions.current was already undefined, so the launch page was unreachable from the primary creation control.

## Source decision

SessionsService.clear() wipes the persisted selection and list.current. Top-level sidebar creation entries (onCreate() with no cwd --- New Session and New Workspace) call clear() so AppFrame renders conversation.empty. The empty state's first send still runs conversation.startSession (create → open → send) and reuses the same InputBar component as the resident composer (variant="hero"). Per-project "+" (onCreate(cwd)) keeps create-then-open until the empty-state picker can accept a seeded cwd.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-24-new-session-clears-to-empty-state.md](../02-notes/archived/feature/2026-07-24-new-session-clears-to-empty-state.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-24-new-session-clears-to-empty-state.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-24-new-session-clears-to-empty-state.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `current`, a construct named by the note. | `symbol-definition` |
| [`vendor/schemastery/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/schemastery/src/index.ts) | package entry point | Defines `current`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts) | package entry point | Defines `current`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/ansi.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ansi.ts) | runtime implementation | Defines `clear`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/WebBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/WebBlock.tsx) | runtime implementation | Defines `empty`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-layout/src/client/AppFrame.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/AppFrame.tsx) | runtime implementation | Defines `AppFrame`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/SearchBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/SearchBlock.tsx) | runtime implementation | Defines `empty`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/TerminalBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/TerminalBlock.tsx) | runtime implementation | Defines `empty`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx) | runtime implementation | Defines `InputBar`, a construct named by the note. Defines `empty`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx) | runtime implementation | Defines `ConversationRoot`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ConversationRoot` | `function` | [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx#L15) | `export function ConversationRoot({` |
| `InputBar` | `function` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L46) | `export function InputBar({` |
| `empty` | `const` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L75) | `const empty = draft.trim() === '' && attachments.length === 0` |
| `AppFrame` | `function` | [`packages/client/ui-layout/src/client/AppFrame.tsx:87`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/AppFrame.tsx#L87) | `export function AppFrame({` |
| `empty` | `const` | [`packages/client/ui-primitives/src/SearchBlock.tsx:182`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/SearchBlock.tsx#L182) | `const empty = rows.length === 0` |
| `empty` | `const` | [`packages/client/ui-primitives/src/TerminalBlock.tsx:226`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/TerminalBlock.tsx#L226) | `const empty = lines.every(line => line.every(span => span.text.trim() === ''))` |
| `empty` | `const` | [`packages/client/ui-primitives/src/WebBlock.tsx:160`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/WebBlock.tsx#L160) | `const empty = (answer === undefined \|\| answer === '') && sources.length === 0` |
| `clear` | `const` | [`packages/client/ui-primitives/src/ansi.ts:256`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ansi.ts#L256) | `const clear = (index: number, fill: string): void => {` |
| `current` | `const` | [`packages/goal/goal/src/fold.ts:274`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L274) | `const current = state.goal` |
| `current` | `const` | [`packages/goal/goal/src/fold.ts:297`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L297) | `const current = state.goal` |
| `current` | `const` | [`packages/goal/goal/src/fold.ts:324`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L324) | `const current = state.goal` |
| `current` | `const` | [`packages/goal/goal/src/index.ts:254`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L254) | `const current = cache.state.goal` |
| `current` | `let` | [`vendor/schemastery/src/index.ts:476`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/schemastery/src/index.ts#L476) | `let current = schema` |

### Tests and executable evidence

- [`scripts/verify-dsh-package-licenses.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-dsh-package-licenses.spec.ts) — A test under the owning area exercises or imports `createWorkspace`.
- [`packages/client/runtime/tests/fake-api.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/fake-api.client.ts) — A test under the owning area exercises or imports `onCreate`.
- [`packages/client/connection/tests/fake-api.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/tests/fake-api.client.ts) — A test under the owning area exercises or imports `onCreate`.
- [`packages/client/runtime/tests/manager.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/manager.client.spec.ts) — A test under the owning area exercises or imports `onCreate`.
- [`packages/client/ui-layout/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `AppFrame`.
- [`packages/client/ui-sidebar/tests/apply.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-sidebar/tests/apply.client.spec.tsx) — A test under the owning area exercises or imports `startSession`.
- [`packages/client/ui-workspace/tests/rows.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/tests/rows.client.spec.tsx) — A test under the owning area exercises or imports `onCreate`.
- [`packages/client/ui-workspace/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `startSession`. A test under the owning area exercises or imports `createWorkspace`.

## How to read the implementation

1. Start with [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/discovery-routing`, `concern/evidence`, `domain/build-release`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/ui-interaction`, `lifecycle/archived`
- Aliases: `ConversationRoot`, `InputBar`, `empty`, `AppFrame`, `clear`, `current`, `EmptyState`, `sessions.current`, `SessionsService.clear`, `list.current`, `onCreate`, `conversation.empty`, `conversation.startSession`, `startSession`
- Regex: `(?i)(ConversationRoot|InputBar|empty|AppFrame|clear|current|EmptyState|sessions\.current)`

```bash
rg -n --pcre2 "(?i)(ConversationRoot|InputBar|empty|AppFrame|clear|current|EmptyState|sessions\\.current)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0454. Simplify session-log representation](0454-simplify-session-log-representation.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/goal/goal/src/index.ts`.
- **`shares-code-with`** — [0220. Web read card frontend --- the read tool's line window renders line-numbered and highlighted](0220-web-read-card-frontend-the-read-tool-s-line-window-renders-line-numbered.md): Shares source implementation: `packages/client/ui-primitives/src/TerminalBlock.tsx`, `packages/client/ui-primitives/src/ansi.ts`.
- **`shares-code-with`** — [0196. Web session fork actions](0196-web-session-fork-actions.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/goal/goal/src/index.ts`.
- **`shares-code-with`** — [0320. The approval takeover shares the composer's text cap](0320-the-approval-takeover-shares-the-composer-s-text-cap.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/client/ui-conversation/src/client/skeleton/InputBar.tsx`.
- **`shares-code-with`** — [0183. Session List Browsing and Manual Workspace Order](0183-session-list-browsing-and-manual-workspace-order.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/goal/goal/src/index.ts`.
- **`shares-code-with`** — [0583. Docked web goal bar](0583-docked-web-goal-bar.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/goal/goal/src/index.ts`.
- **`shares-code-with`** — [0617. Intent draft echoes in the same tick](0617-intent-draft-echoes-in-the-same-tick.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/client/ui-conversation/src/client/skeleton/InputBar.tsx`.
- **`shares-code-with`** — [0223. Web result card frontend --- rendering the web render intent in the browser](0223-web-result-card-frontend-rendering-the-web-render-intent-in-the-browser.md): Shares source implementation: `packages/client/ui-primitives/src/WebBlock.tsx`, `packages/client/ui-primitives/src/ansi.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0588-new-session-clears-onto-the-empty-state-launch.md`.
