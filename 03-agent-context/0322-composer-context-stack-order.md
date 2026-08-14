---
id: "dsh-note-0322"
title: "Composer context stack order"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-30-composer-context-stack-order.md"
implementation_evidence: "medium"
target_anchor: "exec, terminal, and process lifecycle"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/ownership"
  - "domain/extensions"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/registry"
aliases:
  - "ConversationRoot"
  - "conversation.input.dock"
  - ", Goal"
  - ", and Queue"
  - "Composer context stack order"
  - "bug fix"
  - "boundary"
  - "concurrency"
  - "evidence"
  - "ownership"
  - "extensions"
  - "shell terminal"
  - "testing"
  - "ui interaction"
search_regex: "(?i)(ConversationRoot|conversation\\.input\\.dock|,[- ]Goal|,[- ]and[- ]Queue|Composer[- ]context[- ]stack[- ]order|bug[- ]fix|boundary|concurrency)"
---

# 0322. Composer context stack order — implementation context

## Open this when

Goal, Todo, and Queue contribute independently to the same conversation.input.dock list, but their registration order and spacing rules did not encode the composition matrix. The renderer therefore placed Todo before Queue and Goal, while both Queue and Goal carried negative margins intended for the composer boundary. When all three were present, Queue joined to Goal and Goal joined to the composer, reversing the design's hierarchy.

## Source decision

The Todo-first alignment decision owns the current ascending order. This note retains the stack contract around that order: numeric gaps leave room for future entries to declare their intended position without relying on plugin activation order, and the composer bar follows the list. ConversationRoot owns the 6px space between independent context cards. Goal is a standalone 752×36px card and collapsed Todo is a standalone 752×44px card.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-30-composer-context-stack-order.md](../02-notes/implemented/bug-fix/2026-07-30-composer-context-stack-order.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-30-composer-context-stack-order.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-30-composer-context-stack-order.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx) | runtime implementation | Defines `ConversationRoot`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ConversationRoot` | `function` | [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx#L15) | `export function ConversationRoot({` |

### Tests and executable evidence

- [`apps/web/tests/steering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/steering.e2e.ts) — A test under the owning area exercises or imports `dock`.
- [`apps/web/tests/todo-row.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/todo-row.snapshot.ts) — A test under the owning area exercises or imports `dock`.
- [`packages/client/ui-goal/tests/browser-plugin.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-goal/tests/browser-plugin.client.spec.tsx) — A test under the owning area exercises or imports `dock`.
- [`packages/client/ui-conversation/tests/skeleton.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/skeleton.client.spec.tsx) — A test under the owning area exercises or imports `ConversationRoot`.
- [`packages/client/ui-tool/tests/assembly-surfaces.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/assembly-surfaces.client.spec.tsx) — A test under the owning area exercises or imports `dock`.
- [`packages/client/ui-conversation/tests/input-bar.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-bar.client.spec.tsx) — A test under the owning area exercises or imports `dock`.
- [`packages/client/ui-conversation/tests/queue-dock.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/queue-dock.client.spec.tsx) — A test under the owning area exercises or imports `dock`.
- [`packages/client/ui-conversation/tests/chat-stats.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-stats.client.spec.tsx) — A test under the owning area exercises or imports `dock`.
- Source verification intent: Registration tests pin all three order values. The keyless Queue browser scenario renders Todo, Goal, and Queue together, pins their accessibility order, and checks their visible card edges; focused Goal and Queue scenarios cover their independent states.

## How to read the implementation

1. Start with [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** exec, terminal, and process lifecycle.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/ownership`, `domain/extensions`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/registry`
- Aliases: `ConversationRoot`, `conversation.input.dock`, `, Goal`, `, and Queue`, `Composer context stack order`, `bug fix`, `boundary`, `concurrency`, `evidence`, `ownership`, `extensions`, `shell terminal`, `testing`, `ui interaction`
- Regex: `(?i)(ConversationRoot|conversation\.input\.dock|,[- ]Goal|,[- ]and[- ]Queue|Composer[- ]context[- ]stack[- ]order|bug[- ]fix|boundary|concurrency)`

```bash
rg -n --pcre2 "(?i)(ConversationRoot|conversation\\.input\\.dock|,[- ]Goal|,[- ]and[- ]Queue|Composer[- ]context[- ]stack[- ]order|bug[- ]fix|boundary|concurrency)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0337. Todo-first composer context order](0337-todo-first-composer-context-order.md): The source note links to this decision directly.
- **`shares-code-with`** — [0239. Goal command input projection](0239-goal-command-input-projection.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/client/ui-conversation/tests/skeleton.client.spec.tsx`.
- **`shares-code-with`** — [0318. Fixed header, sticky composer inside the transcript scrollport](0318-fixed-header-sticky-composer-inside-the-transcript-scrollport.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/client/ui-conversation/tests/skeleton.client.spec.tsx`.
- **`shares-code-with`** — [0275. The no-Workspace composer opens the existing picker](0275-the-no-workspace-composer-opens-the-existing-picker.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/client/ui-conversation/tests/skeleton.client.spec.tsx`.
- **`shares-code-with`** — [0605. Web composer stats detail and input-zone polish](0605-web-composer-stats-detail-and-input-zone-polish.md): Shares source implementation: `apps/web/tests/steering.e2e.ts`, `apps/web/tests/todo-row.snapshot.ts`.
- **`shares-code-with`** — [0219. Steer a queued Web message into the active turn](0219-steer-a-queued-web-message-into-the-active-turn.md): Shares source implementation: `apps/web/tests/steering.e2e.ts`.
- **`shares-code-with`** — [0190. Allow several `in_progress` todos at once](0190-allow-several-in-progress-todos-at-once.md): Shares source implementation: `apps/web/tests/todo-row.snapshot.ts`.
- **`shares-code-with`** — [0598. Address pending queue occurrences for edit and removal](0598-address-pending-queue-occurrences-for-edit-and-removal.md): Shares source implementation: `apps/web/tests/todo-row.snapshot.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0322-composer-context-stack-order.md`.
