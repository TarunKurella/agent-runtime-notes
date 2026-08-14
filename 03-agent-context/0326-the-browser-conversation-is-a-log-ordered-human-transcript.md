---
id: "dsh-note-0326"
title: "The browser conversation is a log-ordered human transcript"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-30-web-transcript-log-ordered-projection.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
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
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/tools"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "json"
  - "CompactionSummaryNode"
  - "CommandNode"
  - "ConversationNode"
  - "ConversationSnapshot"
  - "INLINE_SAFE"
  - "CompactionItem"
  - "sourceEventSeq"
  - "isCompactCheckpointSource"
  - "Context"
  - "references"
  - "nodes"
  - "isAppendSurfaceEvent"
  - "isReplacementSurfaceEvent"
search_regex: "(?i)(json|CompactionSummaryNode|CommandNode|ConversationNode|ConversationSnapshot|INLINE_SAFE|CompactionItem|sourceEventSeq)"
---

# 0326. The browser conversation is a log-ordered human transcript — implementation context

## Open this when

The browser client built its conversation from the model-visible surface: FoldAdapter ran the core SurfaceManager over the history window and read surface.nodes. A successful compaction replaces a surface range with one checkpoint node, so the moment that replacement landed the web flow collapsed every message it shadowed into a single dim context row --- conversation the user had already read. Nothing was lost from the log; the defect was entirely in the projection, and the terminal and the host gateway were fixed the same way while the browser was left for this change.

## Source decision

TranscriptAdapter replaces FoldAdapter and never consults surface order. It projects the raw window in log order: every append-origin surface event (isAppendSurfaceEvent) at its own log position, plus one CompactionSummaryNode marker per landed compaction checkpoint. A landed compaction therefore keeps the conversation it shadowed on the model side, and the marker reports where the model stopped seeing that history instead of erasing it.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-30-web-transcript-log-ordered-projection.md](../02-notes/implemented/bug-fix/2026-07-30-web-transcript-log-ordered-projection.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-30-web-transcript-log-ordered-projection.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-30-web-transcript-log-ordered-projection.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/development.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/development.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/client/tsdown.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/tsdown.client.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client`. The source note names this file directly. | `named-directory-member, named-file, symbol-definition` |
| [`packages/client/runtime/tsconfig.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tsconfig.json) | composition and configuration | The source note names this file directly. | `named-file` |
| [`packages/compaction/compaction/src/checkpoint.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/checkpoint.ts) | runtime implementation | The source note names this file directly. Core file in the package named by the note: `packages/compaction/compaction`. | `exact-code-occurrence, named-file, named-package-member, symbol-definition` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. Defines `Context`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. Defines `nodes`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. Defines `SessionEvent`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/surface.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `SurfaceManager`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/llm/llm/src/adapter-failure.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/adapter-failure.ts) | provider/backend adapter | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `CompactionSummaryNode` | `interface` | [`packages/client/runtime/src/client/sessions/conversation.ts:213`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts#L213) | `export interface CompactionSummaryNode {` |
| `CommandNode` | `interface` | [`packages/client/runtime/src/client/sessions/conversation.ts:256`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts#L256) | `export interface CommandNode {` |
| `ConversationNode` | `type` | [`packages/client/runtime/src/client/sessions/conversation.ts:281`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts#L281) | `export type ConversationNode =` |
| `ConversationSnapshot` | `interface` | [`packages/client/runtime/src/client/sessions/conversation.ts:433`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts#L433) | `export interface ConversationSnapshot {` |
| `INLINE_SAFE` | `const` | [`packages/client/tsdown.client.ts:33`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/tsdown.client.ts#L33) | `export const INLINE_SAFE = /^@deepseek-ai\/dsh-(host-apiproxy\|session\|llm\|tools\|brand)(\/\|$)/` |
| `CompactionItem` | `const` | [`packages/client/ui-conversation/src/client/chat/CompactionItem.tsx:35`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/CompactionItem.tsx#L35) | `export const CompactionItem = memo(function CompactionItem({` |
| `sourceEventSeq` | `const` | [`packages/client/ui-conversation/src/client/conversation-nodes/command.ts:54`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/command.ts#L54) | `const sourceEventSeq = data.kind === 'success'` |
| `isCompactCheckpointSource` | `function` | [`packages/compaction/compaction/src/checkpoint.ts:49`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/checkpoint.ts#L49) | `export function isCompactCheckpointSource(source: MessageSource): boolean {` |
| `Context` | `interface` | [`packages/compaction/compaction/src/index.ts:82`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/index.ts#L82) | `interface Context {` |
| `Context` | `interface` | [`packages/context/session-reference/src/index.ts:54`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/index.ts#L54) | `interface Context {` |
| `references` | `const` | [`packages/context/session-reference/src/uri.ts:69`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/uri.ts#L69) | `const references: SessionReferenceInput[] = []` |
| `Context` | `interface` | [`packages/core/session/src/index.ts:38`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L38) | `interface Context {` |
| `nodes` | `const` | [`packages/core/session/src/index.ts:728`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L728) | `const nodes = surface.nodes` |
| `isAppendSurfaceEvent` | `function` | [`packages/core/session/src/surface.ts:51`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L51) | `export function isAppendSurfaceEvent(` |
| `isReplacementSurfaceEvent` | `function` | [`packages/core/session/src/surface.ts:64`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L64) | `export function isReplacementSurfaceEvent(` |

### Tests and executable evidence

- [`packages/client/ui-conversation/tests/conversation-node-definitions.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/conversation-node-definitions.client.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `sourceEventSeq`.
- [`packages/core/session/tests/surface.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/surface.spec.ts) — A test under the owning area exercises or imports `SurfaceManager`. A test under the owning area exercises or imports `isAppendSurfaceEvent`.
- [`packages/client/runtime/tests/session.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/session.client.spec.ts) — A test under the owning area exercises or imports `ConversationSnapshot`. A test under the owning area exercises or imports `ConversationNode`.
- [`packages/interaction/commands/tests/commands.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/tests/commands.spec.ts) — A test under the owning area exercises or imports `sourceEventSeq`.
- [`packages/interaction/commands/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/tests/invariant.spec.ts) — A test under the owning area exercises or imports `sourceEventSeq`.
- [`packages/compaction/compaction/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/tests/invariant.spec.ts) — A test under the owning area exercises or imports `dsh-compaction`.
- [`packages/compaction/compaction/tests/compaction.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/tests/compaction.spec.ts) — A test under the owning area exercises or imports `dsh-compaction`. A test under the owning area exercises or imports `isCompactCheckpointSource`.
- [`packages/compaction/compaction/tests/tool-pairing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/tests/tool-pairing.spec.ts) — A test under the owning area exercises or imports `dsh-compaction`.

## How to read the implementation

1. Start with [`docs/development.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/development.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `domain/agent-loop`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/tools`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `json`, `CompactionSummaryNode`, `CommandNode`, `ConversationNode`, `ConversationSnapshot`, `INLINE_SAFE`, `CompactionItem`, `sourceEventSeq`, `isCompactCheckpointSource`, `Context`, `references`, `nodes`, `isAppendSurfaceEvent`, `isReplacementSurfaceEvent`
- Regex: `(?i)(json|CompactionSummaryNode|CommandNode|ConversationNode|ConversationSnapshot|INLINE_SAFE|CompactionItem|sourceEventSeq)`

```bash
rg -n --pcre2 "(?i)(json|CompactionSummaryNode|CommandNode|ConversationNode|ConversationSnapshot|INLINE_SAFE|CompactionItem|sourceEventSeq)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0601. Live standalone compaction progress in the terminal](0601-live-standalone-compaction-progress-in-the-terminal.md): The source note links to this decision directly.
- **`source-link`** — [0316. The human transcript projects append-origin events](0316-the-human-transcript-projects-append-origin-events.md): The source note links to this decision directly.
- **`source-link`** — [0215. Queued manual compaction with one durable lock](0215-queued-manual-compaction-with-one-durable-lock.md): The source note links to this decision directly.
- **`shares-code-with`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0025. Every LLM request is reconstructable from the session log](0025-every-llm-request-is-reconstructable-from-the-session-log.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0071. Create every message as an identified immutable value](0071-create-every-message-as-an-identified-immutable-value.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0326-the-browser-conversation-is-a-log-ordered-human-transcript.md`.
