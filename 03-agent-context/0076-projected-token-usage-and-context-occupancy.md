---
id: "dsh-note-0076"
title: "Projected token usage and context occupancy"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-29-projected-token-usage-and-request-context.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "change"
  - "StatsLine"
  - "measure"
  - "models"
  - "lastSeq"
  - "headerEquals"
  - "EpochHeader"
  - "cancelled"
  - "contextWindow"
  - "pressureTokens"
  - "@deepseek-ai/dsh-token-meter"
  - "ctx.sessionProjections"
  - "sessionProjections"
  - "tokenUsage"
search_regex: "(?i)(change|StatsLine|measure|models|lastSeq|headerEquals|EpochHeader|cancelled)"
---

# 0076. Projected token usage and context occupancy — implementation context

## Open this when

The Web stats line derived token totals from the currently loaded conversation nodes. That window is paged, so scrolling changed the totals, and compaction replaces visible content without preserving the billing behind it. Durable provider billing needs a source that survives both. Context occupancy needs a numerator and a denominator that no existing surface carried to the browser: the prompt size of the latest request, and the capacity of the route it used.

## Source decision

Both values are ordinary durable session-projection state. @deepseek-ai/dsh-token-meter registers two units when ctx.sessionProjections is present. tokenUsage folds the complete durable log into uncached input, output, cache-read, and cache-write buckets. An assistant/chunk usage sample survives a later failed request; an assistant/message usage value for the same (turn, step) replaces the earlier sample instead of double-counting it. Reasoning stays an output subdivision. Compaction and surface replacement do not erase earlier billing.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-29-projected-token-usage-and-request-context.md](../02-notes/implemented/architecture/2026-07-29-projected-token-usage-and-request-context.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-29-projected-token-usage-and-request-context.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-29-projected-token-usage-and-request-context.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/token-meter/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/token-meter`. | `named-package-member` |
| [`packages/llm/token-meter/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/token-meter`. | `named-package-member` |
| [`packages/llm/token-meter/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/token-meter`. | `named-package-member` |
| [`packages/client/ui-conversation/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-model-selection/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-model-selection`. | `named-package-member` |
| [`packages/client/ui-conversation/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/llm/token-meter/src/usage-projection.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/usage-projection.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/token-meter`. Defines `pressureTokens`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-model-selection/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-model-selection`. | `named-package-member` |
| [`packages/client/ui-model-selection/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/client/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-model-selection`. Defines `models`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/StatsLine.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/StatsLine.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. Defines `StatsLine`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/ContextBody.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ContextBody.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. Defines `change`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/token-meter`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `change` | `const` | [`packages/client/ui-conversation/src/client/chat/ContextBody.tsx:203`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ContextBody.tsx#L203) | `const change = asRecord(entry)` |
| `StatsLine` | `const` | [`packages/client/ui-conversation/src/client/chat/StatsLine.tsx:163`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/StatsLine.tsx#L163) | `export const StatsLine = memo(function StatsLine({ useSession, useProjection, t }: StatsLineProps) {` |
| `measure` | `const` | [`packages/client/ui-conversation/src/client/chat/StatsLine.tsx:214`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/StatsLine.tsx#L214) | `const measure = () => { setTruncated(el.scrollWidth > el.clientWidth) }` |
| `models` | `const` | [`packages/client/ui-model-selection/src/client/index.ts:124`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/client/index.ts#L124) | `const models = scope.modelDirectories` |
| `models` | `const` | [`packages/client/ui-model-selection/src/client/index.ts:155`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/client/index.ts#L155) | `const models = scope.modelDirectories` |
| `lastSeq` | `const` | [`packages/core/session/src/index.ts:1114`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L1114) | `const lastSeq = events.at(-1)?.seq` |
| `headerEquals` | `function` | [`packages/core/session/src/request-header.ts:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/request-header.ts#L44) | `export function headerEquals(a: EpochHeader, b: EpochHeader): boolean {` |
| `EpochHeader` | `interface` | [`packages/core/session/src/types.ts:201`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L201) | `export interface EpochHeader {` |
| `cancelled` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2037`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2037) | `const cancelled = () => err<{ items: SessionSearchItem[]; hasMore: boolean }>(request, {` |
| `contextWindow` | `const` | [`packages/llm/token-meter/src/usage-projection.ts:172`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/usage-projection.ts#L172) | `const contextWindow = event.data.contextWindow` |
| `pressureTokens` | `const` | [`packages/llm/token-meter/src/usage-projection.ts:184`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/usage-projection.ts#L184) | `const pressureTokens = pressureFrom(usage)` |

### Tests and executable evidence

- [`packages/core/session/tests/fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/fork.spec.ts) — A test under the owning area exercises or imports `lastSeq`.
- [`packages/core/session/tests/request-header.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/request-header.spec.ts) — A test under the owning area exercises or imports `EpochHeader`. A test under the owning area exercises or imports `headerEquals`.
- [`packages/llm/token-meter/tests/token-meter.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/tests/token-meter.spec.ts) — A test under the owning area exercises or imports `EpochHeader`. A test under the owning area exercises or imports `measure`.
- [`packages/llm/token-meter/tests/token-usage-projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/tests/token-usage-projection.spec.ts) — A test under the owning area exercises or imports `sessionProjections`. A test under the owning area exercises or imports `tokenUsage`.
- [`packages/client/ui-conversation/tests/skeleton.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/skeleton.client.spec.tsx) — A test under the owning area exercises or imports `useProjection`.
- [`packages/client/ui-conversation/tests/chat-view.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-view.client.spec.tsx) — A test under the owning area exercises or imports `useProjection`.
- [`packages/client/ui-conversation/tests/input-bar.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-bar.client.spec.tsx) — A test under the owning area exercises or imports `useProjection`. A test under the owning area exercises or imports `measure`.
- [`packages/client/ui-conversation/tests/chat-stats.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-stats.client.spec.tsx) — A test under the owning area exercises or imports `tokenUsage`. A test under the owning area exercises or imports `contextPressure`.

## How to read the implementation

1. Start with [`packages/llm/token-meter/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `change`, `StatsLine`, `measure`, `models`, `lastSeq`, `headerEquals`, `EpochHeader`, `cancelled`, `contextWindow`, `pressureTokens`, `@deepseek-ai/dsh-token-meter`, `ctx.sessionProjections`, `sessionProjections`, `tokenUsage`
- Regex: `(?i)(change|StatsLine|measure|models|lastSeq|headerEquals|EpochHeader|cancelled)`

```bash
rg -n --pcre2 "(?i)(change|StatsLine|measure|models|lastSeq|headerEquals|EpochHeader|cancelled)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0182. Session model selection in the Web composer](0182-session-model-selection-in-the-web-composer.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0050. Routed model context and compaction policy](0050-routed-model-context-and-compaction-policy.md): Shares source implementation: `packages/llm/token-meter/src/index.ts`, `packages/llm/token-meter/src/invariant.ts`.
- **`shares-code-with`** — [0041. Replay token meter service](0041-replay-token-meter-service.md): Shares source implementation: `packages/llm/token-meter/src/index.ts`, `packages/llm/token-meter/src/invariant.ts`.
- **`shares-code-with`** — [0255. Composer context meter with heuristic composition breakdown](0255-composer-context-meter-with-heuristic-composition-breakdown.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/llm/token-meter/src/index.ts`.
- **`shares-code-with`** — [0369. Full-session stats-strip figures through a sessionStats projection](0369-full-session-stats-strip-figures-through-a-sessionstats-projection.md): Shares source implementation: `packages/llm/token-meter/src/index.ts`, `packages/llm/token-meter/src/invariant.ts`.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0253. Web turn and window latency/throughput metrics](0253-web-turn-and-window-latency-throughput-metrics.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/StatsLine.tsx`, `packages/client/ui-conversation/src/index.ts`.
- **`shares-code-with`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): Shares source implementation: `packages/llm/token-meter/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0076-projected-token-usage-and-context-occupancy.md`.
