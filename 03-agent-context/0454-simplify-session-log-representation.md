---
id: "dsh-note-0454"
title: "Simplify session-log representation"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-07-12-simplify-session-log-representation.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/context"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
aliases:
  - "change"
  - "fallback"
  - "prev"
  - "nodes"
  - "SessionStartSource"
  - "indexOf"
  - "foldSurface"
  - "SurfaceManager"
  - "SESSION_FORMAT_VERSION"
  - "next"
  - "request/header"
  - "SystemDelta"
  - "ToolsDelta"
  - "request/header-delta"
search_regex: "(?i)(change|fallback|prev|nodes|SessionStartSource|indexOf|foldSurface|SurfaceManager)"
---

# 0454. Simplify session-log representation — implementation context

## Open this when

The session log maintains two representations that cost more machinery than their consumers require: a pseudo-linked surface and custom request-header deltas. SurfaceManager stores the same order in an array, a seq map, and mutable prev/next links. Production never reads either link: compact's tool-pairing balance answers from per-cut balances cached in surface order. Replacement already uses indexOf, so the links do not make its dominant operation constant-time. A seq array with linear replacement lookup has the same asymptotic replacement cost and one representation to validate.

## Source decision

SurfaceManager.nodes is a readonly number[] of event sequences; the public SurfaceNode shape, node links, and seq-to-node map are removed. The internal replace-generation signal remains. The complete foldSurface() read used by session-query returns the same number-array representation plus replacement metadata without making the incremental manager retain history. Tool-pairing balance and compaction use event sequences and surface positions; the compact-owned per-cut balance cache does not depend on node links. Request headers use canonical full snapshots only.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-07-12-simplify-session-log-representation.md](../02-notes/implemented/simplification/2026-07-12-simplify-session-log-representation.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-07-12-simplify-session-log-representation.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-07-12-simplify-session-log-representation.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Defines `nodes`, a construct named by the note. Contains the exact code literal `tool/call` named by the note. | `exact-code-occurrence, symbol-definition` |
| [`vendor/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts) | package entry point | Defines `next`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts) | runtime implementation | Defines `next`, a construct named by the note. | `symbol-definition` |
| [`vendor/cosmokit/src/string.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cosmokit/src/string.ts) | runtime implementation | Defines `next`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `next`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `next`, a construct named by the note. Defines `change`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts) | package entry point | Defines `change`, a construct named by the note. | `symbol-definition` |
| [`vendor/schemastery/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/schemastery/src/index.ts) | package entry point | Defines `fallback`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Defines `nodes`, a construct named by the note. Contains the exact code literal `request/header-delta` named by the note. | `exact-code-occurrence, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Defines `SESSION_FORMAT_VERSION`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts) | package entry point | Defines `fallback`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/surface.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts) | runtime implementation | Defines `SurfaceManager`, a construct named by the note. Defines `foldSurface`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `change` | `const` | [`packages/client/connection/src/client/fixture.ts:1408`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1408) | `const change = event.data` |
| `fallback` | `const` | [`packages/client/modules/src/index.ts:138`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L138) | `const fallback = (client as Record<string, unknown>).default` |
| `prev` | `const` | [`packages/client/runtime/src/client/sessions/partial.ts:56`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/partial.ts#L56) | `const prev = this.blocks[chunk.index]` |
| `prev` | `const` | [`packages/client/runtime/src/client/sessions/partial.ts:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/partial.ts#L62) | `const prev = this.blocks[chunk.index]` |
| `prev` | `const` | [`packages/client/runtime/src/client/sessions/partial.ts:68`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/partial.ts#L68) | `const prev = this.blocks[chunk.index]` |
| `prev` | `const` | [`packages/client/ui-input-trigger/src/core/detect.ts:21`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-input-trigger/src/core/detect.ts#L21) | `const prev = draft.charAt(index - 1)` |
| `nodes` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryView.tsx:147`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryView.tsx#L147) | `const nodes = inspection.eventNodes` |
| `nodes` | `const` | [`packages/compaction/compaction-basic/src/region.ts:316`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/region.ts#L316) | `const nodes = session.surface.nodes` |
| `SessionStartSource` | `type` | [`packages/core/agent/src/runtime-types.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/runtime-types.ts#L61) | `export type SessionStartSource = 'startup' \| 'resume' \| 'clear' \| 'compact'` |
| `indexOf` | `function` | [`packages/core/session/src/chunk-rows.ts:131`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts#L131) | `function indexOf(event: DeltaEvent): number {` |
| `nodes` | `const` | [`packages/core/session/src/index.ts:728`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L728) | `const nodes = surface.nodes` |
| `foldSurface` | `function` | [`packages/core/session/src/surface.ts:387`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L387) | `export function foldSurface(events: readonly SessionEvent[]): SurfaceFoldResult {` |
| `SurfaceManager` | `class` | [`packages/core/session/src/surface.ts:398`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L398) | `export class SurfaceManager implements SessionSurface {` |
| `SESSION_FORMAT_VERSION` | `const` | [`packages/core/session/src/types.ts:56`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L56) | `export const SESSION_FORMAT_VERSION = 0` |
| `next` | `const` | [`packages/goal/goal/src/fold.ts:205`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L205) | `const next = change.goal` |
| `change` | `const` | [`packages/goal/goal/src/fold.ts:315`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L315) | `const change = decodeGoalChange(event.data)` |

### Tests and executable evidence

- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — A test under the owning area exercises or imports `sourceEventSeqs`.
- [`apps/web/tests/produced-files.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/produced-files.e2e.ts) — A test under the owning area exercises or imports `sourceEventSeqs`.
- [`apps/web/tests/chat-scroll-fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/chat-scroll-fixture.ts) — A test under the owning area exercises or imports `sourceEventSeqs`.
- [`apps/web/tests/message-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/message-actions.e2e.ts) — A test under the owning area exercises or imports `sourceEventSeqs`.
- [`apps/web/tests/complex-history.perf.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/complex-history.perf.ts) — A test under the owning area exercises or imports `sourceEventSeqs`.
- [`apps/web/tests/stats-paged-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/stats-paged-history.e2e.ts) — A test under the owning area exercises or imports `sourceEventSeqs`.
- [`packages/core/session/tests/surface.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/surface.spec.ts) — A test under the owning area exercises or imports `SurfaceManager`. A test under the owning area exercises or imports `foldSurface`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `SESSION_FORMAT_VERSION`.
- Source verification intent: Unit coverage pins ordered-surface append/replace behavior, tool pairing, compaction, full-header folding/logging, request reconstruction, and dev invariants. Seed validation plus JSONL and SQLite load tests reject the legacy event before replay. The keyless ACP suite exercises record, refresh, replay, changed-header pinning, and the sandbox mode-switch fixture in the new shape.

## How to read the implementation

1. Start with [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/context`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/generation`
- Aliases: `change`, `fallback`, `prev`, `nodes`, `SessionStartSource`, `indexOf`, `foldSurface`, `SurfaceManager`, `SESSION_FORMAT_VERSION`, `next`, `request/header`, `SystemDelta`, `ToolsDelta`, `request/header-delta`
- Regex: `(?i)(change|fallback|prev|nodes|SessionStartSource|indexOf|foldSurface|SurfaceManager)`

```bash
rg -n --pcre2 "(?i)(change|fallback|prev|nodes|SessionStartSource|indexOf|foldSurface|SurfaceManager)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0088. Claim inbox input before one pre-step decision](0088-claim-inbox-input-before-one-pre-step-decision.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0078. Terminal LLM stream failures](0078-terminal-llm-stream-failures.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0058. Separate context injection from turn execution](0058-separate-context-injection-from-turn-execution.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0231. Permission Settings default for new sessions](0231-permission-settings-default-for-new-sessions.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0634. JSDoc completeness gate for the cordis surface](0634-jsdoc-completeness-gate-for-the-cordis-surface.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0567. Dedicated full-screen TUI front door](0567-dedicated-full-screen-tui-front-door.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/llm/llm/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0454-simplify-session-log-representation.md`.
