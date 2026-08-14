---
id: "dsh-note-0013"
title: "Shared persistence write coordinator"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-06-18-shared-persistence-write-coordinator.md"
implementation_evidence: "medium"
target_anchor: "append-only event and session store"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/extensions"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/generation"
aliases:
  - "interruptedTurnClosers"
  - "closers"
  - "inspect"
  - "list"
  - "PersistenceBackend"
  - "PersistenceCoordinator"
  - "SessionPersistence"
  - "append"
  - "materialize"
  - "dsh-session-persistence-jsonl"
  - "dsh-session-persistence-sqlite"
  - "session/created"
  - "dsh-session-persistence"
  - "new PersistenceCoordinator"
search_regex: "(?i)(interruptedTurnClosers|closers|inspect|list|PersistenceBackend|PersistenceCoordinator|SessionPersistence|append)"
---

# 0013. Shared persistence write coordinator — implementation context

## Open this when

dsh-session-persistence-jsonl and dsh-session-persistence-sqlite intentionally prove the same SessionPersistence contract over different storage media, but their write-path orchestration was duplicated: per-session state, session/created adoption, backend-specific prefix reads, write-behind control, per-id operation serialization, HMR seeding, and dispose drains. The pure seed-prefix collision and serializability guards had already moved into the Service Definition package; the remaining orchestration was still correctness-heavy and received the same fixes twice. Only the storage primitives (write bytes vs.

## Source decision

Extract a backend-agnostic PersistenceCoordinator into dsh-session-persistence. The coordinator owns the orchestration once; each first-party backend composes one (new PersistenceCoordinator(ctx, this)), implements a small PersistenceBackend hook interface, and delegates its stateful public methods (create/append/prepare/load/inspect/readFrom) to it. Backend-owned metadata and revision listing bypass the coordinator. Composition, not inheritance. The coordinator is a concrete class the backend holds, not a base class the backend extends.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-06-18-shared-persistence-write-coordinator.md](../02-notes/implemented/architecture/2026-06-18-shared-persistence-write-coordinator.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-06-18-shared-persistence-write-coordinator.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-06-18-shared-persistence-write-coordinator.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/repair.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `closers`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/session/session-persistence/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session/session-persistence`. Defines `SessionPersistence`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session/session-persistence/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-persistence`. | `named-package-member` |
| [`packages/session/session-persistence/src/coordinator.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/coordinator.ts) | runtime implementation | Core file in the package named by the note: `packages/session/session-persistence`. Defines `PersistenceCoordinator`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session/session-persistence-jsonl/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session/session-persistence-jsonl`. | `named-package-member` |
| [`packages/session/session-persistence-sqlite/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-sqlite/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session/session-persistence-sqlite`. | `named-package-member` |
| [`packages/session/session-persistence-jsonl/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-persistence-jsonl`. | `named-package-member` |
| [`packages/session/session-persistence-sqlite/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-sqlite/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-persistence-sqlite`. | `named-package-member` |
| [`packages/core/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/session) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `interruptedTurnClosers` | `function` | [`packages/core/session/src/repair.ts:27`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts#L27) | `export function interruptedTurnClosers(events: readonly SessionEvent[]): SessionEvent[] {` |
| `closers` | `const` | [`packages/core/session/src/repair.ts:87`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts#L87) | `const closers: SessionEvent[] = []` |
| `inspect` | `const` | [`packages/extensions/cordis-client-runner/src/client/index.ts:189`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-client-runner/src/client/index.ts#L189) | `const inspect = new ClientCordisInspectRegistry({` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `PersistenceBackend` | `interface` | [`packages/session/session-persistence/src/coordinator.ts:127`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/coordinator.ts#L127) | `export interface PersistenceBackend<TornMarker = unknown> {` |
| `PersistenceCoordinator` | `class` | [`packages/session/session-persistence/src/coordinator.ts:588`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/coordinator.ts#L588) | `export class PersistenceCoordinator<TornMarker = unknown> {` |
| `closers` | `const` | [`packages/session/session-persistence/src/coordinator.ts:903`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/coordinator.ts#L903) | `const closers = interruptedTurnClosers(storedEvents).map(adoptSessionEvent)` |
| `SessionPersistence` | `class` | [`packages/session/session-persistence/src/index.ts:84`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/index.ts#L84) | `export abstract class SessionPersistence extends Service {` |
| `append` | `const` | [`packages/workflow/tool-workflow/src/index.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/index.ts#L75) | `const append = <Type extends keyof ToolWorkflowRecordEventMap>(` |
| `materialize` | `function` | [`packages/workflow/workflow-worker-thread/src/realm.ts:78`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow-worker-thread/src/realm.ts#L78) | `function materialize(value: unknown, path: string, seen: Set<object>): unknown {` |

### Tests and executable evidence

- [`packages/core/session/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/scoped.spec.ts) — A test under the owning area exercises or imports `prepare`.
- [`packages/core/session/tests/repair.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/repair.spec.ts) — A test under the owning area exercises or imports `closers`. A test under the owning area exercises or imports `interruptedTurnClosers`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `prepare`.
- [`packages/session/session-persistence/tests/contract.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/tests/contract.ts) — A test under the owning area exercises or imports `SessionPersistence`. A test under the owning area exercises or imports `closers`.
- [`packages/session/session-persistence-jsonl/tests/zstd.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/tests/zstd.spec.ts) — A test under the owning area exercises or imports `loadStored`. A test under the owning area exercises or imports `closers`.
- [`packages/session/session-persistence/tests/persistence.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/tests/persistence.spec.ts) — A test under the owning area exercises or imports `SessionPersistence`. A test under the owning area exercises or imports `PersistenceCoordinator`.
- [`packages/session/session-persistence-jsonl/tests/jsonl.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/tests/jsonl.spec.ts) — A test under the owning area exercises or imports `loadStored`. A test under the owning area exercises or imports `materialize`.
- [`packages/session/session-persistence-sqlite/tests/sqlite.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-sqlite/tests/sqlite.spec.ts) — A test under the owning area exercises or imports `dsh-session-persistence-sqlite`. A test under the owning area exercises or imports `loadStored`.
- Source verification intent: The shared runPersistenceContract (public-API contract) runs for every backend and proves that inspect balances an interrupted logical view without changing storage or revisions before prepare or load commits recovery. runCoordinatorContract (tests/coordinator-contract.ts) covers adoption, HMR, collision, session and backend disposal drains, and crash-tail repair through an in-memory reference, JSONL, and SQLite.

## How to read the implementation

1. Start with [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** append-only event and session store.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/simplification`, `domain/extensions`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/generation`
- Aliases: `interruptedTurnClosers`, `closers`, `inspect`, `list`, `PersistenceBackend`, `PersistenceCoordinator`, `SessionPersistence`, `append`, `materialize`, `dsh-session-persistence-jsonl`, `dsh-session-persistence-sqlite`, `session/created`, `dsh-session-persistence`, `new PersistenceCoordinator`
- Regex: `(?i)(interruptedTurnClosers|closers|inspect|list|PersistenceBackend|PersistenceCoordinator|SessionPersistence|append)`

```bash
rg -n --pcre2 "(?i)(interruptedTurnClosers|closers|inspect|list|PersistenceBackend|PersistenceCoordinator|SessionPersistence|append)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0102. Reusable Session preparation before publication](0102-reusable-session-preparation-before-publication.md): The source note links to this decision directly.
- **`source-link`** — [0110. Bounded session persistence write batching](0110-bounded-session-persistence-write-batching.md): The source note links to this decision directly.
- **`source-link`** — [0460. Collapse live persistence into one flush controller](0460-collapse-live-persistence-into-one-flush-controller.md): The source note links to this decision directly.
- **`shares-code-with`** — [0009. Session persistence as an abstract service over the existing `SessionEvent`](0009-session-persistence-as-an-abstract-service-over-the-existing-sessioneven.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0536. Fold the persistence interface into dsh-session](0536-fold-the-persistence-interface-into-dsh-session.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0065. Make packed chunk rows the default JSONL layout](0065-make-packed-chunk-rows-the-default-jsonl-layout.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0085. the end-seed log boundary](0085-the-end-seed-log-boundary.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0446. Drop the mutable session summary](0446-drop-the-mutable-session-summary.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0013-shared-persistence-write-coordinator.md`.
