---
id: "dsh-note-0316"
title: "The human transcript projects append-origin events"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-29-human-transcript-append-origin.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
aliases:
  - "isCompactCheckpointSource"
  - "CompactionEngine"
  - "events"
  - "surface"
  - "current"
  - "isAppendSurfaceEvent"
  - "isReplacementSurfaceEvent"
  - "SurfaceOp"
  - "history"
  - "maxMessages"
  - "user/message"
  - "assistant/message"
  - "compaction/summary"
  - "Session.events"
search_regex: "(?i)(isCompactCheckpointSource|CompactionEngine|events|surface|current|isAppendSurfaceEvent|isReplacementSurfaceEvent|SurfaceOp)"
---

# 0316. The human transcript projects append-origin events — implementation context

## Open this when

The terminal and the host history gateway both treated the model-visible surface as the human transcript. A successful compaction replaces a surface range with one checkpoint node, so the moment that replacement landed the terminal dropped every message it shadowed --- conversation the user had already read --- and re-ran that destructive rebuild on any later replacement.

## Source decision

Model and human projections are separate, and the event's own marker decides which one an event belongs to. dsh-session exports the marker split isAppendSurfaceEvent(event) and isReplacementSurfaceEvent(event) over the two SurfaceOp variants, from the browser-safe surface module. Append-origin events are the durable source for a transcript; replacement copies stay model-only. Everything that must send exactly what the model sees --- deriveMessages, token accounting, the compaction backends, tool pairing, injected-context liveness, cross-session reference projection --- keeps reading session.surface.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-29-human-transcript-append-origin.md](../02-notes/implemented/bug-fix/2026-07-29-human-transcript-append-origin.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-29-human-transcript-append-origin.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-29-human-transcript-append-origin.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/json.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/json.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `current`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. Defines `events`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. Defines `SurfaceOp`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/surface.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `isAppendSurfaceEvent`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/chunk-rows.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `events`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/compaction/compaction/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/index.ts) | package entry point | Core file in the package named by the note: `packages/compaction/compaction`. Defines `CompactionEngine`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/compaction/compaction/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/compaction/compaction`. | `named-package-member` |
| [`packages/compaction/compaction/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/compaction/compaction`. | `named-package-member` |
| [`packages/context/session-reference/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/index.ts) | package entry point | Core file in the package named by the note: `packages/context/session-reference`. | `named-package-member` |
| [`packages/context/session-reference/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/context/session-reference`. | `named-package-member` |
| [`packages/compaction/compaction/src/checkpoint.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/checkpoint.ts) | runtime implementation | Core file in the package named by the note: `packages/compaction/compaction`. Defines `isCompactCheckpointSource`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `isCompactCheckpointSource` | `function` | [`packages/compaction/compaction/src/checkpoint.ts:49`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/checkpoint.ts#L49) | `export function isCompactCheckpointSource(source: MessageSource): boolean {` |
| `CompactionEngine` | `class` | [`packages/compaction/compaction/src/index.ts:96`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/index.ts#L96) | `export abstract class CompactionEngine extends Service {` |
| `events` | `const` | [`packages/compaction/compaction/src/tool-pairing.ts:59`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/tool-pairing.ts#L59) | `const events = session.events` |
| `surface` | `const` | [`packages/compaction/compaction/src/tool-pairing.ts:78`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/tool-pairing.ts#L78) | `const surface = session.surface` |
| `current` | `let` | [`packages/context/agent-instructions/src/files.ts:182`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/files.ts#L182) | `let current = resolve(cwd)` |
| `current` | `let` | [`packages/context/agent-instructions/src/files.ts:201`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/files.ts#L201) | `let current = resolve(cwd)` |
| `current` | `const` | [`packages/context/agent-instructions/src/index.ts:267`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/index.ts#L267) | `const current = previous.then(() => composeAndSync(agent, projectionLifecycle.signal, [], [touchedPath]))` |
| `events` | `const` | [`packages/core/session/src/chunk-rows.ts:295`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts#L295) | `const events: SessionEvent[] = []` |
| `current` | `const` | [`packages/core/session/src/index.ts:202`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L202) | `const current = pending.pop()!` |
| `surface` | `const` | [`packages/core/session/src/index.ts:727`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L727) | `const surface = this.surface` |
| `events` | `const` | [`packages/core/session/src/index.ts:1098`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L1098) | `const events = session.events` |
| `current` | `const` | [`packages/core/session/src/json.ts:117`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/json.ts#L117) | `const current = task.value` |
| `isAppendSurfaceEvent` | `function` | [`packages/core/session/src/surface.ts:51`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L51) | `export function isAppendSurfaceEvent(` |
| `isReplacementSurfaceEvent` | `function` | [`packages/core/session/src/surface.ts:64`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L64) | `export function isReplacementSurfaceEvent(` |
| `SurfaceOp` | `type` | [`packages/core/session/src/types.ts:372`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L372) | `export type SurfaceOp =` |
| `events` | `const` | [`packages/session-query/session-query/src/index.ts:334`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/index.ts#L334) | `const events = loaded.events.slice(startSeq, endSeq + 1)` |

### Tests and executable evidence

- [`packages/core/session/tests/fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/fork.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/session/tests/surface.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/surface.spec.ts) — A test under the owning area exercises or imports `isAppendSurfaceEvent`. A test under the owning area exercises or imports `isReplacementSurfaceEvent`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `deriveMessages`. A test under the owning area exercises or imports `agent-instructions`.
- [`packages/core/session/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/properties.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/session/tests/derived-cache.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/derived-cache.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/compaction/compaction/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/tests/invariant.spec.ts) — A test under the owning area exercises or imports `dsh-compaction`.
- [`packages/compaction/compaction/tests/compaction.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/tests/compaction.spec.ts) — A test under the owning area exercises or imports `isCompactCheckpointSource`. A test under the owning area exercises or imports `CompactionEngine`.
- [`packages/compaction/compaction/tests/tool-pairing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/tests/tool-pairing.spec.ts) — A test under the owning area exercises or imports `dsh-compaction`.

## How to read the implementation

1. Start with [`packages/core/session/src/json.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/json.ts) because it has the strongest evidence link to the note.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/projection`
- Aliases: `isCompactCheckpointSource`, `CompactionEngine`, `events`, `surface`, `current`, `isAppendSurfaceEvent`, `isReplacementSurfaceEvent`, `SurfaceOp`, `history`, `maxMessages`, `user/message`, `assistant/message`, `compaction/summary`, `Session.events`
- Regex: `(?i)(isCompactCheckpointSource|CompactionEngine|events|surface|current|isAppendSurfaceEvent|isReplacementSurfaceEvent|SurfaceOp)`

```bash
rg -n --pcre2 "(?i)(isCompactCheckpointSource|CompactionEngine|events|surface|current|isAppendSurfaceEvent|isReplacementSurfaceEvent|SurfaceOp)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0601. Live standalone compaction progress in the terminal](0601-live-standalone-compaction-progress-in-the-terminal.md): The source note links to this decision directly.
- **`source-link`** — [0326. The browser conversation is a log-ordered human transcript](0326-the-browser-conversation-is-a-log-ordered-human-transcript.md): The source note links to this decision directly.
- **`shares-code-with`** — [0012. Session surface --- an ordered projection over the event log](0012-session-surface-an-ordered-projection-over-the-event-log.md): Shares source implementation: `packages/compaction/compaction/src/index.ts`, `packages/compaction/compaction/src/invariant.ts`.
- **`shares-code-with`** — [0168. Cross-session references](0168-cross-session-references.md): Shares source implementation: `packages/compaction/compaction/src/index.ts`, `packages/compaction/compaction/src/invariant.ts`.
- **`shares-code-with`** — [0518. Recallable compaction --- index checkpoints, a state checkpoint, and in-session history recall](0518-recallable-compaction-index-checkpoints-a-state-checkpoint-and-in-sessio.md): Shares source implementation: `packages/compaction/compaction/src/index.ts`, `packages/compaction/compaction/src/invariant.ts`.
- **`shares-code-with`** — [0003. Event-sourced sessions with derived message history](0003-event-sourced-sessions-with-derived-message-history.md): Shares source implementation: `packages/compaction/compaction/src/index.ts`, `packages/compaction/compaction/src/invariant.ts`.
- **`shares-code-with`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0466. Remove synthetic turns for log-only events](0466-remove-synthetic-turns-for-log-only-events.md): Shares source implementation: `packages/core/session/src/chunk-rows.ts`, `packages/core/session/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0316-the-human-transcript-projects-append-origin-events.md`.
