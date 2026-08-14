---
id: "dsh-note-0255"
title: "Composer context meter with heuristic composition breakdown"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-05-composer-context-meter-breakdown.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/context"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
aliases:
  - "messageTokens"
  - "measure"
  - "ContextMeter"
  - "canonicalHeader"
  - "deriveEventMessage"
  - "systemTokens"
  - "toolsTokens"
  - "foldSurfaceTokens"
  - "pressureTokens"
  - "Context N% of X"
  - "contextPressure"
  - "dsh-session"
  - "dsh-token-meter"
  - "src/estimate.ts"
search_regex: "(?i)(messageTokens|measure|ContextMeter|canonicalHeader|deriveEventMessage|systemTokens|toolsTokens|foldSurfaceTokens)"
---

# 0255. Composer context meter with heuristic composition breakdown — implementation context

## Open this when

The Web chat's stats line showed context occupancy as one inline figure (Context N% of X) among its billing groups. That answers "how full" but not "what fills it": nothing showed how the window divides between the system prompt, tool schemas, and conversation, and the one-line row has no room for that detail. The available numbers also live in two vocabularies --- the provider-exact billed prompt size from contextPressure versus the token-meter's fixed character heuristic --- and no existing surface could present composition without conflating them.

## Source decision

Three cooperating pieces, one per package boundary: dsh-session exports the pure deriveEventMessage(event) (previously reachable only as a Session method, which now delegates to it) so a host-side fold can price surface nodes without a Session instance. dsh-token-meter extracts its pricing heuristic into src/estimate.ts and its positional surface fold into src/surface-fold.ts --- both shared verbatim with the measurement service --- and registers a third session projection, contextBreakdown, carrying systemTokens / toolsTokens / messageTokens.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-05-composer-context-meter-breakdown.md](../02-notes/implemented/feature/2026-08-05-composer-context-meter-breakdown.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-05-composer-context-meter-breakdown.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-05-composer-context-meter-breakdown.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/surface.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `deriveEventMessage`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/token-meter/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/token-meter`. | `named-package-member` |
| [`packages/llm/token-meter/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/token-meter`. | `named-package-member` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/llm/token-meter/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/token-meter`. | `named-package-member` |
| [`packages/core/session/src/request-header.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/request-header.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `canonicalHeader`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/llm/token-meter/src/surface-fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/surface-fold.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/token-meter`. Defines `foldSurfaceTokens`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/llm/token-meter/src/usage-projection.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/usage-projection.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/token-meter`. Defines `pressureTokens`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `messageTokens` | `let` | [`packages/client/connection/src/client/fixture.ts:993`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L993) | `let messageTokens = 0` |
| `measure` | `const` | [`packages/client/ui-conversation/src/client/chat/StatsLine.tsx:214`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/StatsLine.tsx#L214) | `const measure = () => { setTruncated(el.scrollWidth > el.clientWidth) }` |
| `ContextMeter` | `function` | [`packages/client/ui-conversation/src/client/skeleton/ContextMeter.tsx:40`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ContextMeter.tsx#L40) | `export function ContextMeter({ useProjection, t }: ContextMeterProps) {` |
| `canonicalHeader` | `function` | [`packages/core/session/src/request-header.ts:21`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/request-header.ts#L21) | `export function canonicalHeader(header: EpochHeader): EpochHeader {` |
| `deriveEventMessage` | `function` | [`packages/core/session/src/surface.ts:83`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L83) | `export function deriveEventMessage(event: SessionEvent): Message \| null {` |
| `systemTokens` | `let` | [`packages/llm/token-meter/src/breakdown-projection.ts:49`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/breakdown-projection.ts#L49) | `let systemTokens = state.systemTokens` |
| `toolsTokens` | `let` | [`packages/llm/token-meter/src/breakdown-projection.ts:50`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/breakdown-projection.ts#L50) | `let toolsTokens = state.toolsTokens` |
| `foldSurfaceTokens` | `function` | [`packages/llm/token-meter/src/surface-fold.ts:42`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/surface-fold.ts#L42) | `export function foldSurfaceTokens(` |
| `pressureTokens` | `const` | [`packages/llm/token-meter/src/usage-projection.ts:184`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/usage-projection.ts#L184) | `const pressureTokens = pressureFrom(usage)` |

### Tests and executable evidence

- [`packages/core/session/tests/derived-cache.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/derived-cache.spec.ts) — A test under the owning area exercises or imports `deriveEventMessage`.
- [`packages/llm/token-meter/tests/token-meter.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/tests/token-meter.spec.ts) — A test under the owning area exercises or imports `dsh-token-meter`. A test under the owning area exercises or imports `canonicalHeader`.
- [`packages/core/session/tests/request-header.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/request-header.spec.ts) — A test under the owning area exercises or imports `canonicalHeader`.
- [`packages/client/connection/tests/fixture.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/tests/fixture.client.spec.ts) — A test under the owning area exercises or imports `messageTokens`.
- [`packages/llm/token-meter/tests/token-usage-projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/tests/token-usage-projection.spec.ts) — A test under the owning area exercises or imports `contextPressure`. A test under the owning area exercises or imports `dsh-token-meter`.
- [`packages/client/ui-conversation/tests/input-bar.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-bar.client.spec.tsx) — A test under the owning area exercises or imports `measure`.
- [`packages/client/ui-conversation/tests/chat-stats.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-stats.client.spec.tsx) — A test under the owning area exercises or imports `contextPressure`. A test under the owning area exercises or imports `ContextMeter`.
- [`packages/client/ui-conversation/tests/context-meter.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/context-meter.client.spec.tsx) — A test under the owning area exercises or imports `contextPressure`. A test under the owning area exercises or imports `contextBreakdown`.

## How to read the implementation

1. Start with [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/ownership`, `concern/recovery`, `domain/context`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/projection`
- Aliases: `messageTokens`, `measure`, `ContextMeter`, `canonicalHeader`, `deriveEventMessage`, `systemTokens`, `toolsTokens`, `foldSurfaceTokens`, `pressureTokens`, `Context N% of X`, `contextPressure`, `dsh-session`, `dsh-token-meter`, `src/estimate.ts`
- Regex: `(?i)(messageTokens|measure|ContextMeter|canonicalHeader|deriveEventMessage|systemTokens|toolsTokens|foldSurfaceTokens)`

```bash
rg -n --pcre2 "(?i)(messageTokens|measure|ContextMeter|canonicalHeader|deriveEventMessage|systemTokens|toolsTokens|foldSurfaceTokens)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0343. the context meter could not see a compaction](0343-the-context-meter-could-not-see-a-compaction.md): The source note links to this decision directly.
- **`shares-code-with`** — [0076. Projected token usage and context occupancy](0076-projected-token-usage-and-context-occupancy.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/llm/token-meter/src/index.ts`.
- **`shares-code-with`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0316. The human transcript projects append-origin events](0316-the-human-transcript-projects-append-origin-events.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/surface.ts`.
- **`shares-code-with`** — [0025. Every LLM request is reconstructable from the session log](0025-every-llm-request-is-reconstructable-from-the-session-log.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0012. Session surface --- an ordered projection over the event log](0012-session-surface-an-ordered-projection-over-the-event-log.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0255-composer-context-meter-with-heuristic-composition-breakdown.md`.
