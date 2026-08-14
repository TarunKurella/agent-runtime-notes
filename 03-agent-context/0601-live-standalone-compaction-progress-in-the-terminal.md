---
id: "dsh-note-0601"
title: "Live standalone compaction progress in the terminal"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-30-compaction-progress-visibility.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/context"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
aliases:
  - "progressLabel"
  - "CommandDefinition"
  - "compact/start"
  - "compact/start { turn: null }"
  - "compact/end"
  - "runningPhaseGlyph"
  - "session/event"
  - "TurnPhase"
  - "TimingBucket"
  - "command/run"
  - "command/done"
  - "Context being compacted 1.0s"
  - "Live standalone compaction progress in the terminal"
  - "feature"
search_regex: "(?i)(progressLabel|CommandDefinition|compact/start|compact/start[- ]\\{[- ]turn:[- ]null[- ]\\}|compact/end|runningPhaseGlyph|session/event|TurnPhase)"
---

# 0601. Live standalone compaction progress in the terminal — implementation context

## Open this when

A standalone manual compaction runs between turns while the agent remains idle. The TUI's turn-phase indicator therefore kept its plain > caret throughout the slow summary operation, and a failed attempt produced no transcript row because no replacement checkpoint landed. The liveness presentation needs to reuse the existing status indicator without introducing a second animated status location. The durable log can retain an unmatched compact/start after a process dies.

## Source decision

The TUI treats the live standalone compact/start { turn: null } to matching compact/end bracket as the source of in-flight compaction presentation. A module-local compacting cell records the render-clock start and owns one animation timer. A fixed row above the prompt renders Context being compacted from that clock, the existing one-cell running status indicator renders ⊙ through the same fade and throb path as turn-phase glyphs, and the terminal progress bit remains active until the bracket closes. runningPhaseGlyph owns the choice among turn-phase glyphs, ⊙, and the idle caret.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-30-compaction-progress-visibility.md](../02-notes/archived/feature/2026-07-30-compaction-progress-visibility.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-30-compaction-progress-visibility.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-30-compaction-progress-visibility.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/interaction/commands/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/index.ts) | package entry point | Defines `CommandDefinition`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/TodoPanel.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/TodoPanel.tsx) | runtime implementation | Defines `progressLabel`, a construct named by the note. | `symbol-definition` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `session/event` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `session/event` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `session/event` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Contains the exact code literal `session/event` named by the note. | `exact-code-occurrence` |
| [`docs/persistence-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/persistence-catalog.md) | package contract and examples | Contains the exact code literal `session/event` named by the note. Contains the exact code literal `command/run` named by the note. | `exact-code-occurrence` |
| [`docs/persistence-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/persistence-catalog.zh.md) | package contract and examples | Contains the exact code literal `session/event` named by the note. Contains the exact code literal `command/run` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-persistence-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-persistence-catalog.ts) | repository automation | Contains the exact code literal `session/event` named by the note. | `exact-code-occurrence` |
| [`docs/event-producer-consumer.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/event-producer-consumer.zh.md) | package contract and examples | Contains the exact code literal `session/event` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `progressLabel` | `function` | [`packages/client/ui-conversation/src/client/skeleton/TodoPanel.tsx:82`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/TodoPanel.tsx#L82) | `function progressLabel(todos: readonly TodoItem[], t: TodoPanelProps['t']): string {` |
| `CommandDefinition` | `interface` | [`packages/interaction/commands/src/index.ts:40`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/index.ts#L40) | `export interface CommandDefinition {` |

### Tests and executable evidence

- [`packages/interaction/commands/tests/commands.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/tests/commands.spec.ts) — A test under the owning area exercises or imports `CommandDefinition`.
- [`packages/compaction/compaction/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/tests/invariant.spec.ts) — A test under the owning area exercises or imports `compacting`.
- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — Contains the exact code literal `command/run` named by the note. Contains the exact code literal `command/done` named by the note.
- [`apps/web/tests/navigation-panes.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/navigation-panes.e2e.ts) — Contains the exact code literal `command/run` named by the note. Contains the exact code literal `command/done` named by the note.
- [`apps/web/tests/feedback-command.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/feedback-command.e2e.ts) — Contains the exact code literal `command/run` named by the note. Contains the exact code literal `command/done` named by the note.
- [`apps/web/tests/goal-command-presentation.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/goal-command-presentation.e2e.ts) — Contains the exact code literal `command/run` named by the note. Contains the exact code literal `command/done` named by the note.
- [`apps/web/tests/snapshots/approval-composer/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/approval-composer/session.jsonl) — Contains the exact code literal `command/run` named by the note. Contains the exact code literal `command/done` named by the note.
- [`apps/web/tests/snapshots/permission-policy-context/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/permission-policy-context/session.jsonl) — Contains the exact code literal `command/run` named by the note. Contains the exact code literal `command/done` named by the note.

## How to read the implementation

1. Start with [`packages/interaction/commands/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/context`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/projection`
- Aliases: `progressLabel`, `CommandDefinition`, `compact/start`, `compact/start { turn: null }`, `compact/end`, `runningPhaseGlyph`, `session/event`, `TurnPhase`, `TimingBucket`, `command/run`, `command/done`, `Context being compacted 1.0s`, `Live standalone compaction progress in the terminal`, `feature`
- Regex: `(?i)(progressLabel|CommandDefinition|compact/start|compact/start[- ]\{[- ]turn:[- ]null[- ]\}|compact/end|runningPhaseGlyph|session/event|TurnPhase)`

```bash
rg -n --pcre2 "(?i)(progressLabel|CommandDefinition|compact/start|compact/start[- ]\\{[- ]turn:[- ]null[- ]\\}|compact/end|runningPhaseGlyph|session/event|TurnPhase)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0328. Compaction checkpoints use an English engineering register](0328-compaction-checkpoints-use-an-english-engineering-register.md): Shares source implementation: `docs/agent-lifecycle.md`, `docs/architecture.md`.
- **`shares-code-with`** — [0310. Stable snapshot refresh volatiles](0310-stable-snapshot-refresh-volatiles.md): Shares source implementation: `docs/agent-lifecycle.md`, `docs/persistence-catalog.md`.
- **`shares-code-with`** — [0215. Queued manual compaction with one durable lock](0215-queued-manual-compaction-with-one-durable-lock.md): Shares source implementation: `packages/interaction/commands/src/index.ts`.
- **`shares-code-with`** — [0358. The minimal preset owns the complete RL agent composition](0358-the-minimal-preset-owns-the-complete-rl-agent-composition.md): Shares source implementation: `docs/agent-lifecycle.md`, `scripts/gen-doc-graphs.ts`.
- **`shares-code-with`** — [0581. TUI status line badges queued steering messages](0581-tui-status-line-badges-queued-steering-messages.md): Shares source implementation: `docs/agent-lifecycle.md`, `scripts/gen-doc-graphs.ts`.
- **`shares-code-with`** — [0633. Documentation graph index for maintainers and SDK users](0633-documentation-graph-index-for-maintainers-and-sdk-users.md): Shares source implementation: `docs/agent-lifecycle.md`, `scripts/gen-doc-graphs.ts`.
- **`shares-code-with`** — [0179. Web todo display --- snapshot side-effect channel + two render surfaces](0179-web-todo-display-snapshot-side-effect-channel-two-render-surfaces.md): Shares source implementation: `scripts/gen-doc-graphs.ts`.
- **`shares-code-with`** — [0140. Interception extension points --- the typed-Decision surface a hook programs against](0140-interception-extension-points-the-typed-decision-surface-a-hook-programs.md): Shares source implementation: `docs/architecture.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0601-live-standalone-compaction-progress-in-the-terminal.md`.
