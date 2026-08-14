---
id: "dsh-note-0003"
title: "Event-sourced sessions with derived message history"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-06-11-event-sourced-sessions.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "SessionEvent"
  - "deriveMessages"
  - "assistant/message"
  - "session/event"
  - "session/flush"
  - "agent/pre-step"
  - "step/start"
  - "user/message"
  - "Event-sourced sessions with derived message history"
  - "architecture"
  - "boundary"
  - "lifecycle"
  - "ownership"
  - "performance"
search_regex: "(?i)(SessionEvent|deriveMessages|assistant/message|session/event|session/flush|agent/pre\\-step|step/start|user/message)"
---

# 0003. Event-sourced sessions with derived message history — implementation context

## Open this when

The MVP requires strict event-based tracing with fully replayable sessions (严格的基于事件的trace、logging系统，session完全可回放).

## Source decision

A Session is an append-only log of typed SessionEvents --- the single source of truth. The LLM message history is derived from the log (deriveMessages()); raw stream chunks are logged for token-level replay fidelity while the assembled assistant/message event is authoritative for derivation. Replay/fork = seed a new session with an existing log. Appends are synchronous (the hot path never blocks on I/O); session/event is a sync notification; persistence plugins buffer write-behind and drain at the awaited session/flush checkpoint fired at every turn end.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-06-11-event-sourced-sessions.md](../02-notes/implemented/architecture/2026-06-11-event-sourced-sessions.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-06-11-event-sourced-sessions.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-06-11-event-sourced-sessions.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. Defines `SessionEvent`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/compaction/compaction/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/index.ts) | package entry point | Core file in the package named by the note: `packages/compaction/compaction`. | `named-package-member` |
| [`packages/compaction/compaction/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/compaction/compaction`. | `named-package-member` |
| [`packages/compaction/compaction/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/compaction/compaction`. | `named-package-member` |
| [`packages/core/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/session) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/compaction/compaction`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/session/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/compaction/compaction/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/README.md) | package contract and examples | Core file in the package named by the note: `packages/compaction/compaction`. | `named-package-member` |
| [`packages/compaction/compaction/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/package.json) | composition and configuration | Core file in the package named by the note: `packages/compaction/compaction`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `SessionEvent` | `type` | [`packages/core/session/src/types.ts:404`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L404) | `export type SessionEvent<T extends SessionEventType = SessionEventType> = {` |

### Tests and executable evidence

- [`packages/core/session/tests/fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/fork.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/session/tests/surface.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/surface.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/session/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/properties.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/session/tests/derived-cache.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/derived-cache.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`apps/cli/tests/fixtures/dsh-badge/snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/dsh-badge/snapshot.ts) — Contains the exact code literal `agent/pre-step` named by the note.

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

- Tags: `class/architecture`, `concern/boundary`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/schema-types`, `domain/agent-loop`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `SessionEvent`, `deriveMessages`, `assistant/message`, `session/event`, `session/flush`, `agent/pre-step`, `step/start`, `user/message`, `Event-sourced sessions with derived message history`, `architecture`, `boundary`, `lifecycle`, `ownership`, `performance`
- Regex: `(?i)(SessionEvent|deriveMessages|assistant/message|session/event|session/flush|agent/pre\-step|step/start|user/message)`

```bash
rg -n --pcre2 "(?i)(SessionEvent|deriveMessages|assistant/message|session/event|session/flush|agent/pre\\-step|step/start|user/message)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0009. Session persistence as an abstract service over the existing `SessionEvent`](0009-session-persistence-as-an-abstract-service-over-the-existing-sessioneven.md): The source note links to this decision directly.
- **`shares-code-with`** — [0518. Recallable compaction --- index checkpoints, a state checkpoint, and in-session history recall](0518-recallable-compaction-index-checkpoints-a-state-checkpoint-and-in-sessio.md): Shares source implementation: `packages/compaction/compaction/src/index.ts`, `packages/compaction/compaction/src/invariant.ts`.
- **`shares-code-with`** — [0012. Session surface --- an ordered projection over the event log](0012-session-surface-an-ordered-projection-over-the-event-log.md): Shares source implementation: `packages/compaction/compaction/src/index.ts`, `packages/compaction/compaction/src/invariant.ts`.
- **`shares-code-with`** — [0168. Cross-session references](0168-cross-session-references.md): Shares source implementation: `packages/compaction/compaction/src/index.ts`, `packages/compaction/compaction/src/invariant.ts`.
- **`shares-code-with`** — [0466. Remove synthetic turns for log-only events](0466-remove-synthetic-turns-for-log-only-events.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0102. Reusable Session preparation before publication](0102-reusable-session-preparation-before-publication.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0316. The human transcript projects append-origin events](0316-the-human-transcript-projects-append-origin-events.md): Shares source implementation: `packages/compaction/compaction/src/index.ts`, `packages/compaction/compaction/src/invariant.ts`.
- **`shares-code-with`** — [0447. Fold trace-only session facts into load-bearing events](0447-fold-trace-only-session-facts-into-load-bearing-events.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0003-event-sourced-sessions-with-derived-message-history.md`.
