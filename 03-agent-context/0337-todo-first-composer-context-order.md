---
id: "dsh-note-0337"
title: "Todo-first composer context order"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-02-todo-first-composer-context-order.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/registry"
aliases:
  - "ConversationRoot"
  - "order"
  - "conversation.input.dock"
  - ", Goal at"
  - ", and Queue at"
  - "; Queue remains pinned at"
  - ", Goal"
  - ", and Queue"
  - "Todo-first composer context order"
  - "bug fix"
  - "boundary"
  - "concurrency"
  - "evidence"
  - "ownership"
search_regex: "(?i)(ConversationRoot|order|conversation\\.input\\.dock|,[- ]Goal[- ]at|,[- ]and[- ]Queue[- ]at|;[- ]Queue[- ]remains[- ]pinned[- ]at|,[- ]Goal|,[- ]and[- ]Queue)"
---

# 0337. Todo-first composer context order — implementation context

## Open this when

The composer context stack rendered Goal before Todo even though the Harness design orders the current task plan before its ongoing goal and pending Queue. Todo also used the Queue wrapper's 776px width as its visible card width, while Goal and the Queue panel rendered on the shared 752px card column. The result inverted the intended information hierarchy and left Todo wider than both adjacent panels.

## Source decision

The conversation.input.dock list uses one ascending product order: Todo at 0, Goal at 10, and Queue at 20, followed by the composer bar outside the list. Registration order remains the semantic source of truth; the renderer does not hardcode known component ids or repair their order with CSS. Todo, Goal, and the visible Queue panel share the 752px card column inside the 800px composer cap. Queue retains a 776px wrapper with 12px transparent inset on each side because that wrapper owns the composer overlap.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-02-todo-first-composer-context-order.md](../02-notes/implemented/bug-fix/2026-08-02-todo-first-composer-context-order.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-02-todo-first-composer-context-order.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-02-todo-first-composer-context-order.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) | package entry point | Defines `order`, a construct named by the note. | `symbol-definition` |
| [`packages/workspace/workspace/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts) | package entry point | Defines `order`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-deepseek/src/translate.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/translate.ts) | runtime implementation | Defines `order`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-title/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts) | package entry point | Defines `order`, a construct named by the note. | `symbol-definition` |
| [`packages/preset/agent-presets/src/metadata.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/metadata.ts) | runtime implementation | Defines `order`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx) | runtime implementation | Defines `ConversationRoot`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ConversationRoot` | `function` | [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx#L15) | `export function ConversationRoot({` |
| `order` | `const` | [`packages/llm/llm-deepseek/src/translate.ts:91`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/translate.ts#L91) | `const order: OpenBlock[] = []` |
| `order` | `const` | [`packages/preset/agent-presets/src/metadata.ts:77`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/metadata.ts#L77) | `const order = typeof record.order === 'number' && Number.isFinite(record.order)` |
| `order` | `const` | [`packages/session/session-title/src/index.ts:601`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts#L601) | `const order = new Map(messages.map((message, index) => [message.seq, index]))` |
| `order` | `const` | [`packages/skill/skill/src/index.ts:410`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L410) | `const order = this.nextProviderOrder` |
| `order` | `const` | [`packages/workspace/workspace/src/index.ts:512`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts#L512) | `const order = new Set<WorkspaceId>()` |

### Tests and executable evidence

- [`vitest.e2e.config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vitest.e2e.config.ts) — A test under the owning area exercises or imports `and`.
- [`apps/web/tests/steering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/steering.e2e.ts) — A test under the owning area exercises or imports `dock`.
- [`scripts/translation-brief.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-brief.spec.ts) — A test under the owning area exercises or imports `and`.
- [`scripts/gen-client-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-client-catalog.spec.ts) — A test under the owning area exercises or imports `and`.
- [`apps/web/tests/todo-row.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/todo-row.snapshot.ts) — A test under the owning area exercises or imports `dock`.
- [`scripts/translation-pairing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing.spec.ts) — A test under the owning area exercises or imports `and`.
- [`scripts/verify-cordis-config.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-cordis-config.spec.ts) — A test under the owning area exercises or imports `and`.
- [`packages/client/ui-goal/tests/browser-plugin.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-goal/tests/browser-plugin.client.spec.tsx) — A test under the owning area exercises or imports `dock`.
- Source verification intent: Todo and Goal registration tests pin orders 0 and 10; Queue remains pinned at 20. The keyless Queue browser scenario renders all three panels concurrently, records their Todo--Goal--Queue accessibility order, and compares their visible bounding boxes at the 1680px desktop baseline and a 640px sub-cap viewport before exercising Queue mutations.

## How to read the implementation

1. Start with [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/registry`
- Aliases: `ConversationRoot`, `order`, `conversation.input.dock`, `, Goal at`, `, and Queue at`, `; Queue remains pinned at`, `, Goal`, `, and Queue`, `Todo-first composer context order`, `bug fix`, `boundary`, `concurrency`, `evidence`, `ownership`
- Regex: `(?i)(ConversationRoot|order|conversation\.input\.dock|,[- ]Goal[- ]at|,[- ]and[- ]Queue[- ]at|;[- ]Queue[- ]remains[- ]pinned[- ]at|,[- ]Goal|,[- ]and[- ]Queue)`

```bash
rg -n --pcre2 "(?i)(ConversationRoot|order|conversation\\.input\\.dock|,[- ]Goal[- ]at|,[- ]and[- ]Queue[- ]at|;[- ]Queue[- ]remains[- ]pinned[- ]at|,[- ]Goal|,[- ]and[- ]Queue)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0322. Composer context stack order](0322-composer-context-stack-order.md): The source note links to this decision directly.
- **`shares-code-with`** — [0585. TUI file-reference autocomplete](0585-tui-file-reference-autocomplete.md): Shares source implementation: `scripts/gen-client-catalog.spec.ts`, `scripts/translation-brief.spec.ts`.
- **`shares-code-with`** — [0297. Workspace Sidebar Order and Folding](0297-workspace-sidebar-order-and-folding.md): Shares source implementation: `packages/workspace/workspace/src/index.ts`, `scripts/translation-brief.spec.ts`.
- **`shares-code-with`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): Shares source implementation: `packages/skill/skill/src/index.ts`.
- **`shares-code-with`** — [0366. Preset cards clamp their description instead of sizing the roster](0366-preset-cards-clamp-their-description-instead-of-sizing-the-roster.md): Shares source implementation: `packages/session/session-title/src/index.ts`, `packages/skill/skill/src/index.ts`.
- **`shares-code-with`** — [0320. The approval takeover shares the composer's text cap](0320-the-approval-takeover-shares-the-composer-s-text-cap.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`.
- **`shares-code-with`** — [0219. Steer a queued Web message into the active turn](0219-steer-a-queued-web-message-into-the-active-turn.md): Shares source implementation: `apps/web/tests/steering.e2e.ts`.
- **`shares-code-with`** — [0192. Skill catalog hot refresh](0192-skill-catalog-hot-refresh.md): Shares source implementation: `packages/skill/skill/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0337-todo-first-composer-context-order.md`.
