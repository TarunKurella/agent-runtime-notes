---
id: "dsh-note-0113"
title: "Client Conversation business-node assembly and keyed Chat snapshots"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-09-client-conversation-node-assembly.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "closed"
  - "user"
  - "ConversationMatch"
  - "ConversationNodeContext"
  - "ConversationNodeDefinition"
  - "conversationContextKey"
  - "ConversationViewRegistry"
  - "upserts"
  - "ConversationLocationIndex"
  - "chat"
  - "location"
  - "turn"
  - "ChatNodeSeat"
  - "node"
search_regex: "(?i)(closed|user|ConversationMatch|ConversationNodeContext|ConversationNodeDefinition|conversationContextKey|ConversationViewRegistry|upserts)"
---

# 0113. Client Conversation business-node assembly and keyed Chat snapshots — implementation context

## Open this when

Client Session owned transport windows, connection state, and pending interactions while also interpreting Assistant, Tool, message, command, compaction, retry, and turn-tail events in a centralized transcript fold. Adding one business node required changes to Session switches, history replay, indexes, caches, and React grouping; business identity, state evolution, and final presentation had no independent owner. The old path also placed running Assistant and Tool values outside the finalized flow.

## Source decision

Client Runtime provides a target-neutral Conversation Node assembly engine. Business plugins register Event Definitions, and view plugins register per-Session View Builders. ui-conversation registers the first built-in Definitions and the chat builder; Session only submits the current contiguous Event window to the engine and publishes its snapshot instead of interpreting individual conversation businesses. This Note retains the derivation, business-by-business validation, responsibilities, algorithms, and trade-offs that remain relevant after implementation. Registry contributions are Cordis effects.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-09-client-conversation-node-assembly.md](../02-notes/implemented/architecture/2026-08-09-client-conversation-node-assembly.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-09-client-conversation-node-assembly.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-09-client-conversation-node-assembly.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/runtime/src/client/contract/conversation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/conversation.ts) | runtime implementation | The source note names this file directly. Defines `ConversationNodeDefinition`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/ChatView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx) | runtime implementation | The source note names this file directly. Defines `hasMore`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/client/runtime/src/client/conversation/view-registry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/conversation/view-registry.ts) | runtime implementation | The source note names this file directly. Defines `ConversationViewRegistry`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/ChatNodeSeat.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatNodeSeat.tsx) | runtime implementation | The source note names this file directly. Defines `ChatNodeSeat`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/client/ui-conversation/src/client/contract/chat-nodes.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/contract/chat-nodes.ts) | runtime implementation | The source note names this file directly. Defines `ChatNodeDataMap`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/client/runtime/src/client/sessions/conversation-location-index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation-location-index.ts) | runtime implementation | The source note names this file directly. Defines `ConversationLocationIndex`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts) | runtime implementation | The source note names this file directly. Defines `step`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. Defines `seq`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. Defines `SessionEvent`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/repair.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `seq`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-tool/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-tool`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/surface.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `state`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `closed` | `let` | [`packages/acp/acp/src/index.ts:111`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L111) | `let closed = false` |
| `user` | `const` | [`packages/boot/app-boot/src/index.ts:185`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L185) | `const user = home === resolve(cwd) ? undefined : readEnvLayer(binName, home, warn)` |
| `ConversationMatch` | `interface` | [`packages/client/runtime/src/client/contract/conversation.ts:99`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/conversation.ts#L99) | `export interface ConversationMatch extends ConversationEventInput {` |
| `ConversationNodeContext` | `interface` | [`packages/client/runtime/src/client/contract/conversation.ts:133`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/conversation.ts#L133) | `export interface ConversationNodeContext<State = unknown> {` |
| `ConversationNodeDefinition` | `interface` | [`packages/client/runtime/src/client/contract/conversation.ts:171`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/conversation.ts#L171) | `export interface ConversationNodeDefinition<State = unknown> {` |
| `conversationContextKey` | `function` | [`packages/client/runtime/src/client/contract/conversation.ts:272`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/conversation.ts#L272) | `export function conversationContextKey(kind: string, id: string): string {` |
| `ConversationViewRegistry` | `class` | [`packages/client/runtime/src/client/conversation/view-registry.ts:6`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/conversation/view-registry.ts#L6) | `export class ConversationViewRegistry extends ConversationDefinitionRegistry<ConversationViewDefinition> {` |
| `upserts` | `const` | [`packages/client/runtime/src/client/sessions/conversation-assembler.ts:308`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation-assembler.ts#L308) | `const upserts = upsertsByTarget.get(view.target) ?? []` |
| `ConversationLocationIndex` | `class` | [`packages/client/runtime/src/client/sessions/conversation-location-index.ts:124`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation-location-index.ts#L124) | `export class ConversationLocationIndex {` |
| `chat` | `const` | [`packages/client/runtime/src/client/sessions/session.ts:735`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/session.ts#L735) | `const chat = (this.conversation.snapshot('chat') as ChatSnapshot \| undefined) ?? EMPTY_CHAT_SNAPSHOT` |
| `location` | `const` | [`packages/client/ui-conversation/src/client/apply.ts:82`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts#L82) | `const location = snapshot.chat.nodes.get(nodeKey)?.location` |
| `turn` | `const` | [`packages/client/ui-conversation/src/client/chat/AssistantNodeView.tsx:10`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/AssistantNodeView.tsx#L10) | `const turn = node.location.kind === 'turn' \|\| node.location.kind === 'step'` |
| `ChatNodeSeat` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatNodeSeat.tsx:19`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatNodeSeat.tsx#L19) | `export const ChatNodeSeat = memo(function ChatNodeSeat({` |
| `node` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatNodeSeat.tsx:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatNodeSeat.tsx#L23) | `const node = useSession(snapshot => snapshot.chat.nodes.get(nodeKey))` |
| `ChatView` | `function` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:146`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L146) | `export function ChatView({` |
| `order` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:150`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L150) | `const order = useSession(s => s.chat.order)` |

### Tests and executable evidence

- [`packages/core/session/tests/surface.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/surface.spec.ts) — A test under the owning area exercises or imports `baseSeq`.
- [`packages/core/session/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/invariant.spec.ts) — A test under the owning area exercises or imports `unresolved`. A test under the owning area exercises or imports `prepend`.
- [`packages/lsp/lsp-stdio/tests/built-lib.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/built-lib.e2e.ts) — A test under the owning area exercises or imports `locations`.
- [`packages/lsp/lsp-stdio/tests/instance.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/instance.spec.ts) — A test under the owning area exercises or imports `locations`.
- [`packages/lsp/lsp-stdio/tests/lifecycle.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/lifecycle.spec.ts) — A test under the owning area exercises or imports `locations`.
- [`packages/core/session/tests/request-header.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/request-header.spec.ts) — A test under the owning area exercises or imports `legacy`.
- [`packages/client/runtime/tests/session.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/session.client.spec.ts) — A test under the owning area exercises or imports `ConversationNodeDefinition`. A test under the owning area exercises or imports `upserts`.
- [`packages/lsp/lsp-stdio/tests/typescript-server.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/typescript-server.e2e.ts) — A test under the owning area exercises or imports `locations`.
- Source verification intent: Runtime tests pin Definition lifecycle registration, exact-ID append, update-before-start collection followed by forward replay after start, prepend identity, Reader window-gap repair, transitive dependencies, Location closure, Step→Turn data phase order, Location data replacement, publication cadence, illegal withdrawal, and per-target Builders.

## How to read the implementation

1. Start with [`packages/client/runtime/src/client/contract/conversation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/conversation.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/agent-loop`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/jobs-tasks`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `closed`, `user`, `ConversationMatch`, `ConversationNodeContext`, `ConversationNodeDefinition`, `conversationContextKey`, `ConversationViewRegistry`, `upserts`, `ConversationLocationIndex`, `chat`, `location`, `turn`, `ChatNodeSeat`, `node`
- Regex: `(?i)(closed|user|ConversationMatch|ConversationNodeContext|ConversationNodeDefinition|conversationContextKey|ConversationViewRegistry|upserts)`

```bash
rg -n --pcre2 "(?i)(closed|user|ConversationMatch|ConversationNodeContext|ConversationNodeDefinition|conversationContextKey|ConversationViewRegistry|upserts)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0111. Client Tool presentation ownership](0111-client-tool-presentation-ownership.md): The source note links to this decision directly.
- **`source-link`** — [0127. Trajectory assembly from registered Conversation Contexts](0127-trajectory-assembly-from-registered-conversation-contexts.md): The source note links to this decision directly.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/repair.ts`.
- **`shares-code-with`** — [0035. Agent-scope runtime design and correctness](0035-agent-scope-runtime-design-and-correctness.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/repair.ts`.
- **`shares-code-with`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/repair.ts`.
- **`shares-code-with`** — [0045. Web client architecture --- the client cordis plugin tree, the slot system, and the React-free object layer](0045-web-client-architecture-the-client-cordis-plugin-tree-the-slot-system-an.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0110. Bounded session persistence write batching](0110-bounded-session-persistence-write-batching.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/repair.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0113-client-conversation-business-node-assembly-and-keyed-chat-snapshots.md`.
