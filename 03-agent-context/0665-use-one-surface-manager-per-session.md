---
id: "dsh-note-0665"
title: "Use one surface manager per session"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-07-19-use-one-session-surface-manager.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/context"
  - "domain/filesystem"
  - "domain/session-state"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/projection"
aliases:
  - "surface"
  - "foldSurface"
  - "SurfaceManager"
  - "session.surface"
  - "validateNext"
  - "log.push"
  - "internal/dispatch"
  - "Session.surface"
  - "Use one surface manager per session"
  - "simplification"
  - "boundary"
  - "evidence"
  - "schema types"
  - "trust"
search_regex: "(?i)(surface|foldSurface|SurfaceManager|session\\.surface|validateNext|log\\.push|internal/dispatch|Use[- ]one[- ]surface[- ]manager[- ]per[- ]session)"
---

# 0665. Use one surface manager per session — implementation context

## Open this when

Session maintained two SurfaceManager instances over the same append-only event log. One validated seed and append candidates, while a second lazy instance independently folded committed events for session.surface, derived messages, compaction, and workspace context. Once the public surface had been read, every later event advanced duplicate node and replacement-generation state without creating a separate authority or failure boundary.

## Source decision

Each Session owns one eagerly constructed SurfaceManager. Seed and append acceptance call validateNext() on that manager before committing an event, and session.surface returns the same object through this readonly contract: Candidate validation remains atomic. validateNext() may synchronize committed log entries, but it only plans the uncommitted candidate. The candidate enters manager state after log.push() and the next delta synchronization, so surface validation failures and pre-commit internal/dispatch vetoes leave no phantom node or replacement generation.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-07-19-use-one-session-surface-manager.md](../02-notes/archived/simplification/2026-07-19-use-one-session-surface-manager.md)
- Pinned source: [.agents/notes/archived/simplification/2026-07-19-use-one-session-surface-manager.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-07-19-use-one-session-surface-manager.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. Defines `surface`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/surface.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `SurfaceManager`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/session) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/session/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`vendor/cordis/src/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts) | runtime implementation | Contains the exact code literal `internal/dispatch` named by the note. | `exact-code-occurrence` |
| [`docs/cordis-api/inherited.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cordis-api/inherited.md) | package contract and examples | Contains the exact code literal `internal/dispatch` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts) | repository automation | Contains the exact code literal `internal/dispatch` named by the note. | `exact-code-occurrence` |
| [`docs/event-producer-consumer.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/event-producer-consumer.md) | package contract and examples | Contains the exact code literal `internal/dispatch` named by the note. | `exact-code-occurrence` |
| [`docs/event-producer-consumer.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/event-producer-consumer.zh.md) | package contract and examples | Contains the exact code literal `internal/dispatch` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `surface` | `const` | [`packages/core/session/src/index.ts:727`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L727) | `const surface = this.surface` |
| `foldSurface` | `function` | [`packages/core/session/src/surface.ts:387`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L387) | `export function foldSurface(events: readonly SessionEvent[]): SurfaceFoldResult {` |
| `SurfaceManager` | `class` | [`packages/core/session/src/surface.ts:398`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L398) | `export class SurfaceManager implements SessionSurface {` |

### Tests and executable evidence

- [`packages/core/session/tests/surface.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/surface.spec.ts) — A test under the owning area exercises or imports `SurfaceManager`. A test under the owning area exercises or imports `foldSurface`.
- [`packages/core/tools/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/scoped.spec.ts) — Contains the exact code literal `internal/dispatch` named by the note.
- [`packages/core/agent-loop/tests/coverage-edges.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/coverage-edges.spec.ts) — Contains the exact code literal `internal/dispatch` named by the note.
- [`packages/core/agent-loop/tests/contract-regressions.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/contract-regressions.spec.ts) — Contains the exact code literal `internal/dispatch` named by the note.

## How to read the implementation

1. Start with [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/evidence`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/context`, `domain/filesystem`, `domain/session-state`, `domain/testing`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/projection`
- Aliases: `surface`, `foldSurface`, `SurfaceManager`, `session.surface`, `validateNext`, `log.push`, `internal/dispatch`, `Session.surface`, `Use one surface manager per session`, `simplification`, `boundary`, `evidence`, `schema types`, `trust`
- Regex: `(?i)(surface|foldSurface|SurfaceManager|session\.surface|validateNext|log\.push|internal/dispatch|Use[- ]one[- ]surface[- ]manager[- ]per[- ]session)`

```bash
rg -n --pcre2 "(?i)(surface|foldSurface|SurfaceManager|session\\.surface|validateNext|log\\.push|internal/dispatch|Use[- ]one[- ]surface[- ]manager[- ]per[- ]session)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0447. Fold trace-only session facts into load-bearing events](0447-fold-trace-only-session-facts-into-load-bearing-events.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0530. Deterministic tests, the replay invariant fixture, and race stress](0530-deterministic-tests-the-replay-invariant-fixture-and-race-stress.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0635. Generated persistence log event catalog](0635-generated-persistence-log-event-catalog.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0466. Remove synthetic turns for log-only events](0466-remove-synthetic-turns-for-log-only-events.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0171. Default local instruction overlay](0171-default-local-instruction-overlay.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0102. Reusable Session preparation before publication](0102-reusable-session-preparation-before-publication.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0311. Load sessions persisted before message identity](0311-load-sessions-persisted-before-message-identity.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0154. Session query relationship tracing](0154-session-query-relationship-tracing.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0665-use-one-surface-manager-per-session.md`.
