---
id: "dsh-note-0111"
title: "Client Tool presentation ownership"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-08-client-tool-presentation-ownership.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "ChatNodeSeat"
  - "node"
  - "ChatView"
  - "order"
  - "callId"
  - "ToolCallTree"
  - "GenericToolCard"
  - "subCalls"
  - "ToolCallBlock"
  - "ui-conversation"
  - "@deepseek-ai/dsh-client-ui-tool"
  - "ToolCallBlock.subCalls"
  - "tool-call"
  - "conversation.chat.node"
search_regex: "(?i)(ChatNodeSeat|node|ChatView|order|callId|ToolCallTree|GenericToolCard|subCalls)"
---

# 0111. Client Tool presentation ownership — implementation context

## Open this when

Client Runtime already paired Tool call/result events by callId and could recover root/subcall topology from Code Dispatch events, but the Chat view also owned Tool placement in the conversation flow, recursive call-tree composition, Tool-name dispatch, the Generic fallback, card models, and first-party Tool renderers. ui-conversation therefore had to interpret every business Tool name; moving individual React components did not change that ownership, and removing atomic renderers left subcalls without a presentation owner.

## Source decision

Tool is a first-class Client UI presentation concept. @deepseek-ai/dsh-client-ui-tool owns root/subcall composition, atomic renderer dispatch by wire Tool name, the Generic fallback, card models, and details output. Business plugins register only their atomic Tool renderers and do not modify conversation or Session. Conversation data assembly follows the later Conversation business-node decision. The ui-conversation Tool Definition pairs root call/result Session Events, folds Code Dispatch edges into recursive ToolCallBlock.subCalls, and emits one stable tool-call Chat Node.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-08-client-tool-presentation-ownership.md](../02-notes/implemented/architecture/2026-08-08-client-tool-presentation-ownership.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-08-client-tool-presentation-ownership.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-08-client-tool-presentation-ownership.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-tool/src/client/apply.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/apply.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/client/ui-tool/src/client/tool/ToolCallTree.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/ToolCallTree.tsx) | runtime implementation | The source note names this file directly. Core file in the package named by the note: `packages/client/ui-tool`. | `named-file, named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/ChatView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx) | runtime implementation | The source note names this file directly. Defines `ChatView`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/ChatNodeSeat.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatNodeSeat.tsx) | runtime implementation | The source note names this file directly. Core file in the package named by the note: `packages/client/ui-conversation`. | `named-file, named-package-member, symbol-definition` |
| [`packages/client/ui-tool/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-tool`. | `named-package-member` |
| [`packages/client/ui-tool/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-tool`. | `named-package-member` |
| [`packages/client/ui-conversation/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-conversation/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-tool/src/client/tool/toolviews/GenericToolCard.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/toolviews/GenericToolCard.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-tool`. Defines `GenericToolCard`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/conversation-nodes/retry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/retry.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. Defines `node`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/conversation-nodes/turn-max-tokens.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/turn-max-tokens.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. Defines `node`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ChatNodeSeat` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatNodeSeat.tsx:19`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatNodeSeat.tsx#L19) | `export const ChatNodeSeat = memo(function ChatNodeSeat({` |
| `node` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatNodeSeat.tsx:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatNodeSeat.tsx#L23) | `const node = useSession(snapshot => snapshot.chat.nodes.get(nodeKey))` |
| `ChatView` | `function` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:146`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L146) | `export function ChatView({` |
| `order` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:150`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L150) | `const order = useSession(s => s.chat.order)` |
| `node` | `const` | [`packages/client/ui-conversation/src/client/conversation-nodes/retry.ts:57`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/retry.ts#L57) | `const node = scheduledNode(match)` |
| `node` | `const` | [`packages/client/ui-conversation/src/client/conversation-nodes/retry.ts:63`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/retry.ts#L63) | `const node = scheduledNode(match)` |
| `node` | `const` | [`packages/client/ui-conversation/src/client/conversation-nodes/turn-error.ts:90`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/turn-error.ts#L90) | `const node: TurnErrorNode = {` |
| `node` | `const` | [`packages/client/ui-conversation/src/client/conversation-nodes/turn-max-tokens.ts:65`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/turn-max-tokens.ts#L65) | `const node: TurnMaxTokensNode = {` |
| `callId` | `const` | [`packages/client/ui-conversation/src/client/skeleton/DetailsPanel.tsx:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/DetailsPanel.tsx#L71) | `const callId = selection?.callId` |
| `ToolCallTree` | `function` | [`packages/client/ui-tool/src/client/tool/ToolCallTree.tsx:90`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/ToolCallTree.tsx#L90) | `export function ToolCallTree({` |
| `GenericToolCard` | `function` | [`packages/client/ui-tool/src/client/tool/toolviews/GenericToolCard.tsx:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/toolviews/GenericToolCard.tsx#L36) | `export function GenericToolCard({ toolName, block, cwd, openFile, inspect, t }: GenericToolCardProps) {` |
| `subCalls` | `const` | [`packages/client/ui-trajectory/src/client/trajectory-tool-definition.ts:182`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/trajectory-tool-definition.ts#L182) | `const subCalls = (state.children.get(callId) ?? [])` |
| `ToolCallBlock` | `interface` | [`packages/llm/llm/src/types.ts:78`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L78) | `export interface ToolCallBlock {` |

### Tests and executable evidence

- [`packages/client/ui-tool/tests/todo-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/todo-row.client.spec.tsx) — A test under the owning area exercises or imports `subCalls`. A test under the owning area exercises or imports `toolview`.
- [`packages/client/ui-tool/tests/web-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/web-card.client.spec.tsx) — A test under the owning area exercises or imports `subCalls`. A test under the owning area exercises or imports `toolview`.
- [`packages/client/ui-tool/tests/tool-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/tool-row.client.spec.tsx) — A test under the owning area exercises or imports `subCalls`. A test under the owning area exercises or imports `toolview`.
- [`packages/client/ui-tool/tests/diff-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/diff-card.client.spec.tsx) — A test under the owning area exercises or imports `subCalls`. A test under the owning area exercises or imports `GenericToolCard`.
- [`packages/client/ui-tool/tests/read-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/read-card.client.spec.tsx) — A test under the owning area exercises or imports `subCalls`. A test under the owning area exercises or imports `toolview`.
- [`packages/client/ui-tool/tests/search-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/search-card.client.spec.tsx) — A test under the owning area exercises or imports `subCalls`. A test under the owning area exercises or imports `GenericToolCard`.
- [`packages/client/ui-trajectory/tests/layout.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/tests/layout.client.spec.tsx) — A test under the owning area exercises or imports `subCalls`.
- [`packages/client/ui-tool/tests/toolview-slot.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/toolview-slot.client.spec.tsx) — A test under the owning area exercises or imports `subCalls`. A test under the owning area exercises or imports `ui-tool`.
- Source verification intent: ui-conversation tests pin the Tool Definition's call/result pairing, Code Dispatch, interruption, and running-to-settled keyed identity without importing production ui-tool renderers. ui-tool tests mount the real conversation host and pin root/subcall recursion, keyed dispatch, Generic fallback, selection, details, and concrete Tool cards. Assembled Web tests cover the path with both plugins loaded.

## How to read the implementation

1. Start with [`packages/client/ui-tool/src/client/apply.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/apply.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `ChatNodeSeat`, `node`, `ChatView`, `order`, `callId`, `ToolCallTree`, `GenericToolCard`, `subCalls`, `ToolCallBlock`, `ui-conversation`, `@deepseek-ai/dsh-client-ui-tool`, `ToolCallBlock.subCalls`, `tool-call`, `conversation.chat.node`
- Regex: `(?i)(ChatNodeSeat|node|ChatView|order|callId|ToolCallTree|GenericToolCard|subCalls)`

```bash
rg -n --pcre2 "(?i)(ChatNodeSeat|node|ChatView|order|callId|ToolCallTree|GenericToolCard|subCalls)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0113. Client Conversation business-node assembly and keyed Chat snapshots](0113-client-conversation-business-node-assembly-and-keyed-chat-snapshots.md): The source note links to this decision directly.
- **`shares-code-with`** — [0208. Ask-question Web presentation](0208-ask-question-web-presentation.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/ChatView.tsx`, `packages/client/ui-conversation/src/index.ts`.
- **`shares-code-with`** — [0076. Projected token usage and context occupancy](0076-projected-token-usage-and-context-occupancy.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0283. Durable workflow runs in Chat](0283-durable-workflow-runs-in-chat.md): Shares source implementation: `packages/client/ui-tool/src/index.ts`, `packages/client/ui-tool/src/invariant.ts`.
- **`shares-code-with`** — [0055. Toolview dissolution --- tool rows are per-view keyed slots](0055-toolview-dissolution-tool-rows-are-per-view-keyed-slots.md): Shares source implementation: `packages/client/ui-tool/src/client/apply.ts`, `packages/client/ui-tool/src/index.ts`.
- **`shares-code-with`** — [0177. Safe assistant Markdown in the Web conversation](0177-safe-assistant-markdown-in-the-web-conversation.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0182. Session model selection in the Web composer](0182-session-model-selection-in-the-web-composer.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0111-client-tool-presentation-ownership.md`.
