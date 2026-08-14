---
id: "dsh-note-0122"
title: "Session log versioning --- one integer, an upgrade chain, and a per-event ignorable marker"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-10-session-log-version-mechanism.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "append"
  - "SessionEventMap"
  - "KNOWN_SESSION_EVENT_TYPES"
  - "surfaceOp"
  - "SurfaceEventType"
  - "SurfaceOp"
  - "SCHEMA_VERSION"
  - "SessionPersistenceCorruptionError"
  - "SessionFormatUnsupportedError"
  - "assertVersion"
  - "request/header"
  - "request/context"
  - "session/end-seed"
  - "gen-persistence-catalog"
search_regex: "(?i)(append|SessionEventMap|KNOWN_SESSION_EVENT_TYPES|surfaceOp|SurfaceEventType|SCHEMA_VERSION|SessionPersistenceCorruptionError|SessionFormatUnsupportedError)"
---

# 0122. Session log versioning --- one integer, an upgrade chain, and a per-event ignorable marker — implementation context

## Open this when

Session logs must be upgradable after release, and the runtime that ships first is the floor for every later decision: whatever refusal and degradation behavior is missing from the first released reader can never be added to the copies users already run. Release issue #1901 required at minimum that an old runtime reading a newer session format reports "unsupported" instead of misreading it.

## Source decision

One monotonic integer, no major/minor split. Whether a version step is auto-upgradable is a property of that step --- expressed by whether its upgrader exists --- not something a two-level numbering scheme should promise in advance (you rarely know at design time whether the next change will turn out "major"). This matches the SQLite backend's SCHEMA_VERSION precedent. The writer decides bumps, not the reader. A bump is required exactly when an old runtime could no longer handle a new log with full semantic correctness.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-10-session-log-version-mechanism.md](../02-notes/implemented/architecture/2026-08-10-session-log-version-mechanism.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-10-session-log-version-mechanism.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-10-session-log-version-mechanism.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Defines `SessionEventMap`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Defines `SessionEventMap`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/domain.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/domain.ts) | runtime implementation | Defines `SessionEventMap`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Defines `SurfaceEventType`, a construct named by the note. Defines `SurfaceOp`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-retry/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/types.ts) | public types and contract | Defines `SessionEventMap`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/surface.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts) | runtime implementation | Defines `surfaceOp`, a construct named by the note. | `symbol-definition` |
| [`packages/workflow/tool-workflow/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/index.ts) | package entry point | Defines `append`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/known-event-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/known-event-types.ts) | runtime implementation | Defines `KNOWN_SESSION_EVENT_TYPES`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `append`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-persistence/src/coordinator.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/coordinator.ts) | runtime implementation | Defines `SessionFormatUnsupportedError`, a construct named by the note. Defines `SessionPersistenceCorruptionError`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-persistence-sqlite/src/schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-sqlite/src/schema.ts) | runtime implementation | Defines `SCHEMA_VERSION`, a construct named by the note. | `symbol-definition` |
| [`packages/code-runtime/code-runtime-worker-thread/src/output-json.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/output-json.ts) | runtime implementation | Defines `append`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `append` | `const` | [`packages/client/connection/src/client/fixture.ts:1681`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1681) | `const append = (id: SessionId, e: Record<string, unknown>): void => {` |
| `append` | `function` | [`packages/code-runtime/code-runtime-worker-thread/src/output-json.ts:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/output-json.ts#L45) | `function append<T>(target: T[], value: T): void {` |
| `append` | `function` | [`packages/code-runtime/code-runtime-worker-thread/src/worker-json.ts:54`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/worker-json.ts#L54) | `function append<T>(target: T[], value: T): void {` |
| `SessionEventMap` | `interface` | [`packages/core/agent/src/types.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts#L13) | `interface SessionEventMap {` |
| `KNOWN_SESSION_EVENT_TYPES` | `const` | [`packages/core/session/src/known-event-types.ts:19`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/known-event-types.ts#L19) | `export const KNOWN_SESSION_EVENT_TYPES: ReadonlySet<string> = new Set([` |
| `surfaceOp` | `const` | [`packages/core/session/src/surface.ts:331`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L331) | `const surfaceOp = surfaceOpOf(event)` |
| `SessionEventMap` | `interface` | [`packages/core/session/src/types.ts:236`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L236) | `export interface SessionEventMap {` |
| `SurfaceEventType` | `type` | [`packages/core/session/src/types.ts:343`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L343) | `export type SurfaceEventType =` |
| `SurfaceOp` | `type` | [`packages/core/session/src/types.ts:372`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L372) | `export type SurfaceOp =` |
| `SessionEventMap` | `interface` | [`packages/core/tools/src/types.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts#L26) | `interface SessionEventMap {` |
| `SessionEventMap` | `interface` | [`packages/goal/goal/src/domain.ts:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/domain.ts#L62) | `interface SessionEventMap {` |
| `SessionEventMap` | `interface` | [`packages/llm/llm-retry/src/types.ts:7`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/types.ts#L7) | `interface SessionEventMap {` |
| `SCHEMA_VERSION` | `const` | [`packages/session/session-persistence-sqlite/src/schema.ts:20`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-sqlite/src/schema.ts#L20) | `export const SCHEMA_VERSION = 15` |
| `SessionPersistenceCorruptionError` | `class` | [`packages/session/session-persistence/src/coordinator.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/coordinator.ts#L36) | `export class SessionPersistenceCorruptionError extends Error {` |
| `SessionFormatUnsupportedError` | `class` | [`packages/session/session-persistence/src/coordinator.ts:55`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/coordinator.ts#L55) | `export class SessionFormatUnsupportedError extends Error {` |
| `append` | `const` | [`packages/workflow/tool-workflow/src/index.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/index.ts#L75) | `const append = <Type extends keyof ToolWorkflowRecordEventMap>(` |

### Tests and executable evidence

- [`packages/core/session/tests/surface.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/surface.spec.ts) — A test under the owning area exercises or imports `SurfaceEventType`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `ignorable`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `SessionEventMap`.
- [`packages/core/session/tests/gen-persistence-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/gen-persistence-catalog.spec.ts) — A test under the owning area exercises or imports `SurfaceEventType`. A test under the owning area exercises or imports `SurfaceOp`.
- [`examples/headless-agent/tests/session-format-guard.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/headless-agent/tests/session-format-guard.snapshot.ts) — A test under the owning area exercises or imports `ignorable`.
- [`packages/session/session-persistence-sqlite/tests/sqlite.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-sqlite/tests/sqlite.spec.ts) — A test under the owning area exercises or imports `SCHEMA_VERSION`. A test under the owning area exercises or imports `ignorable`.
- [`packages/session/session-persistence/tests/coordinator-contract.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/tests/coordinator-contract.ts) — A test under the owning area exercises or imports `SessionFormatUnsupportedError`. A test under the owning area exercises or imports `ignorable`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — Contains the exact code literal `session/end-seed` named by the note.

## How to read the implementation

1. Start with [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `append`, `SessionEventMap`, `KNOWN_SESSION_EVENT_TYPES`, `surfaceOp`, `SurfaceEventType`, `SurfaceOp`, `SCHEMA_VERSION`, `SessionPersistenceCorruptionError`, `SessionFormatUnsupportedError`, `assertVersion`, `request/header`, `request/context`, `session/end-seed`, `gen-persistence-catalog`
- Regex: `(?i)(append|SessionEventMap|KNOWN_SESSION_EVENT_TYPES|surfaceOp|SurfaceEventType|SCHEMA_VERSION|SessionPersistenceCorruptionError|SessionFormatUnsupportedError)`

```bash
rg -n --pcre2 "(?i)(append|SessionEventMap|KNOWN_SESSION_EVENT_TYPES|surfaceOp|SurfaceEventType|SCHEMA_VERSION|SessionPersistenceCorruptionError|SessionFormatUnsupportedError)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0535. Drop durable step boundary events](0535-drop-durable-step-boundary-events.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0460. Collapse live persistence into one flush controller](0460-collapse-live-persistence-into-one-flush-controller.md): Shares source implementation: `packages/core/session/src/surface.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/core/tools/src/types.ts`.
- **`shares-code-with`** — [0142. Dynamic workflows --- a script-driven multi-agent orchestration seam](0142-dynamic-workflows-a-script-driven-multi-agent-orchestration-seam.md): Shares source implementation: `packages/core/agent/src/types.ts`, `packages/core/tools/src/types.ts`.
- **`shares-code-with`** — [0158. persistent PTY sessions](0158-persistent-pty-sessions.md): Shares source implementation: `packages/core/agent/src/types.ts`, `packages/core/tools/src/types.ts`.
- **`shares-code-with`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): Shares source implementation: `packages/core/session/src/surface.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0012. Session surface --- an ordered projection over the event log](0012-session-surface-an-ordered-projection-over-the-event-log.md): Shares source implementation: `packages/core/session/src/surface.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0150. Expose agent session identity and JSONL location to tools and hooks](0150-expose-agent-session-identity-and-jsonl-location-to-tools-and-hooks.md): Shares source implementation: `packages/core/agent/src/types.ts`, `packages/core/session/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0122-session-log-versioning-one-integer-an-upgrade-chain-and-a-per-event-igno.md`.
