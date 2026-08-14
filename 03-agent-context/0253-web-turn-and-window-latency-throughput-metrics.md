---
id: "dsh-note-0253"
title: "Web turn and window latency/throughput metrics"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-04-web-latency-throughput-metrics.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/context"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "conversation"
  - "deriveStats"
  - "usage"
  - "formatLatencySeconds"
  - "assistantStepReading"
  - "deriveTurnMetrics"
  - "turnTimings"
  - "ProjectionDefinition"
  - "stepStartTime"
  - "firstTokenTime"
  - "completedTime"
  - "ui-conversation"
  - "chat/turn-metrics.ts"
  - "TTFT {s}s · {tps} tok/s"
search_regex: "(?i)(conversation|deriveStats|usage|formatLatencySeconds|assistantStepReading|deriveTurnMetrics|turnTimings|ProjectionDefinition)"
---

# 0253. Web turn and window latency/throughput metrics — implementation context

## Open this when

The Web chat records per-step LLM timing (stepStartTime / firstTokenTime / completedTime) and per-step usage, and the trajectory view exposes them per step, but the chat surface answers neither "how responsive was this turn" nor "how fast is this session going": the assistant footer shows only the turn wall time, and the stats line folds only wall-time totals.

## Source decision

A package-local fold, ui-conversation's chat/turn-metrics.ts, is the single derivation from assistant nodes to latency/throughput readings. assistantStepReading turns one node into a step reading: TTFT needs both stepStartTime and firstTokenTime, decode span needs firstTokenTime, negative spans clamp to zero, and output tokens come from the untrusted usage value only when they are finite and non-negative.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-04-web-latency-throughput-metrics.md](../02-notes/implemented/feature/2026-08-04-web-latency-throughput-metrics.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-04-web-latency-throughput-metrics.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-04-web-latency-throughput-metrics.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-conversation/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-conversation/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-conversation/src/client/apply.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. Defines `conversation`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/StatsLine.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/StatsLine.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. Defines `usage`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/turn-metrics.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/turn-metrics.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. Defines `assistantStepReading`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. Defines `turnTimings`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/message-chrome.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/message-chrome.ts) | runtime implementation | Defines `formatLatencySeconds`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session/session-projection/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection/src/index.ts) | package entry point | Defines `ProjectionDefinition`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/README.md) | package contract and examples | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-conversation/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/package.json) | composition and configuration | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `conversation` | `const` | [`packages/client/ui-conversation/src/client/apply.ts:95`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts#L95) | `const conversation = scoped.get('conversation')` |
| `conversation` | `const` | [`packages/client/ui-conversation/src/client/apply.ts:102`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts#L102) | `const conversation = ctx.get('conversation') as ConversationController \| undefined` |
| `conversation` | `const` | [`packages/client/ui-conversation/src/client/apply.ts:246`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts#L246) | `const conversation = concreteConversation(ctx)` |
| `conversation` | `const` | [`packages/client/ui-conversation/src/client/apply.ts:303`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts#L303) | `const conversation = concreteConversation(ctx)` |
| `conversation` | `const` | [`packages/client/ui-conversation/src/client/apply.ts:387`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts#L387) | `const conversation = concreteConversation(ctx)` |
| `deriveStats` | `function` | [`packages/client/ui-conversation/src/client/chat/StatsLine.tsx:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/StatsLine.tsx#L46) | `export function deriveStats(nodes: ConversationSnapshot['nodes']): WindowStats {` |
| `usage` | `const` | [`packages/client/ui-conversation/src/client/chat/StatsLine.tsx:165`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/StatsLine.tsx#L165) | `const usage = useProjection('tokenUsage')` |
| `formatLatencySeconds` | `function` | [`packages/client/ui-conversation/src/client/chat/message-chrome.ts:57`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/message-chrome.ts#L57) | `export function formatLatencySeconds(ms: number): string {` |
| `assistantStepReading` | `function` | [`packages/client/ui-conversation/src/client/chat/turn-metrics.ts:40`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/turn-metrics.ts#L40) | `export function assistantStepReading(node: AssistantNode): StepReading {` |
| `deriveTurnMetrics` | `function` | [`packages/client/ui-conversation/src/client/chat/turn-metrics.ts:70`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/turn-metrics.ts#L70) | `export function deriveTurnMetrics(nodes: readonly ConversationNode[]): Map<number, TurnMetrics> {` |
| `turnTimings` | `const` | [`packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts:317`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/chat-snapshot-builder.ts#L317) | `const turnTimings = new Map<number, { startTime: number; endTime?: number }>()` |
| `ProjectionDefinition` | `interface` | [`packages/session/session-projection/src/index.ts:42`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection/src/index.ts#L42) | `export interface ProjectionDefinition<K extends keyof SessionProjectionMap, S> {` |

### Tests and executable evidence

- [`packages/session/session-projection/tests/registry.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection/tests/registry.spec.ts) — A test under the owning area exercises or imports `ProjectionDefinition`.
- [`packages/client/ui-conversation/tests/skeleton.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/skeleton.client.spec.tsx) — A test under the owning area exercises or imports `turnTimings`.
- [`packages/client/ui-conversation/tests/chat-view.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-view.client.spec.tsx) — A test under the owning area exercises or imports `stepStartTime`. A test under the owning area exercises or imports `firstTokenTime`.
- [`packages/client/ui-conversation/tests/input-bar.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-bar.client.spec.tsx) — A test under the owning area exercises or imports `turnTimings`.
- [`packages/client/ui-conversation/tests/chat-stats.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-stats.client.spec.tsx) — A test under the owning area exercises or imports `stepStartTime`. A test under the owning area exercises or imports `firstTokenTime`.
- [`packages/client/ui-conversation/tests/queue-dock.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/queue-dock.client.spec.tsx) — A test under the owning area exercises or imports `turnTimings`.
- [`packages/client/ui-conversation/tests/turn-metrics.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/turn-metrics.client.spec.ts) — A test under the owning area exercises or imports `stepStartTime`. A test under the owning area exercises or imports `firstTokenTime`.
- [`packages/client/ui-conversation/tests/input-matrix.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-matrix.client.spec.tsx) — A test under the owning area exercises or imports `turnTimings`.

## How to read the implementation

1. Start with [`packages/client/ui-conversation/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/feature`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/schema-types`, `domain/agent-loop`, `domain/context`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/storage`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `conversation`, `deriveStats`, `usage`, `formatLatencySeconds`, `assistantStepReading`, `deriveTurnMetrics`, `turnTimings`, `ProjectionDefinition`, `stepStartTime`, `firstTokenTime`, `completedTime`, `ui-conversation`, `chat/turn-metrics.ts`, `TTFT {s}s · {tps} tok/s`
- Regex: `(?i)(conversation|deriveStats|usage|formatLatencySeconds|assistantStepReading|deriveTurnMetrics|turnTimings|ProjectionDefinition)`

```bash
rg -n --pcre2 "(?i)(conversation|deriveStats|usage|formatLatencySeconds|assistantStepReading|deriveTurnMetrics|turnTimings|ProjectionDefinition)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0330. Fork anchor floors to an event seq](0330-fork-anchor-floors-to-an-event-seq.md): Shares source implementation: `packages/client/ui-conversation`, `packages/client/ui-conversation/README.md`.
- **`shares-code-with`** — [0486. Remove the steering interjection caption](0486-remove-the-steering-interjection-caption.md): Shares source implementation: `packages/client/ui-conversation`, `packages/client/ui-conversation/README.md`.
- **`shares-code-with`** — [0076. Projected token usage and context occupancy](0076-projected-token-usage-and-context-occupancy.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/StatsLine.tsx`, `packages/client/ui-conversation/src/index.ts`.
- **`shares-code-with`** — [0294. Web attachment display aligns with DeepSeek Chat via attachment atoms](0294-web-attachment-display-aligns-with-deepseek-chat-via-attachment-atoms.md): Shares source implementation: `packages/client/ui-conversation/src/client/apply.ts`, `packages/client/ui-conversation/src/index.ts`.
- **`shares-code-with`** — [0675. Web UI drops steer entry and interjection chrome](0675-web-ui-drops-steer-entry-and-interjection-chrome.md): Shares source implementation: `packages/client/ui-conversation/README.md`, `packages/client/ui-conversation/src/index.ts`.
- **`shares-code-with`** — [0177. Safe assistant Markdown in the Web conversation](0177-safe-assistant-markdown-in-the-web-conversation.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0182. Session model selection in the Web composer](0182-session-model-selection-in-the-web-composer.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0605. Web composer stats detail and input-zone polish](0605-web-composer-stats-detail-and-input-zone-polish.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/StatsLine.tsx`, `packages/client/ui-conversation/src/client/chat/turn-metrics.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0253-web-turn-and-window-latency-throughput-metrics.md`.
