---
id: "dsh-note-0653"
title: "Stop mirroring the token stream as an agent event"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-07-02-remove-stream-chunk-mirror.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/simplification"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/streaming"
aliases:
  - "turn"
  - "StreamChunk"
  - "step"
  - "assistant/chunk"
  - "agent/stream-chunk"
  - "packages/core/agent-loop/src/agent.ts"
  - "assistant/chunk: { turn, step, chunk }"
  - "session/event"
  - "assistant/message"
  - "agent/steering"
  - "steering/message"
  - "agent/status"
  - "agent/error"
  - "agent/created"
search_regex: "(?i)(turn|StreamChunk|step|assistant/chunk|agent/stream\\-chunk|packages/core/agent\\-loop/src/agent\\.ts|assistant/chunk:[- ]\\{[- ]turn,[- ]step,[- ]chunk[- ]\\}|session/event)"
---

# 0653. Stop mirroring the token stream as an agent event — implementation context

## Open this when

The loop records every model token delta as a durable assistant/chunk session event AND emitted a parallel live agent/stream-chunk Cordis event carrying the identical data. In packages/core/agent-loop/src/agent.ts the two sat one line apart: Durable: assistant/chunk: { turn, step, chunk }. Live emit: agent/stream-chunk(agent, turn, step, chunk) --- same StreamChunk, same turn/step. The only thing the emit added over the session event was the live Agent handle, and the sole consumer discarded it (its handler signature was (_agent, _turn, _step, chunk)).

## Source decision

Remove agent/stream-chunk from the agent event taxonomy. The token stream is read off session/event as assistant/chunk, the same feed persistence and replay already use --- session/event is the single live transcript stream (assistant chunks, turn/step boundaries, tool activity, todos). Consumers. Persistence, replay, and interactive renderers consume the authoritative session stream directly. The automation-only ACP bridge emits committed assistant/message text rather than raw chunks, so it needs neither event. No production consumer requires an Agent-first token mirror.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-07-02-remove-stream-chunk-mirror.md](../02-notes/archived/simplification/2026-07-02-remove-stream-chunk-mirror.md)
- Pinned source: [.agents/notes/archived/simplification/2026-07-02-remove-stream-chunk-mirror.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-07-02-remove-stream-chunk-mirror.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/agent-loop/src/agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts) | runtime implementation | The source note names this file directly. Defines `turn`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/runtime-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/runtime-types.ts) | runtime implementation | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Defines `StreamChunk`, a construct named by the note. | `symbol-definition` |
| [`packages/lsp/lsp-stdio/src/framing.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/framing.ts) | runtime implementation | Defines `step`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/agent`. Contains the exact code literal `agent/disposed` named by the note. | `exact-code-occurrence, named-package-member` |
| [`packages/core/agent/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`docs/glossary.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/glossary.md) | package contract and examples | Contains the exact code literal `agent/session-start` named by the note. | `exact-code-occurrence` |
| [`docs/glossary.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/glossary.zh.md) | package contract and examples | Contains the exact code literal `agent/session-start` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `turn` | `const` | [`packages/core/agent-loop/src/agent.ts:204`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L204) | `const turn = this.phase.kind === 'running' ? this.phase.turn : this.phase.lastTurn` |
| `StreamChunk` | `type` | [`packages/llm/llm/src/types.ts:291`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L291) | `export type StreamChunk =` |
| `step` | `const` | [`packages/lsp/lsp-stdio/src/framing.ts:51`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/framing.ts#L51) | `const step = this.next()` |

### Tests and executable evidence

- [`scripts/session-fixture-layout.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/session-fixture-layout.spec.ts) — Contains the exact code literal `assistant/chunk` named by the note.
- [`packages/core/agent-loop/tests/resume.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/resume.spec.ts) — Contains the exact code literal `steering/message` named by the note.
- [`packages/preset/agent-presets/tests/mount.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/tests/mount.spec.ts) — Contains the exact code literal `agent/created` named by the note.
- [`packages/core/agent-loop/tests/scope-lifecycle.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/scope-lifecycle.spec.ts) — Contains the exact code literal `agent/error` named by the note.
- [`packages/core/agent-loop/tests/contract-regressions.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/contract-regressions.spec.ts) — Contains the exact code literal `agent/error` named by the note.
- [`packages/session/session-persistence/tests/coordinator-contract.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/tests/coordinator-contract.ts) — Contains the exact code literal `steering/message` named by the note.

## How to read the implementation

1. Start with [`packages/core/agent-loop/src/agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/simplification`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/streaming`
- Aliases: `turn`, `StreamChunk`, `step`, `assistant/chunk`, `agent/stream-chunk`, `packages/core/agent-loop/src/agent.ts`, `assistant/chunk: { turn, step, chunk }`, `session/event`, `assistant/message`, `agent/steering`, `steering/message`, `agent/status`, `agent/error`, `agent/created`
- Regex: `(?i)(turn|StreamChunk|step|assistant/chunk|agent/stream\-chunk|packages/core/agent\-loop/src/agent\.ts|assistant/chunk:[- ]\{[- ]turn,[- ]step,[- ]chunk[- ]\}|session/event)`

```bash
rg -n --pcre2 "(?i)(turn|StreamChunk|step|assistant/chunk|agent/stream\\-chunk|packages/core/agent\\-loop/src/agent\\.ts|assistant/chunk:[- ]\\{[- ]turn,[- ]step,[- ]chunk[- ]\\}|session/event)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0659. Remove the `agent/steering` mirror emit](0659-remove-the-agent-steering-mirror-emit.md): The source note links to this decision directly.
- **`source-link`** — [0533. Persist assembled assistant messages, not stream chunks](0533-persist-assembled-assistant-messages-not-stream-chunks.md): The source note links to this decision directly.
- **`shares-code-with`** — [0449. Stop mirroring durable boundaries as agent events](0449-stop-mirroring-durable-boundaries-as-agent-events.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/README.md`.
- **`shares-code-with`** — [0464. Request-error retry action](0464-request-error-retry-action.md): Shares source implementation: `packages/core/agent`, `packages/core/agent-loop/src/agent.ts`.
- **`shares-code-with`** — [0469. Keep agent routing private](0469-keep-agent-routing-private.md): Shares source implementation: `packages/core/agent`, `packages/core/agent-loop/src/agent.ts`.
- **`shares-code-with`** — [0104. Agent-scoped events dispatch a single payload object](0104-agent-scoped-events-dispatch-a-single-payload-object.md): Shares source implementation: `packages/core/agent`, `packages/core/agent-loop/src/agent.ts`.
- **`shares-code-with`** — [0334. Reject human interaction from runtime-owned subagents](0334-reject-human-interaction-from-runtime-owned-subagents.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/README.md`.
- **`shares-code-with`** — [0004. Microkernel --- extension via Cordis event taxonomy, one concrete loop](0004-microkernel-extension-via-cordis-event-taxonomy-one-concrete-loop.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0653-stop-mirroring-the-token-stream-as-an-agent-event.md`.
