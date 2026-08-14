---
id: "dsh-note-0352"
title: "unpriced surface replacements fold neutrally"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-06-token-surface-unpriced-replace-compatibility.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/simplification"
  - "domain/context"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
aliases:
  - "messageTokens"
  - "shadowedTokenCount"
  - "foldSurfaceProjection"
  - "contextPressure"
  - "contextBreakdown"
  - "compaction/summary"
  - "compaction/prune"
  - "deltaTokens: 0"
  - "stateVersion"
  - "surface-fold.ts"
  - "ctx.tokenMeter.measure"
  - "contextBreakdown.messageTokens"
  - "contextPressure.projectedTokens"
  - "projectedTokens"
search_regex: "(?i)(messageTokens|shadowedTokenCount|foldSurfaceProjection|contextPressure|contextBreakdown|compaction/summary|compaction/prune|deltaTokens:[- ]0)"
---

# 0352. unpriced surface replacements fold neutrally — implementation context

## Open this when

The contextPressure and contextBreakdown projections keep a running surface-token total plus at most one pending shadow-price claim, so their persisted checkpoints stay O(1) over a session's life. Current replace producers append a compaction/summary or compaction/prune metering event immediately before the replacement; its shadowedTokenCount prices the exact replaced range, and foldSurfaceProjection turns that into the signed delta. Sessions recorded before the shadow-price protocol log replacements with no adjacent metering event.

## Source decision

A replace that arrives with no armed claim folds price-neutrally: foldSurfaceProjection returns deltaTokens: 0, pricing the replaced range as if it had cost exactly what its replacement costs, and replay continues. A claim expired by an intervening event reaches the same neutral path, since the fold cannot distinguish it from a log that never metered. An armed claim naming a different range still throws.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-06-token-surface-unpriced-replace-compatibility.md](../02-notes/implemented/bug-fix/2026-08-06-token-surface-unpriced-replace-compatibility.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-06-token-surface-unpriced-replace-compatibility.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-06-token-surface-unpriced-replace-compatibility.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `messageTokens`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/token-meter/src/surface-projection.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/surface-projection.ts) | runtime implementation | Defines `foldSurfaceProjection`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/conversation-nodes/command.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/command.ts) | runtime implementation | Defines `shadowedTokenCount`, a construct named by the note. | `symbol-definition` |
| [`docs/subsystems/session.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/session.md) | package contract and examples | Contains the exact code literal `compaction/summary` named by the note. | `exact-code-occurrence` |
| [`docs/persistence-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/persistence-catalog.md) | package contract and examples | Contains the exact code literal `compaction/summary` named by the note. Contains the exact code literal `compaction/prune` named by the note. | `exact-code-occurrence` |
| [`docs/subsystems/session.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/session.zh.md) | package contract and examples | Contains the exact code literal `compaction/summary` named by the note. | `exact-code-occurrence` |
| [`docs/subsystems/compaction.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/compaction.md) | package contract and examples | Contains the exact code literal `compaction/summary` named by the note. Contains the exact code literal `compaction/prune` named by the note. | `exact-code-occurrence` |
| [`docs/persistence-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/persistence-catalog.zh.md) | package contract and examples | Contains the exact code literal `compaction/summary` named by the note. Contains the exact code literal `compaction/prune` named by the note. | `exact-code-occurrence` |
| [`docs/subsystems/compaction.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/compaction.zh.md) | package contract and examples | Contains the exact code literal `compaction/summary` named by the note. Contains the exact code literal `compaction/prune` named by the note. | `exact-code-occurrence` |
| [`packages/compaction/compaction/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/types.ts) | public types and contract | Contains the exact code literal `compaction/prune` named by the note. | `exact-code-occurrence` |
| [`packages/core/session/src/known-event-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/known-event-types.ts) | runtime implementation | Contains the exact code literal `compaction/summary` named by the note. Contains the exact code literal `compaction/prune` named by the note. | `exact-code-occurrence` |
| [`packages/compaction/compaction-tool-result-pruner/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-tool-result-pruner/src/index.ts) | package entry point | Contains the exact code literal `compaction/prune` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `messageTokens` | `let` | [`packages/client/connection/src/client/fixture.ts:993`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L993) | `let messageTokens = 0` |
| `shadowedTokenCount` | `let` | [`packages/client/ui-conversation/src/client/conversation-nodes/command.ts:106`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/command.ts#L106) | `let shadowedTokenCount: number \| null = null` |
| `foldSurfaceProjection` | `function` | [`packages/llm/token-meter/src/surface-projection.ts:66`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/surface-projection.ts#L66) | `export function foldSurfaceProjection(` |

### Tests and executable evidence

- [`packages/llm/token-meter/tests/token-usage-projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/tests/token-usage-projection.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `contextPressure`.
- [`packages/llm/token-meter/tests/context-breakdown-projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/tests/context-breakdown-projection.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `contextBreakdown`.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `contextPressure`. A test under the owning area exercises or imports `contextBreakdown`.
- [`packages/subagent/subagent/tests/list-children.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/list-children.spec.ts) — A test under the owning area exercises or imports `stateVersion`.
- [`packages/client/connection/tests/fixture.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/tests/fixture.client.spec.ts) — A test under the owning area exercises or imports `contextPressure`. A test under the owning area exercises or imports `contextBreakdown`.
- [`packages/host/apiproxy/tests/api-proxy-projections.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-projections.spec.ts) — A test under the owning area exercises or imports `stateVersion`.
- [`packages/session/session-projection/tests/registry.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection/tests/registry.spec.ts) — A test under the owning area exercises or imports `stateVersion`.
- [`packages/session/session-projection-cache/tests/cache.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection-cache/tests/cache.spec.ts) — A test under the owning area exercises or imports `stateVersion`.
- Source verification intent: packages/llm/token-meter/tests/context-breakdown-projection.spec.ts pins the neutral fold for the no-claim and expired-claim replacements, the throw for a mismatched claim, and the exact pricing for a matched one. packages/llm/token-meter/tests/token-usage-projection.spec.ts pins contextPressure holding still across an unpriced replacement.

## How to read the implementation

1. Start with [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/evidence`, `concern/simplification`, `domain/context`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/projection`
- Aliases: `messageTokens`, `shadowedTokenCount`, `foldSurfaceProjection`, `contextPressure`, `contextBreakdown`, `compaction/summary`, `compaction/prune`, `deltaTokens: 0`, `stateVersion`, `surface-fold.ts`, `ctx.tokenMeter.measure`, `contextBreakdown.messageTokens`, `contextPressure.projectedTokens`, `projectedTokens`
- Regex: `(?i)(messageTokens|shadowedTokenCount|foldSurfaceProjection|contextPressure|contextBreakdown|compaction/summary|compaction/prune|deltaTokens:[- ]0)`

```bash
rg -n --pcre2 "(?i)(messageTokens|shadowedTokenCount|foldSurfaceProjection|contextPressure|contextBreakdown|compaction/summary|compaction/prune|deltaTokens:[- ]0)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0343. the context meter could not see a compaction](0343-the-context-meter-could-not-see-a-compaction.md): The source note links to this decision directly.
- **`shares-code-with`** — [0535. Drop durable step boundary events](0535-drop-durable-step-boundary-events.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`.
- **`shares-code-with`** — [0519. Interactive side sessions and merge-back](0519-interactive-side-sessions-and-merge-back.md): Shares source implementation: `apps/cli/tests/web-agent-presets.e2e.ts`.
- **`shares-code-with`** — [0173. Durable subagent catalog and list_agents](0173-durable-subagent-catalog-and-list-agents.md): Shares source implementation: `packages/subagent/subagent/tests/list-children.spec.ts`.
- **`shares-code-with`** — [0611. Keep the Code Mode result card complete](0611-keep-the-code-mode-result-card-complete.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`.
- **`shares-code-with`** — [0447. Fold trace-only session facts into load-bearing events](0447-fold-trace-only-session-facts-into-load-bearing-events.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`.
- **`shares-code-with`** — [0333. Web stop preserves pending Queue](0333-web-stop-preserves-pending-queue.md): Shares source implementation: `packages/client/connection/tests/fixture.client.spec.ts`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0352-unpriced-surface-replacements-fold-neutrally.md`.
