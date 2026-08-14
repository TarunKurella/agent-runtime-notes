---
id: "dsh-note-0065"
title: "Make packed chunk rows the default JSONL layout"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-26-packed-chunk-rows-by-default.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/streaming"
aliases:
  - "packChunkRuns"
  - "events"
  - "decodeStorageRecord"
  - "SessionEventMap"
  - "SessionEvent"
  - "assistant/chunk"
  - "session/event"
  - "sourceEventSeqs"
  - "text-chunks"
  - "reasoning-chunks"
  - "tool-call-chunks"
  - "dsh-session-persistence-jsonl"
  - "packChunks"
  - "packChunks: false"
search_regex: "(?i)(packChunkRuns|events|decodeStorageRecord|SessionEventMap|SessionEvent|assistant/chunk|session/event|sourceEventSeqs)"
---

# 0065. Make packed chunk rows the default JSONL layout — implementation context

## Open this when

Provider streams produce many token-sized assistant/chunk delta events whose repeated JSON envelopes can outweigh their payloads. The session log must retain each chunk as a distinct logical event: live session/event delivery, sequence numbers, sourceEventSeqs, replay, cancellation evidence, and UI streaming all depend on those boundaries. The JSONL storage seam can reduce that envelope cost without changing the logical log.

## Source decision

dsh-session-persistence-jsonl resolves an omitted packChunks to true. The ACP demo wrapper exposes the same default, and every composition that omits the field inherits packed writes. packChunks: false remains an explicit write-side diagnostic mode that stores one event per line. Reading is unconditional and layout-blind. Packed, unpacked, and mixed files load into the same contiguous SessionEvent[], so the default does not require a session-format version change or an on-disk runtime migration. The option controls newly appended batches only; it never selects a reader mode.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-26-packed-chunk-rows-by-default.md](../02-notes/implemented/architecture/2026-07-26-packed-chunk-rows-by-default.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-26-packed-chunk-rows-by-default.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-26-packed-chunk-rows-by-default.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/migrate-packed-session-fixtures.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/migrate-packed-session-fixtures.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/session-fixture-layout.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/session-fixture-layout.snapshot.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. Defines `events`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. Defines `SessionEventMap`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/chunk-rows.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `packChunkRuns`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session/session-persistence-jsonl/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session/session-persistence-jsonl`. | `named-package-member` |
| [`packages/session/session-persistence-jsonl/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-persistence-jsonl`. | `named-package-member` |
| [`apps/web/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `apps/web`. | `named-directory-member` |
| [`apps/web`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/web) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/core/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/session) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session/session-persistence-jsonl`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `packChunkRuns` | `function` | [`packages/core/session/src/chunk-rows.ts:192`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts#L192) | `export function packChunkRuns(events: readonly SessionEvent[]): StorageRecord[] {` |
| `events` | `const` | [`packages/core/session/src/chunk-rows.ts:295`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts#L295) | `const events: SessionEvent[] = []` |
| `decodeStorageRecord` | `function` | [`packages/core/session/src/chunk-rows.ts:339`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts#L339) | `export function decodeStorageRecord(value: unknown): SessionEvent[] {` |
| `events` | `const` | [`packages/core/session/src/index.ts:1098`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L1098) | `const events = session.events` |
| `SessionEventMap` | `interface` | [`packages/core/session/src/types.ts:236`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L236) | `export interface SessionEventMap {` |
| `SessionEvent` | `type` | [`packages/core/session/src/types.ts:404`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L404) | `export type SessionEvent<T extends SessionEventType = SessionEventType> = {` |

### Tests and executable evidence

- [`packages/core/session/tests/fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/fork.spec.ts) — A test under the owning area exercises or imports `SessionEventMap`.
- [`packages/core/session/tests/chunk-rows.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/chunk-rows.spec.ts) — A test under the owning area exercises or imports `text-chunks`. A test under the owning area exercises or imports `reasoning-chunks`.
- [`packages/core/session/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/properties.spec.ts) — A test under the owning area exercises or imports `SessionEventMap`.
- [`packages/core/session/tests/gen-persistence-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/gen-persistence-catalog.spec.ts) — A test under the owning area exercises or imports `SessionEventMap`.
- [`packages/session/session-persistence-jsonl/tests/zstd.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/tests/zstd.spec.ts) — A test under the owning area exercises or imports `text-chunks`.
- [`packages/session/session-persistence-jsonl/tests/jsonl.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/tests/jsonl.spec.ts) — A test under the owning area exercises or imports `text-chunks`. A test under the owning area exercises or imports `packChunks`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — Contains the exact code literal `dsh-session-persistence-jsonl` named by the note.
- [`scripts/release/families.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/release/families.spec.ts) — Contains the exact code literal `apps/web` named by the note.

## How to read the implementation

1. Start with [`scripts/migrate-packed-session-fixtures.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/migrate-packed-session-fixtures.ts) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/streaming`
- Aliases: `packChunkRuns`, `events`, `decodeStorageRecord`, `SessionEventMap`, `SessionEvent`, `assistant/chunk`, `session/event`, `sourceEventSeqs`, `text-chunks`, `reasoning-chunks`, `tool-call-chunks`, `dsh-session-persistence-jsonl`, `packChunks`, `packChunks: false`
- Regex: `(?i)(packChunkRuns|events|decodeStorageRecord|SessionEventMap|SessionEvent|assistant/chunk|session/event|sourceEventSeqs)`

```bash
rg -n --pcre2 "(?i)(packChunkRuns|events|decodeStorageRecord|SessionEventMap|SessionEvent|assistant/chunk|session/event|sourceEventSeqs)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0526. Remove the packed-session fixture branch migrator](0526-remove-the-packed-session-fixture-branch-migrator.md): The source note links to this decision directly.
- **`shares-code-with`** — [0518. Recallable compaction --- index checkpoints, a state checkpoint, and in-session history recall](0518-recallable-compaction-index-checkpoints-a-state-checkpoint-and-in-sessio.md): Shares source implementation: `packages/core/session/src/chunk-rows.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0110. Bounded session persistence write batching](0110-bounded-session-persistence-write-batching.md): Shares source implementation: `packages/core/session/src/chunk-rows.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0009. Session persistence as an abstract service over the existing `SessionEvent`](0009-session-persistence-as-an-abstract-service-over-the-existing-sessioneven.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0013. Shared persistence write coordinator](0013-shared-persistence-write-coordinator.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0012. Session surface --- an ordered projection over the event log](0012-session-surface-an-ordered-projection-over-the-event-log.md): Shares source implementation: `packages/core/session/src/chunk-rows.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0446. Drop the mutable session summary](0446-drop-the-mutable-session-summary.md): Shares source implementation: `packages/core/session/src/chunk-rows.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0466. Remove synthetic turns for log-only events](0466-remove-synthetic-turns-for-log-only-events.md): Shares source implementation: `packages/core/session/src/chunk-rows.ts`, `packages/core/session/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0065-make-packed-chunk-rows-the-default-jsonl-layout.md`.
