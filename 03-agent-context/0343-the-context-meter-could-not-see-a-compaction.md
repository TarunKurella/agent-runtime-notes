---
id: "dsh-note-0343"
title: "the context meter could not see a compaction"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-05-context-meter-blind-to-compaction.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/context"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "messageTokens"
  - "baseline"
  - "contextOccupancy"
  - "measure"
  - "apply"
  - "AgentLoop"
  - "pressureTokens"
  - "ProjectionDefinition"
  - "~used / capacity"
  - "contextPressure.pressureTokens"
  - "compaction-basic"
  - "ctx.llm.stream"
  - "compaction/start"
  - "compaction/summary"
search_regex: "(?i)(messageTokens|baseline|contextOccupancy|measure|apply|AgentLoop|pressureTokens|ProjectionDefinition)"
---

# 0343. the context meter could not see a compaction — implementation context

## Open this when

The composer's context meter took its ring, percentage, and ~used / capacity header from contextPressure.pressureTokens, the newest provider-reported prompt size. That number moves only when a request reports usage, and compaction reports none: compaction-basic summarizes through a direct ctx.llm.stream() call and appends compaction/start, compaction/summary, the replacement user/message, and compaction/end --- no assistant/message, no usage chunk. So the meter was frozen across the one action taken to change it.

## Source decision

contextPressure publishes a second numerator, projectedTokens: the provider sample plus the heuristic repricing of everything the surface gained or lost since that sample was taken, clamped at zero. The fold carries the priced surface through the shared surface-fold.ts and stamps sampledSurfaceTokens when a usage sample lands --- before the same event joins the surface, so an assistant/message anchors against the surface its own request actually carried. stateVersion moves to 3. Only the delta is estimated.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-05-context-meter-blind-to-compaction.md](../02-notes/implemented/bug-fix/2026-08-05-context-meter-blind-to-compaction.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-05-context-meter-blind-to-compaction.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-05-context-meter-blind-to-compaction.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/compaction/compaction-basic/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/index.ts) | package entry point | Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-package-member` |
| [`packages/compaction/compaction-basic/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-package-member` |
| [`packages/compaction/compaction-basic/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/compaction/compaction-basic`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/compaction/compaction-basic`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/hmr/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/invariant.ts) | runtime contract checks | Defines `baseline`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Defines `AgentLoop`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/Toast.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Toast.tsx) | runtime implementation | Defines `measure`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/token-meter/src/usage-projection.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/usage-projection.ts) | runtime implementation | Defines `pressureTokens`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-projection/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection/src/index.ts) | package entry point | Defines `ProjectionDefinition`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `messageTokens`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/StatsLine.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/StatsLine.tsx) | runtime implementation | Defines `contextOccupancy`, a construct named by the note. | `symbol-definition` |
| [`packages/compaction/compaction-basic/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/README.md) | package contract and examples | Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `messageTokens` | `let` | [`packages/client/connection/src/client/fixture.ts:993`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L993) | `let messageTokens = 0` |
| `baseline` | `const` | [`packages/client/hmr/src/invariant.ts:42`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/invariant.ts#L42) | `const baseline = baselines.get(fiber)` |
| `contextOccupancy` | `function` | [`packages/client/ui-conversation/src/client/chat/StatsLine.tsx:143`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/StatsLine.tsx#L143) | `export function contextOccupancy(` |
| `measure` | `const` | [`packages/client/ui-primitives/src/Toast.tsx:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Toast.tsx#L44) | `const measure = (): void => {` |
| `apply` | `const` | [`packages/compaction/compaction-basic/src/invariant.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/invariant.ts#L28) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `AgentLoop` | `class` | [`packages/core/agent-loop/src/index.ts:296`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L296) | `export class AgentLoop extends Service implements AgentFactory {` |
| `pressureTokens` | `const` | [`packages/llm/token-meter/src/usage-projection.ts:184`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/usage-projection.ts#L184) | `const pressureTokens = pressureFrom(usage)` |
| `ProjectionDefinition` | `interface` | [`packages/session/session-projection/src/index.ts:42`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection/src/index.ts#L42) | `export interface ProjectionDefinition<K extends keyof SessionProjectionMap, S> {` |

### Tests and executable evidence

- [`packages/llm/token-meter/tests/token-usage-projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/tests/token-usage-projection.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `pressureTokens`.
- [`packages/client/ui-conversation/tests/context-meter.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/context-meter.client.spec.tsx) — The source note names this file directly.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `AgentLoop`.
- [`packages/core/agent-loop/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/agent.spec.ts) — A test under the owning area exercises or imports `AgentLoop`.
- [`packages/core/agent-loop/tests/resume.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/resume.spec.ts) — A test under the owning area exercises or imports `AgentLoop`.
- [`packages/core/agent-loop/tests/cancel.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/cancel.spec.ts) — A test under the owning area exercises or imports `AgentLoop`.
- [`packages/core/agent-loop/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/settings.spec.ts) — A test under the owning area exercises or imports `AgentLoop`.
- [`packages/core/agent-loop/tests/tool-calls.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/tool-calls.spec.ts) — A test under the owning area exercises or imports `AgentLoop`.
- Source verification intent: packages/llm/token-meter/tests/token-usage-projection.spec.ts covers the carry-forward across surface growth and a compaction (the sample holding still while the projection shrinks) and the zero clamp when heuristic error would drive the figure negative. packages/client/ui-conversation/tests/context-meter.client.spec.tsx pins the ring reading the projected figure, and chat-stats.spec.tsx pins contextOccupancy's preference and its fallback. The end-to-end numbers above came from driving BasicCompactionEngine.compactNow through a real AgentLoop with the projection registry mounted.

## How to read the implementation

1. Start with [`packages/compaction/compaction-basic/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/agent-loop`, `domain/context`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `messageTokens`, `baseline`, `contextOccupancy`, `measure`, `apply`, `AgentLoop`, `pressureTokens`, `ProjectionDefinition`, `~used / capacity`, `contextPressure.pressureTokens`, `compaction-basic`, `ctx.llm.stream`, `compaction/start`, `compaction/summary`
- Regex: `(?i)(messageTokens|baseline|contextOccupancy|measure|apply|AgentLoop|pressureTokens|ProjectionDefinition)`

```bash
rg -n --pcre2 "(?i)(messageTokens|baseline|contextOccupancy|measure|apply|AgentLoop|pressureTokens|ProjectionDefinition)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0255. Composer context meter with heuristic composition breakdown](0255-composer-context-meter-with-heuristic-composition-breakdown.md): The source note links to this decision directly.
- **`shares-code-with`** — [0033. After-call compaction pressure and context-overflow recovery](0033-after-call-compaction-pressure-and-context-overflow-recovery.md): Shares source implementation: `packages/compaction/compaction-basic`, `packages/compaction/compaction-basic/src/index.ts`.
- **`shares-code-with`** — [0215. Queued manual compaction with one durable lock](0215-queued-manual-compaction-with-one-durable-lock.md): Shares source implementation: `packages/compaction/compaction-basic/src/index.ts`, `packages/compaction/compaction-basic/src/invariant.ts`.
- **`shares-code-with`** — [0175. Web multimodal image input and durable attachments](0175-web-multimodal-image-input-and-durable-attachments.md): Shares source implementation: `packages/compaction/compaction-basic/src/index.ts`, `packages/compaction/compaction-basic/src/types.ts`.
- **`shares-code-with`** — [0041. Replay token meter service](0041-replay-token-meter-service.md): Shares source implementation: `packages/compaction/compaction-basic/src/index.ts`, `packages/compaction/compaction-basic/src/invariant.ts`.
- **`shares-code-with`** — [0540. Fold the single compaction backend into its service package](0540-fold-the-single-compaction-backend-into-its-service-package.md): Shares source implementation: `packages/compaction/compaction-basic`, `packages/compaction/compaction-basic/src/index.ts`.
- **`shares-code-with`** — [0518. Recallable compaction --- index checkpoints, a state checkpoint, and in-session history recall](0518-recallable-compaction-index-checkpoints-a-state-checkpoint-and-in-sessio.md): Shares source implementation: `packages/compaction/compaction-basic/src/index.ts`, `packages/compaction/compaction-basic/src/types.ts`.
- **`shares-code-with`** — [0050. Routed model context and compaction policy](0050-routed-model-context-and-compaction-policy.md): Shares source implementation: `packages/compaction/compaction-basic/src/types.ts`, `packages/llm/token-meter/src/usage-projection.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0343-the-context-meter-could-not-see-a-compaction.md`.
