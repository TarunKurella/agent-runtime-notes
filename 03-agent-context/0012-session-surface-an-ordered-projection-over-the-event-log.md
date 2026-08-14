---
id: "dsh-note-0012"
title: "Session surface --- an ordered projection over the event log"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-06-18-session-surface.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "end"
  - "events"
  - "surface"
  - "indexOf"
  - "time"
  - "seq"
  - "SessionSurface"
  - "surfaceOp"
  - "SurfaceManager"
  - "SurfaceOp"
  - "SurfaceIntent"
  - "SessionEvent"
  - "version"
  - "SCHEMA_VERSION"
search_regex: "(?i)(events|surface|indexOf|time|SessionSurface|surfaceOp|SurfaceManager|SurfaceIntent)"
---

# 0012. Session surface --- an ordered projection over the event log — implementation context

## Open this when

The event log is authoritative, but history manipulation had no durable shared mechanism. Without one, plugins such as compaction would rewrite derived requests through order-sensitive listeners without recording which events each replacement used. Every new history manipulation would also require changes to deriveMessages().

## Source decision

Add a surface --- a derived, cached order of event sequences (the subset of events that produce LLM messages) --- maintained by surfaceOp markers in the event log. Every SessionEvent gains two optional fields (structural metadata, like seq/time): sourceEventSeqs?: number[] --- seq numbers of earlier events cited as sources (e.g., the assistant/chunk seqs that built an assistant/message, or the surface nodes shadowed by a compaction marker).

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-06-18-session-surface.md](../02-notes/implemented/architecture/2026-06-18-session-surface.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-06-18-session-surface.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-06-18-session-surface.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/core/session`. Core file in the package named by the note: `packages/core/session`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/core/session`. Core file in the package named by the note: `packages/core/session`. | `exact-code-occurrence, named-directory-member, named-package-member, symbol-definition` |
| [`packages/core/session/src/repair.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/core/session`. Core file in the package named by the note: `packages/core/session`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/core/session/src/surface.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/core/session`. Core file in the package named by the note: `packages/core/session`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/core/session`. Core file in the package named by the note: `packages/core/session`. | `named-directory-member, named-package-member` |
| [`packages/core/session/src/chunk-rows.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/core/session`. Core file in the package named by the note: `packages/core/session`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/compaction/compaction/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/index.ts) | package entry point | Core file in the package named by the note: `packages/compaction/compaction`. | `named-package-member` |
| [`packages/compaction/compaction/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/compaction/compaction`. | `named-package-member` |
| [`packages/compaction/compaction/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/compaction/compaction`. | `named-package-member` |
| [`packages/compaction/compaction/src/tool-pairing.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/tool-pairing.ts) | runtime implementation | Core file in the package named by the note: `packages/compaction/compaction`. Defines `surface`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/core/session`. Core file in the package named by the note: `packages/core/session`. | `named-directory-member, named-package-member` |
| [`packages/core/agent-loop/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/core/agent-loop`. | `named-directory-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `end` | `const` | [`packages/acp/acp/src/index.ts:325`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L325) | `const end = inflight.endReason` |
| `events` | `const` | [`packages/compaction/compaction/src/tool-pairing.ts:59`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/tool-pairing.ts#L59) | `const events = session.events` |
| `surface` | `const` | [`packages/compaction/compaction/src/tool-pairing.ts:78`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/tool-pairing.ts#L78) | `const surface = session.surface` |
| `indexOf` | `function` | [`packages/core/session/src/chunk-rows.ts:131`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts#L131) | `function indexOf(event: DeltaEvent): number {` |
| `time` | `let` | [`packages/core/session/src/chunk-rows.ts:284`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts#L284) | `let time = value.time0 as number` |
| `events` | `const` | [`packages/core/session/src/chunk-rows.ts:295`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts#L295) | `const events: SessionEvent[] = []` |
| `time` | `let` | [`packages/core/session/src/chunk-rows.ts:296`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts#L296) | `let time = row.time0` |
| `seq` | `const` | [`packages/core/session/src/index.ts:233`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L233) | `const seq = event['seq']` |
| `time` | `const` | [`packages/core/session/src/index.ts:234`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L234) | `const time = event['time']` |
| `surface` | `const` | [`packages/core/session/src/index.ts:727`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L727) | `const surface = this.surface` |
| `events` | `const` | [`packages/core/session/src/index.ts:1098`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L1098) | `const events = session.events` |
| `seq` | `let` | [`packages/core/session/src/repair.ts:85`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts#L85) | `let seq = last.seq + 1` |
| `time` | `const` | [`packages/core/session/src/repair.ts:86`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts#L86) | `const time = last.time` |
| `SessionSurface` | `interface` | [`packages/core/session/src/surface.ts:137`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L137) | `export interface SessionSurface {` |
| `surfaceOp` | `const` | [`packages/core/session/src/surface.ts:331`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L331) | `const surfaceOp = surfaceOpOf(event)` |
| `SurfaceManager` | `class` | [`packages/core/session/src/surface.ts:398`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L398) | `export class SurfaceManager implements SessionSurface {` |

### Tests and executable evidence

- [`packages/core/session/tests/fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/fork.spec.ts) — A test under the owning area exercises or imports `deriveMessages`. Contains the exact code literal `compaction/start` named by the note.
- [`packages/core/session/tests/surface.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/surface.spec.ts) — A test under the owning area exercises or imports `deriveMessages`. A test under the owning area exercises or imports `SurfaceManager`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `deriveMessages`. A test under the owning area exercises or imports `SessionSurface`.
- [`packages/core/session/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/properties.spec.ts) — A test under the owning area exercises or imports `deriveMessages`. A test under the owning area exercises or imports `SurfaceIntent`.
- [`packages/core/session/tests/derived-cache.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/derived-cache.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/session/tests/gen-persistence-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/gen-persistence-catalog.spec.ts) — A test under the owning area exercises or imports `SurfaceOp`.
- [`packages/session/session-persistence-sqlite/tests/sqlite.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-sqlite/tests/sqlite.spec.ts) — A test under the owning area exercises or imports `SCHEMA_VERSION`.
- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — Contains the exact code literal `compaction/start` named by the note. Contains the exact code literal `compaction/end` named by the note.

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

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/agent-loop`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/storage`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `end`, `events`, `surface`, `indexOf`, `time`, `seq`, `SessionSurface`, `surfaceOp`, `SurfaceManager`, `SurfaceOp`, `SurfaceIntent`, `SessionEvent`, `version`, `SCHEMA_VERSION`
- Regex: `(?i)(events|surface|indexOf|time|SessionSurface|surfaceOp|SurfaceManager|SurfaceIntent)`

```bash
rg -n --pcre2 "(?i)(events|surface|indexOf|time|SessionSurface|surfaceOp|SurfaceManager|SurfaceIntent)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0518. Recallable compaction --- index checkpoints, a state checkpoint, and in-session history recall](0518-recallable-compaction-index-checkpoints-a-state-checkpoint-and-in-sessio.md): Shares source implementation: `packages/compaction/compaction/src/index.ts`, `packages/compaction/compaction/src/invariant.ts`.
- **`shares-code-with`** — [0316. The human transcript projects append-origin events](0316-the-human-transcript-projects-append-origin-events.md): Shares source implementation: `packages/compaction/compaction/src/index.ts`, `packages/compaction/compaction/src/invariant.ts`.
- **`shares-code-with`** — [0168. Cross-session references](0168-cross-session-references.md): Shares source implementation: `packages/compaction/compaction/src/index.ts`, `packages/compaction/compaction/src/invariant.ts`.
- **`shares-code-with`** — [0003. Event-sourced sessions with derived message history](0003-event-sourced-sessions-with-derived-message-history.md): Shares source implementation: `packages/compaction/compaction/src/index.ts`, `packages/compaction/compaction/src/invariant.ts`.
- **`shares-code-with`** — [0085. the end-seed log boundary](0085-the-end-seed-log-boundary.md): Shares source implementation: `packages/core/session/src/chunk-rows.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0446. Drop the mutable session summary](0446-drop-the-mutable-session-summary.md): Shares source implementation: `packages/core/session/src/chunk-rows.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/repair.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0012-session-surface-an-ordered-projection-over-the-event-log.md`.
