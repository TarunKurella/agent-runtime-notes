---
id: "dsh-note-0369"
title: "Full-session stats-strip figures through a sessionStats projection"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-12-full-session-turn-step-counts.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/lifecycle"
  - "concern/performance"
  - "concern/recovery"
  - "domain/agent-loop"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "sessionStatsOf"
  - "deriveStats"
  - "llmMs"
  - "toolMs"
  - "StatsLine"
  - "resetForRetry"
  - "turns"
  - "lastTurn"
  - "nodes"
  - "interruptedTurnClosers"
  - "isTokenDelta"
  - "StreamChunk"
  - "steps"
  - "chat.legacy.nodes"
search_regex: "(?i)(sessionStatsOf|deriveStats|llmMs|toolMs|StatsLine|resetForRetry|turns|lastTurn)"
---

# 0369. Full-session stats-strip figures through a sessionStats projection — implementation context

## Open this when

The web chat stats strip folded StatsLine's loaded conversation window (deriveStats over chat.legacy.nodes) for every non-token figure: the "N turns · M steps" counter, the LLM and tool wall times, and the TTFT/throughput averages. History is paged 50 messages at a time, so each 加载更早 (Load earlier) click grew the window and every figure with it --- 7 turns · 44 steps became 10 turns · 89 steps after one page, and the LLM duration climbed the same way. The product expectation is whole-session figures independent of how much history a client has loaded.

## Source decision

A new function plugin @deepseek-ai/dsh-session-stats registers a sessionStats projection unit on ctx.sessionProjections, mounted as a web-app bundle row. The value carries the strip's whole non-token figure set --- { turns, steps, llmMs, toolMs, ttftMs, ttftSteps, decodeMs, decodeTokens }, field names mirroring the window fold so the two swap wholesale.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-12-full-session-turn-step-counts.md](../02-notes/implemented/bug-fix/2026-08-12-full-session-turn-step-counts.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-12-full-session-turn-step-counts.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-12-full-session-turn-step-counts.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. Defines `StreamChunk`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/message.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/message.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm`. Defines `isTokenDelta`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/token-meter/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/token-meter`. | `named-package-member` |
| [`packages/llm/token-meter/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/token-meter`. | `named-package-member` |
| [`packages/llm/token-meter/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/token-meter`. | `named-package-member` |
| [`packages/session/session-stats/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-stats/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session/session-stats`. | `named-package-member` |
| [`packages/session/session-stats/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-stats/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/session/session-stats`. | `named-package-member` |
| [`packages/session/session-stats/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-stats/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-stats`. | `named-package-member` |
| [`packages/llm/llm`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/token-meter`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `sessionStatsOf` | `function` | [`packages/client/connection/src/client/fixture.ts:881`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L881) | `function sessionStatsOf(log: readonly SessionEvent[]): {` |
| `deriveStats` | `function` | [`packages/client/ui-conversation/src/client/chat/StatsLine.tsx:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/StatsLine.tsx#L46) | `export function deriveStats(nodes: ConversationSnapshot['nodes']): WindowStats {` |
| `llmMs` | `let` | [`packages/client/ui-conversation/src/client/chat/StatsLine.tsx:49`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/StatsLine.tsx#L49) | `let llmMs = 0` |
| `toolMs` | `let` | [`packages/client/ui-conversation/src/client/chat/StatsLine.tsx:50`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/StatsLine.tsx#L50) | `let toolMs = 0` |
| `StatsLine` | `const` | [`packages/client/ui-conversation/src/client/chat/StatsLine.tsx:163`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/StatsLine.tsx#L163) | `export const StatsLine = memo(function StatsLine({ useSession, useProjection, t }: StatsLineProps) {` |
| `resetForRetry` | `function` | [`packages/client/ui-conversation/src/client/conversation-nodes/assistant.ts:72`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/assistant.ts#L72) | `function resetForRetry(state: AssistantState): AssistantState {` |
| `turns` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L156) | `const turns = new Map<number, TurnBucket>()` |
| `lastTurn` | `const` | [`packages/core/agent-loop/src/agent.ts:92`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L92) | `const lastTurn = session.events.findLast(event => event.type === 'turn/start')?.data.turn ?? 0` |
| `nodes` | `const` | [`packages/core/session/src/index.ts:728`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L728) | `const nodes = surface.nodes` |
| `interruptedTurnClosers` | `function` | [`packages/core/session/src/repair.ts:27`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts#L27) | `export function interruptedTurnClosers(events: readonly SessionEvent[]): SessionEvent[] {` |
| `isTokenDelta` | `function` | [`packages/llm/llm/src/message.ts:251`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/message.ts#L251) | `export function isTokenDelta(chunk: StreamChunk): boolean {` |
| `StreamChunk` | `type` | [`packages/llm/llm/src/types.ts:291`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L291) | `export type StreamChunk =` |
| `steps` | `const` | [`packages/schedule/schedule/src/domain.ts:536`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/domain.ts#L536) | `const steps = Math.floor((acceptedAt - target) / interval)` |

### Tests and executable evidence

- [`packages/core/session/tests/repair.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/repair.spec.ts) — A test under the owning area exercises or imports `interruptedTurnClosers`.
- [`packages/session/session-stats/tests/projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-stats/tests/projection.spec.ts) — A test under the owning area exercises or imports `sessionStats`. A test under the owning area exercises or imports `llmMs`.
- [`packages/llm/token-meter/tests/token-usage-projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/tests/token-usage-projection.spec.ts) — A test under the owning area exercises or imports `tokenUsage`. A test under the owning area exercises or imports `sessionProjections`.
- [`packages/session/session-stats/tests/loader-composition.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-stats/tests/loader-composition.spec.ts) — A test under the owning area exercises or imports `sessionStats`.
- [`packages/client/ui-conversation/tests/chat-stats.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-stats.client.spec.tsx) — A test under the owning area exercises or imports `StatsLine`. A test under the owning area exercises or imports `deriveStats`.
- [`packages/llm/token-meter/tests/context-breakdown-projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/tests/context-breakdown-projection.spec.ts) — A test under the owning area exercises or imports `sessionProjections`.
- [`packages/client/ui-conversation/tests/chat-branch-tails.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-branch-tails.client.spec.tsx) — A test under the owning area exercises or imports `StatsLine`.
- [`packages/client/ui-conversation/tests/gate-branch-tails.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/gate-branch-tails.client.spec.tsx) — A test under the owning area exercises or imports `StatsLine`.

## How to read the implementation

1. Start with [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/compatibility`, `concern/lifecycle`, `concern/performance`, `concern/recovery`, `domain/agent-loop`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `sessionStatsOf`, `deriveStats`, `llmMs`, `toolMs`, `StatsLine`, `resetForRetry`, `turns`, `lastTurn`, `nodes`, `interruptedTurnClosers`, `isTokenDelta`, `StreamChunk`, `steps`, `chat.legacy.nodes`
- Regex: `(?i)(sessionStatsOf|deriveStats|llmMs|toolMs|StatsLine|resetForRetry|turns|lastTurn)`

```bash
rg -n --pcre2 "(?i)(sessionStatsOf|deriveStats|llmMs|toolMs|StatsLine|resetForRetry|turns|lastTurn)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0050. Routed model context and compaction policy](0050-routed-model-context-and-compaction-policy.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/invariant.ts`.
- **`shares-code-with`** — [0037. Provider-routed LLM adapters and a generic pi-ai backend](0037-provider-routed-llm-adapters-and-a-generic-pi-ai-backend.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/invariant.ts`.
- **`shares-code-with`** — [0071. Create every message as an identified immutable value](0071-create-every-message-as-an-identified-immutable-value.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/invariant.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/invariant.ts`.
- **`shares-code-with`** — [0181. Per-provider request retry policies](0181-per-provider-request-retry-policies.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/invariant.ts`.
- **`shares-code-with`** — [0076. Projected token usage and context occupancy](0076-projected-token-usage-and-context-occupancy.md): Shares source implementation: `packages/llm/token-meter/src/index.ts`, `packages/llm/token-meter/src/invariant.ts`.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0369-full-session-stats-strip-figures-through-a-sessionstats-projection.md`.
