---
id: "dsh-note-0447"
title: "Fold trace-only session facts into load-bearing events"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-06-20-collapse-trace-only-session-events.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "usage"
  - "SessionEventMap"
  - "turn/end { kind: 'error', message, code }"
  - "assistant/message"
  - "assistant/message { turn, step, content, usage? }"
  - "assistant/message { content: [], usage }"
  - "deriveMessages"
  - "turn/end.reason"
  - "turn/end"
  - "agent/error"
  - "turn/end { kind: 'error', step, message, code? }"
  - "(backends reject any non-"
  - "and rejects any other version without migration."
  - "assistant/message.usage"
search_regex: "(?i)(usage|SessionEventMap|turn/end[- ]\\{[- ]kind:[- ]'error',[- ]message,[- ]code[- ]\\}|assistant/message|assistant/message[- ]\\{[- ]turn,[- ]step,[- ]content,[- ]usage\\?[- ]\\}|assistant/message[- ]\\{[- ]content:[- ]\\[\\],[- ]usage[- ]\\}|deriveMessages|turn/end\\.reason)"
---

# 0447. Fold trace-only session facts into load-bearing events — implementation context

## Open this when

The session event vocabulary includes first-class events that are not part of replayable conversation history and have little or no production consumption. usage is already present as a model stream chunk before the loop also appends a separate usage event. error duplicates the turn/end { kind: 'error', message, code } reason for loop failures; ACP settlement reads the turn-end reason, while message and UI projections skip the standalone error event. These events make the canonical transcript look more useful as telemetry than it currently is.

## Source decision

Standalone trace-only events are removed exactly where their information is preserved without a parallel record: Successful-step usage folds into the matching assistant/message (assistant/message { turn, step, content, usage? }), so the assembled model output and its accounting travel together. A failed or aborted step that has usage but no assistant content carries the usage on an empty-content assistant/message { content: [], usage } --- no persisted usage chunk goes unrepresented. The no-information-loss case is the max-tokens path: a step cut off with usage but empty content (e.g.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-06-20-collapse-trace-only-session-events.md](../02-notes/implemented/simplification/2026-06-20-collapse-trace-only-session-events.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-06-20-collapse-trace-only-session-events.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-06-20-collapse-trace-only-session-events.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. Defines `SessionEventMap`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/session) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `usage`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/session`. Contains the exact code literal `assistant/message.usage` named by the note. | `exact-code-occurrence, named-package-member` |
| [`packages/core/session/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `assistant/message` named by the note. Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `assistant/message` named by the note. Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `assistant/message` named by the note. Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/subsystems/core.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/core.md) | package contract and examples | Contains the exact code literal `agent/error` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `usage` | `const` | [`packages/client/connection/src/client/fixture.ts:836`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L836) | `const usage = item.type === 'assistant/chunk' && item.data.chunk?.type === 'usage'` |
| `SessionEventMap` | `interface` | [`packages/core/session/src/types.ts:236`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L236) | `export interface SessionEventMap {` |

### Tests and executable evidence

- [`packages/core/session/tests/fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/fork.spec.ts) — A test under the owning area exercises or imports `deriveMessages`. A test under the owning area exercises or imports `SessionEventMap`.
- [`packages/core/session/tests/surface.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/surface.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/session/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/properties.spec.ts) — A test under the owning area exercises or imports `deriveMessages`. A test under the owning area exercises or imports `SessionEventMap`.
- [`packages/core/session/tests/derived-cache.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/derived-cache.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/session/tests/gen-persistence-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/gen-persistence-catalog.spec.ts) — A test under the owning area exercises or imports `SessionEventMap`.
- [`packages/core/agent-loop/tests/scope-lifecycle.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/scope-lifecycle.spec.ts) — Contains the exact code literal `agent/error` named by the note.
- [`packages/core/agent-loop/tests/contract-regressions.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/contract-regressions.spec.ts) — Contains the exact code literal `agent/error` named by the note.
- Source verification intent: SessionEventMap carries no standalone usage or error; the loop appends no separate usage event and records durable failures through turn/end { kind: 'error', step, message, code? }; ACP snapshots and persistence tests assert no trace-only lines; recorded fixtures are on the new event shape with the session format version pinned at 0 (backends reject any non-0 stored log per the pre-release format policy); and the docs state where token usage and operational errors are observed.

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

- Tags: `class/simplification`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/agent-loop`, `domain/build-release`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `usage`, `SessionEventMap`, `turn/end { kind: 'error', message, code }`, `assistant/message`, `assistant/message { turn, step, content, usage? }`, `assistant/message { content: [], usage }`, `deriveMessages`, `turn/end.reason`, `turn/end`, `agent/error`, `turn/end { kind: 'error', step, message, code? }`, `(backends reject any non-`, `and rejects any other version without migration.`, `assistant/message.usage`
- Regex: `(?i)(usage|SessionEventMap|turn/end[- ]\{[- ]kind:[- ]'error',[- ]message,[- ]code[- ]\}|assistant/message|assistant/message[- ]\{[- ]turn,[- ]step,[- ]content,[- ]usage\?[- ]\}|assistant/message[- ]\{[- ]content:[- ]\[\],[- ]usage[- ]\}|deriveMessages|turn/end\.reason)`

```bash
rg -n --pcre2 "(?i)(usage|SessionEventMap|turn/end[- ]\\{[- ]kind:[- ]'error',[- ]message,[- ]code[- ]\\}|assistant/message|assistant/message[- ]\\{[- ]turn,[- ]step,[- ]content,[- ]usage\\?[- ]\\}|assistant/message[- ]\\{[- ]content:[- ]\\[\\],[- ]usage[- ]\\}|deriveMessages|turn/end\\.reason)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0530. Deterministic tests, the replay invariant fixture, and race stress](0530-deterministic-tests-the-replay-invariant-fixture-and-race-stress.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0635. Generated persistence log event catalog](0635-generated-persistence-log-event-catalog.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0311. Load sessions persisted before message identity](0311-load-sessions-persisted-before-message-identity.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0466. Remove synthetic turns for log-only events](0466-remove-synthetic-turns-for-log-only-events.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0504. Frame-coalesced reasoning-chunk publication and browser stress validation](0504-frame-coalesced-reasoning-chunk-publication-and-browser-stress-validatio.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/core/session`.
- **`shares-code-with`** — [0102. Reusable Session preparation before publication](0102-reusable-session-preparation-before-publication.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0665. Use one surface manager per session](0665-use-one-surface-manager-per-session.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0171. Default local instruction overlay](0171-default-local-instruction-overlay.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0447-fold-trace-only-session-facts-into-load-bearing-events.md`.
