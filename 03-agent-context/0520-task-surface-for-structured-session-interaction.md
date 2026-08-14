---
id: "dsh-note-0520"
title: "Task Surface for structured session interaction"
status: "proposed"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/feature/2026-08-04-task-surface.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/proposed"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "stack"
  - "ToolResultNode"
  - "composer"
  - "messageId"
  - "source"
  - "queued"
  - "submit"
  - "code"
  - "MarkdownText"
  - "markdown"
  - "SurfaceManager"
  - "SurfaceOp"
  - "ToolDefinition"
  - "view"
search_regex: "(?i)(stack|ToolResultNode|composer|messageId|source|queued|submit|code)"
---

# 0520. Task Surface for structured session interaction — implementation context

## Open this when

Some tasks are awkward to finish through alternating prose messages. Comparing several options, reordering a plan, reviewing a table, or filling a small set of related fields all work better as one structured interaction. Today an agent can describe such an interaction, but it cannot ask the Web client to render one without adding a permanent product component or generating executable Client Plugin code. Those two workarounds put ownership in the wrong place. Product-specific components require a new trigger and release for every task shape.

## Source decision

Add Task Surface, a versioned declarative model rendered by a normal Web Client Plugin. One stable model-facing tool, show_task_surface, publishes the model. A successful call ends the current turn. The user edits and submits the rendered panel; the Host records the submission as one ordinary visible user message and starts the next turn. Task Surface is the default structured-UI path when all of the following hold: the interaction belongs to the current Session and current task; its behavior fits the declared component set; it needs no background execution or new runtime authority; and the useful durable.

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/feature/2026-08-04-task-surface.md](../02-notes/proposed/feature/2026-08-04-task-surface.md)
- Pinned source: [.agents/notes/proposed/feature/2026-08-04-task-surface.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/feature/2026-08-04-task-surface.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/util/brand/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/brand/src/index.ts) | package entry point | Core file in the package named by the note: `packages/util/brand`. Defines `Branded`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/util/brand/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/brand/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/util/brand`. | `named-package-member` |
| [`packages/client/ui-slots/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-slots`. | `named-package-member` |
| [`packages/client/ui-primitives/src/ansi.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ansi.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client/ui-primitives`. Core file in the package named by the note: `packages/client/ui-primitives`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/client/ui-slots/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-slots`. | `named-package-member` |
| [`packages/client/ui-primitives/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/client/ui-primitives`. Core file in the package named by the note: `packages/client/ui-primitives`. | `named-directory-member, named-package-member` |
| [`packages/client/ui-conversation/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-primitives/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/client/ui-primitives`. Core file in the package named by the note: `packages/client/ui-primitives`. | `named-directory-member, named-package-member` |
| [`packages/client/ui-conversation/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-conversation/src/client/input/hub.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/hub.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. Defines `queued`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/MarkdownText.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/MarkdownText.tsx) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client/ui-primitives`. Core file in the package named by the note: `packages/client/ui-primitives`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/TurnTailNodeView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/TurnTailNodeView.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. Defines `messageId`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `stack` | `const` | [`packages/boot/app-boot/src/index.ts:799`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L799) | `const stack = deepest instanceof Error && deepest !== cause ? \`\n${deepest.stack ?? deepest.message}\` : ''` |
| `ToolResultNode` | `interface` | [`packages/client/runtime/src/client/sessions/conversation.ts:184`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts#L184) | `export interface ToolResultNode {` |
| `composer` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:55`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L55) | `const composer = scrollport.querySelector<HTMLElement>('[data-composer-seat]')` |
| `composer` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:336`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L336) | `const composer = scrollport.querySelector<HTMLElement>('[data-composer-seat]')` |
| `messageId` | `const` | [`packages/client/ui-conversation/src/client/chat/TurnTailNodeView.tsx:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/TurnTailNodeView.tsx#L31) | `const messageId = closing.finalNode.messageId` |
| `source` | `const` | [`packages/client/ui-conversation/src/client/conversation-nodes/command.ts:84`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/command.ts#L84) | `const source = event.data.source as unknown as {` |
| `source` | `const` | [`packages/client/ui-conversation/src/client/conversation-nodes/command.ts:140`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/command.ts#L140) | `const source = compactSource(checkpoint.event)` |
| `source` | `const` | [`packages/client/ui-conversation/src/client/conversation-nodes/message.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/message.ts#L26) | `const source = event.data.source` |
| `queued` | `const` | [`packages/client/ui-conversation/src/client/input/hub.ts:183`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/hub.ts#L183) | `const queued = session.getSnapshot().queue.filter(item => item.placement === 'queued')` |
| `composer` | `const` | [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx:170`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx#L170) | `const composer = renderSlotChain(` |
| `submit` | `const` | [`packages/client/ui-conversation/src/client/skeleton/PermissionSelect.tsx:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/PermissionSelect.tsx#L98) | `const submit = (id: string): void => {` |
| `code` | `const` | [`packages/client/ui-primitives/src/ansi.ts:136`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ansi.ts#L136) | `const code = char.codePointAt(0)` |
| `code` | `const` | [`packages/client/ui-primitives/src/ansi.ts:175`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ansi.ts#L175) | `const code = String(codes[index])` |
| `MarkdownText` | `const` | [`packages/client/ui-primitives/src/markdown/MarkdownText.tsx:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/MarkdownText.tsx#L156) | `export const MarkdownText = memo(function MarkdownText({ text, streaming = false, codeLabels, fileMentions }: {` |
| `markdown` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx:933`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx#L933) | `const markdown = cell.kind === 'user' \|\| cell.kind === 'context'` |
| `SurfaceManager` | `class` | [`packages/core/session/src/surface.ts:398`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L398) | `export class SurfaceManager implements SessionSurface {` |

### Tests and executable evidence

- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `ToolDefinition`.
- [`packages/core/tools/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/scoped.spec.ts) — A test under the owning area exercises or imports `ToolDefinition`.
- [`packages/core/session/tests/surface.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/surface.spec.ts) — A test under the owning area exercises or imports `SurfaceManager`.
- [`packages/core/tools/tests/execution-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/execution-mode.spec.ts) — A test under the owning area exercises or imports `ToolDefinition`.
- [`packages/client/ui-conversation/tests/host.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/host.client.spec.ts) — A test under the owning area exercises or imports `steer`.
- [`packages/core/session/tests/gen-persistence-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/gen-persistence-catalog.spec.ts) — A test under the owning area exercises or imports `SurfaceOp`.
- [`packages/client/ui-primitives/tests/markdown.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown.client.spec.tsx) — A test under the owning area exercises or imports `MarkdownText`.
- [`packages/client/ui-primitives/tests/code-block.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/code-block.client.spec.tsx) — A test under the owning area exercises or imports `MarkdownText`.
- Source verification intent: A real model in native or both mode can call one stable show_task_surface schema, the call ends its turn, and a capable Web client renders the same normalized model live and after replay; code-only mode does not advertise it. The static TaskSurfaceDock is the only editor and remains actionable for an active result outside the loaded history window; the keyed toolview remains a read-only transcript summary and replay. A composer takeover hides the still-mounted Dock, preserves its draft, and reveals the same owner after release.

## How to read the implementation

1. Start with [`packages/util/brand/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/brand/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/proposed`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `stack`, `ToolResultNode`, `composer`, `messageId`, `source`, `queued`, `submit`, `code`, `MarkdownText`, `markdown`, `SurfaceManager`, `SurfaceOp`, `ToolDefinition`, `view`
- Regex: `(?i)(stack|ToolResultNode|composer|messageId|source|queued|submit|code)`

```bash
rg -n --pcre2 "(?i)(stack|ToolResultNode|composer|messageId|source|queued|submit|code)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0023. Tagged render-intent union for tool-call presentation](0023-tagged-render-intent-union-for-tool-call-presentation.md): The source note links to this decision directly.
- **`source-link`** — [0049. Canonical tool output contract](0049-canonical-tool-output-contract.md): The source note links to this decision directly.
- **`source-link`** — [0052. The slot system standard --- single register, four props shares, and the framework store seat](0052-the-slot-system-standard-single-register-four-props-shares-and-the-frame.md): The source note links to this decision directly.
- **`source-link`** — [0055. Toolview dissolution --- tool rows are per-view keyed slots](0055-toolview-dissolution-tool-rows-are-per-view-keyed-slots.md): The source note links to this decision directly.
- **`shares-code-with`** — [0177. Safe assistant Markdown in the Web conversation](0177-safe-assistant-markdown-in-the-web-conversation.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0208. Ask-question Web presentation](0208-ask-question-web-presentation.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0220. Web read card frontend --- the read tool's line window renders line-numbered and highlighted](0220-web-read-card-frontend-the-read-tool-s-line-window-renders-line-numbered.md): Shares source implementation: `packages/client/ui-primitives/src/ansi.ts`, `packages/client/ui-primitives/src/index.ts`.
- **`shares-code-with`** — [0175. Web multimodal image input and durable attachments](0175-web-multimodal-image-input-and-durable-attachments.md): Shares source implementation: `packages/util/brand/src/index.ts`, `packages/util/brand/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0520-task-surface-for-structured-session-interaction.md`.
