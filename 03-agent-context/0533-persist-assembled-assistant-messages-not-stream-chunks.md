---
id: "dsh-note-0533"
title: "Persist assembled assistant messages, not stream chunks"
status: "rejected"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/rejected/simplification/2026-06-20-assembled-assistant-messages-only.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/rejected"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "usage"
  - "seq"
  - "SessionEventMap"
  - "assistant/chunk"
  - "assistant/message"
  - "deriveMessages"
  - "tool/call"
  - "tool/result"
  - "session/load"
  - "llm-replay"
  - "Persist assembled assistant messages, not stream chunks"
  - "simplification"
  - "boundary"
  - "compatibility"
search_regex: "(?i)(usage|SessionEventMap|assistant/chunk|assistant/message|deriveMessages|tool/call|tool/result|session/load)"
---

# 0533. Persist assembled assistant messages, not stream chunks — implementation context

## Open this when

The canonical session log currently persists every assistant/chunk exactly as streamed by the model. The session persistence Agent Note chose this for token-level replay fidelity and contiguous seq, but the cost has grown: JSONL fixtures are dominated by tiny delta records, snapshot scenarios replay the model by grouping chunk events, ACP load reconstructs prior assistant output from chunks, and any future log reader must distinguish durable message history from token-level trace. For successful steps that assemble completed content, the loop already appends an assistant/message.

## Source decision

Stop storing assistant/chunk in the canonical session log. The durable log keeps assistant/message, tool/call, tool/result, usage if retained, and turn boundaries. Live UIs can still receive token deltas through a deliberately transient stream event. Snapshot replay should move its model script into an explicit fixture sidecar or derive it from a recorded adapter artifact, rather than treating the canonical user session as a token tape. Scenarios that need partial failed-stream output must record that output in the replay fixture.

## Decision status

Rejected design. Do not implement it as written. Reuse its failure analysis, constraints, and reversal condition.

- Raw note: [rejected/simplification/2026-06-20-assembled-assistant-messages-only.md](../02-notes/rejected/simplification/2026-06-20-assembled-assistant-messages-only.md)
- Pinned source: [.agents/notes/rejected/simplification/2026-06-20-assembled-assistant-messages-only.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/rejected/simplification/2026-06-20-assembled-assistant-messages-only.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/session/session-persistence/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/test-support/llm-replay/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`packages/test-support/llm-replay/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`packages/test-support/llm-replay`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Defines `SessionEventMap`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Defines `seq`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `usage`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/llm-replay/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/README.md) | package contract and examples | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`packages/test-support/llm-replay/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/package.json) | composition and configuration | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `assistant/chunk` named by the note. Contains the exact code literal `assistant/message` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) | package contract and examples | Contains the exact code literal `tool/call` named by the note. Contains the exact code literal `tool/result` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) | package contract and examples | Contains the exact code literal `assistant/chunk` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `usage` | `const` | [`packages/client/connection/src/client/fixture.ts:836`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L836) | `const usage = item.type === 'assistant/chunk' && item.data.chunk?.type === 'usage'` |
| `seq` | `const` | [`packages/core/session/src/index.ts:233`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L233) | `const seq = event['seq']` |
| `SessionEventMap` | `interface` | [`packages/core/tools/src/types.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts#L26) | `interface SessionEventMap {` |

### Tests and executable evidence

- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `SessionEventMap`.
- [`packages/test-support/llm-replay/tests/llm-replay.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/tests/llm-replay.spec.ts) — A test under the owning area exercises or imports `llm-replay`.
- [`scripts/session-fixture-layout.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/session-fixture-layout.spec.ts) — Contains the exact code literal `assistant/chunk` named by the note.
- Source verification intent: SessionEventMap drops assistant/chunk, or marks it as non-persisted if a transitional live event is needed. Session persistence docs no longer require every stream chunk to be stored verbatim. llm-replay and ACP snapshots use an explicit replay fixture format or sidecar for model chunks. session/load renders completed assistant messages from assistant/message. Stored logs get much smaller and remain seq-contiguous without chunk holes. The session format version and recorded fixtures are refreshed; non-current stored logs are rejected per the pre-release format policy.

## How to read the implementation

1. Start with [`packages/session/session-persistence/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/README.md) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Rejected design. Do not implement it as written. Reuse its failure analysis, constraints, and reversal condition.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/rejected`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `usage`, `seq`, `SessionEventMap`, `assistant/chunk`, `assistant/message`, `deriveMessages`, `tool/call`, `tool/result`, `session/load`, `llm-replay`, `Persist assembled assistant messages, not stream chunks`, `simplification`, `boundary`, `compatibility`
- Regex: `(?i)(usage|SessionEventMap|assistant/chunk|assistant/message|deriveMessages|tool/call|tool/result|session/load)`

```bash
rg -n --pcre2 "(?i)(usage|SessionEventMap|assistant/chunk|assistant/message|deriveMessages|tool/call|tool/result|session/load)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0009. Session persistence as an abstract service over the existing `SessionEvent`](0009-session-persistence-as-an-abstract-service-over-the-existing-sessioneven.md): The source note links to this decision directly.
- **`source-link`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): The source note links to this decision directly.
- **`shares-code-with`** — [0497. Persist the seed boundary so fork-child replay routes correctly](0497-persist-the-seed-boundary-so-fork-child-replay-routes-correctly.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/test-support/llm-replay/README.md`.
- **`shares-code-with`** — [0677. Use `session.jsonl` as the only snapshot session-log artifact](0677-use-session-jsonl-as-the-only-snapshot-session-log-artifact.md): Shares source implementation: `packages/test-support/llm-replay`, `packages/test-support/llm-replay/README.md`.
- **`shares-code-with`** — [0571. Auto-titled terminal from the first message](0571-auto-titled-terminal-from-the-first-message.md): Shares source implementation: `packages/test-support/llm-replay`, `packages/test-support/llm-replay/src/index.ts`.
- **`shares-code-with`** — [0683. Snapshot semantic terminal state for the TUI](0683-snapshot-semantic-terminal-state-for-the-tui.md): Shares source implementation: `packages/test-support/llm-replay`, `packages/test-support/llm-replay/src/index.ts`.
- **`shares-code-with`** — [0335. Goal-round wrap-up message](0335-goal-round-wrap-up-message.md): Shares source implementation: `packages/test-support/llm-replay`, `packages/test-support/llm-replay/src/index.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/tools/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0533-persist-assembled-assistant-messages-not-stream-chunks.md`.
