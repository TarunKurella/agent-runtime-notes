---
id: "dsh-note-0179"
title: "Web todo display --- snapshot side-effect channel + two render surfaces"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-23-web-todo-display.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/simplification"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "inject"
  - "apply"
  - "ConversationSnapshot"
  - "ConversationController"
  - "ConversationRoot"
  - "todos"
  - "todoDockEntry"
  - "ToolRow"
  - "todoToolview"
  - "nodes"
  - "ToolEventView"
  - "todo_write"
  - "todo/write"
  - "ConversationSnapshot.nodes"
search_regex: "(?i)(inject|apply|ConversationSnapshot|ConversationController|ConversationRoot|todos|todoDockEntry|ToolRow)"
---

# 0179. Web todo display --- snapshot side-effect channel + two render surfaces — implementation context

## Open this when

todo_write appends todo/write whole-list snapshots to the session log; the TUI renders a persistent plan panel (the automation-only ACP bridge deliberately omits todo presentation). The web client dropped the event entirely: the host mux stream already forwards every session event, but todo/write is not a surface type (it never folds into ConversationSnapshot.nodes), and no side-effect branch accumulated it --- the browser had no consumption point and no display surface.

## Source decision

Consume todo/write as a Session side effect, not a surface node, and render it on two surfaces matching the split the TUI already draws. applyEventSideEffects gains a todo/write case (whole list, last write wins) and clears on turn/start (turn-scoped plan lifetime). rebuildDerivedFromWindow sweeps the window from an empty plan and restores the tail-page seed only when the window never determined the plan (no todo/write and no turn/start); otherwise the in-window write/turn/start fold wins.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-23-web-todo-display.md](../02-notes/implemented/feature/2026-07-23-web-todo-display.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-23-web-todo-display.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-23-web-todo-display.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Defines `nodes`, a construct named by the note. Contains the exact code literal `todo/write` named by the note. | `exact-code-occurrence, symbol-definition` |
| [`vendor/cordis/src/registry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts) | runtime implementation | Defines `inject`, a construct named by the note. | `symbol-definition` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `inject`, a construct named by the note. Defines `apply`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/invariant.ts) | runtime contract checks | Defines `inject`, a construct named by the note. Defines `apply`, a construct named by the note. | `symbol-definition` |
| [`packages/client/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts) | package entry point | Defines `inject`, a construct named by the note. Defines `apply`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts) | package entry point | Defines `inject`, a construct named by the note. Defines `apply`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts) | package entry point | Defines `apply`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Defines `nodes`, a construct named by the note. | `symbol-definition` |
| [`packages/todo/tool-todo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/index.ts) | package entry point | Defines `todos`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/events.ts) | runtime implementation | Defines `ToolEventView`, a construct named by the note. | `symbol-definition` |
| [`packages/compaction/compaction-basic/src/region.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/region.ts) | runtime implementation | Defines `nodes`, a construct named by the note. | `symbol-definition` |
| [`packages/session-query/session-query/src/tracing.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/tracing.ts) | runtime implementation | Defines `nodes`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `inject` | `const` | [`packages/acp/acp/src/index.ts:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L44) | `export const inject = ['agents']` |
| `apply` | `function` | [`packages/acp/acp/src/index.ts:105`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L105) | `export function apply(ctx: Context, config: AcpConfig): void {` |
| `inject` | `const` | [`packages/client/hmr/src/index.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts#L28) | `export const inject = ['clientModules', 'webServer']` |
| `apply` | `function` | [`packages/client/hmr/src/index.ts:57`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts#L57) | `export function apply(ctx: Context, config: Config): void {` |
| `ConversationSnapshot` | `interface` | [`packages/client/runtime/src/client/sessions/conversation.ts:433`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts#L433) | `export interface ConversationSnapshot {` |
| `ConversationController` | `class` | [`packages/client/ui-conversation/src/client/service.ts:91`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/service.ts#L91) | `export class ConversationController extends Service implements IConversation {` |
| `ConversationRoot` | `function` | [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx#L15) | `export function ConversationRoot({` |
| `todos` | `const` | [`packages/client/ui-conversation/src/client/skeleton/TodoPanel.tsx:135`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/TodoPanel.tsx#L135) | `const todos = useProjection('todos')` |
| `todoDockEntry` | `const` | [`packages/client/ui-conversation/src/client/skeleton/TodoPanel.tsx:143`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/TodoPanel.tsx#L143) | `export const todoDockEntry = {` |
| `ToolRow` | `function` | [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx:128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx#L128) | `export function ToolRow({` |
| `todos` | `const` | [`packages/client/ui-tool/src/client/tool/toolviews/todo-row.tsx:47`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/toolviews/todo-row.tsx#L47) | `const todos = (parsed as { todos?: unknown }).todos` |
| `todoToolview` | `const` | [`packages/client/ui-tool/src/client/tool/toolviews/todo-row.tsx:87`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/toolviews/todo-row.tsx#L87) | `export const todoToolview = {` |
| `nodes` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryView.tsx:147`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryView.tsx#L147) | `const nodes = inspection.eventNodes` |
| `nodes` | `const` | [`packages/compaction/compaction-basic/src/region.ts:316`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/region.ts#L316) | `const nodes = session.surface.nodes` |
| `nodes` | `const` | [`packages/core/session/src/index.ts:728`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L728) | `const nodes = surface.nodes` |
| `inject` | `const` | [`packages/fs/fs/src/invariant.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/invariant.ts#L12) | `export const inject = ['invariants']` |

### Tests and executable evidence

- [`packages/client/ui-conversation/tests/todo-panel.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/todo-panel.client.spec.tsx) — The source note names this file directly. A test under the owning area exercises or imports `dock`.
- [`apps/web/tests/steering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/steering.e2e.ts) — A test under the owning area exercises or imports `dock`.
- [`apps/web/tests/todo-row.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/todo-row.snapshot.ts) — A test under the owning area exercises or imports `todo_write`. A test under the owning area exercises or imports `dock`.
- [`apps/web/tests/skill-tool-row.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/skill-tool-row.e2e.ts) — A test under the owning area exercises or imports `toolview`.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `todo_write`.
- [`apps/web/tests/cordis-tool-round.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/cordis-tool-round.e2e.ts) — A test under the owning area exercises or imports `toolview`.
- [`apps/web/tests/shipped-composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/shipped-composition.e2e.ts) — A test under the owning area exercises or imports `todo_write`.
- [`packages/todo/tool-todo/tests/tool-todo.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/tool-todo.spec.ts) — A test under the owning area exercises or imports `todo_write`. A test under the owning area exercises or imports `todos`.

## How to read the implementation

1. Start with [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/feature`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/simplification`, `domain/agent-loop`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `inject`, `apply`, `ConversationSnapshot`, `ConversationController`, `ConversationRoot`, `todos`, `todoDockEntry`, `ToolRow`, `todoToolview`, `nodes`, `ToolEventView`, `todo_write`, `todo/write`, `ConversationSnapshot.nodes`
- Regex: `(?i)(inject|apply|ConversationSnapshot|ConversationController|ConversationRoot|todos|todoDockEntry|ToolRow)`

```bash
rg -n --pcre2 "(?i)(inject|apply|ConversationSnapshot|ConversationController|ConversationRoot|todos|todoDockEntry|ToolRow)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0205. Todo plan strip clears on the next turn](0205-todo-plan-strip-clears-on-the-next-turn.md): The source note links to this decision directly.
- **`shares-code-with`** — [0656. Fold the stdio UI helper into the stdio app](0656-fold-the-stdio-ui-helper-into-the-stdio-app.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0455. Remove implicit batching from ordinary sends](0455-remove-implicit-batching-from-ordinary-sends.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0573. TUI banner brand gradient](0573-tui-banner-brand-gradient.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0058. Separate context injection from turn execution](0058-separate-context-injection-from-turn-execution.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/fs/fs/src/invariant.ts`.
- **`shares-code-with`** — [0010. Filesystem capability seam --- ctx.fs, local backend, and model-facing filesystem tools](0010-filesystem-capability-seam-ctx-fs-local-backend-and-model-facing-filesys.md): Shares source implementation: `packages/fs/fs/src/invariant.ts`, `packages/fs/tool-fs/src/index.ts`.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/sdk/server/src/index.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/todo/tool-todo/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0179-web-todo-display-snapshot-side-effect-channel-two-render-surfaces.md`.
