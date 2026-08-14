---
id: "dsh-note-0137"
title: "The `todo_write` tool --- model task list as event-sourced session state"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-06-29-todo-write-tool.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "surfaceOp"
  - "plan"
  - "SurfaceEventType"
  - "inject"
  - "SessionEventMap"
  - "todos"
  - "presentCall"
  - "todo_write"
  - "todo/write"
  - "ConversationSnapshot.todos"
  - "update_plan"
  - "pending | in_progress | completed"
  - "PlanEntryStatus"
  - "turn/start"
search_regex: "(?i)(surfaceOp|plan|SurfaceEventType|inject|SessionEventMap|todos|presentCall|todo_write)"
---

# 0137. The `todo_write` tool --- model task list as event-sourced session state — implementation context

## Open this when

The harness gives the model bash and subagent tools but no way to record a structured task list. A todo list serves two co-equal purposes: it steers the model to plan multi-step work and keep the active work unambiguous, and it gives an interactive host a live progress checklist. Every reference coding agent surveyed (claude-code, opencode, codex, oh-my-pi, pi) ships some form of this; the harness had nothing.

## Source decision

Add a model-facing todo_write(todos: [{ content, status }]) tool whose whole-list state lives on the event-sourced session log as a new todo/write SessionEventMap variant. Interactive hosts render from the durable event: the TUI folds it directly, the web client projects it into ConversationSnapshot.todos (web todo display), while the automation-only ACP bridge deliberately omits todo presentation. The list is appended as a todo/write event carrying the full { todos } snapshot.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-06-29-todo-write-tool.md](../02-notes/implemented/feature/2026-06-29-todo-write-tool.md)
- Pinned source: [.agents/notes/implemented/feature/2026-06-29-todo-write-tool.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-06-29-todo-write-tool.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/gen-cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. Defines `SessionEventMap`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/testing.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/testing.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/tools`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/todo/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/todo`. | `named-directory-member` |
| [`packages/todo/tool-todo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/todo`. Defines `todos`, a construct named by the note. | `named-directory-member, symbol-definition` |
| [`packages/todo/tool-todo/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/todo`. | `named-directory-member` |
| [`packages/todo`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/todo) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/core/tools`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Defines `SurfaceEventType`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/surface.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts) | runtime implementation | Defines `plan`, a construct named by the note. Defines `surfaceOp`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `surfaceOp` | `const` | [`packages/core/session/src/surface.ts:331`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L331) | `const surfaceOp = surfaceOpOf(event)` |
| `plan` | `const` | [`packages/core/session/src/surface.ts:357`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L357) | `const plan = planSurfaceEvent(state, event, expectedSeq, events, baseSeq)` |
| `SurfaceEventType` | `type` | [`packages/core/session/src/types.ts:343`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L343) | `export type SurfaceEventType =` |
| `inject` | `const` | [`packages/core/tools/src/invariant.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts#L13) | `export const inject = ['invariants']` |
| `SessionEventMap` | `interface` | [`packages/core/tools/src/types.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts#L26) | `interface SessionEventMap {` |
| `todos` | `const` | [`packages/todo/tool-todo/src/index.ts:92`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/index.ts#L92) | `const todos: TodoItem[] = []` |
| `presentCall` | `function` | [`packages/workflow/tool-ralph/src/index.ts:394`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts#L394) | `function presentCall(args: RalphCallArgs): ToolCallView {` |

### Tests and executable evidence

- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `presentCall`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `SessionEventMap`. A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/session/tests/surface.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/surface.spec.ts) — A test under the owning area exercises or imports `SurfaceEventType`.
- [`packages/todo/tool-todo/tests/tool-todo.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/tool-todo.spec.ts) — A test under the owning area exercises or imports `todos`.
- [`packages/todo/tool-todo/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/invariant.spec.ts) — A test under the owning area exercises or imports `todos`.
- [`packages/todo/tool-todo/tests/projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/projection.spec.ts) — A test under the owning area exercises or imports `todos`.
- [`packages/todo/tool-todo/tests/integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/integration.spec.ts) — A test under the owning area exercises or imports `todos`.
- [`packages/core/tools/tests/gen-tool-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/gen-tool-catalog.spec.ts) — A test under the owning area exercises or imports `todo_write`. A test under the owning area exercises or imports `todos`.
- Source verification intent: Four tiers: Unit --- the session event (append/snapshot-clone/last-write-wins/not-on-surface); the tool (schema shape, arg validation via the real ctx.tools.execute, value validation, the event append + replacement, no-agent rejection, presentCall, HMR-safety); and TUI folding. Real-Loader path --- the plugin run through Loader.unwrapExports, asserting the namespace export shape survives (it HAS inject, so a stray default would crash at load --- postmortem/0001). Full-loop integration --- a scripted mock model calls todo_write through the real agent loop; the todo/write event lands and a second call replaces it.

## How to read the implementation

1. Start with [`scripts/gen-cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/agent-loop`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`
- Aliases: `surfaceOp`, `plan`, `SurfaceEventType`, `inject`, `SessionEventMap`, `todos`, `presentCall`, `todo_write`, `todo/write`, `ConversationSnapshot.todos`, `update_plan`, `pending | in_progress | completed`, `PlanEntryStatus`, `turn/start`
- Regex: `(?i)(surfaceOp|plan|SurfaceEventType|inject|SessionEventMap|todos|presentCall|todo_write)`

```bash
rg -n --pcre2 "(?i)(surfaceOp|plan|SurfaceEventType|inject|SessionEventMap|todos|presentCall|todo_write)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0179. Web todo display --- snapshot side-effect channel + two render surfaces](0179-web-todo-display-snapshot-side-effect-channel-two-render-surfaces.md): The source note links to this decision directly.
- **`source-link`** — [0190. Allow several `in_progress` todos at once](0190-allow-several-in-progress-todos-at-once.md): The source note links to this decision directly.
- **`source-link`** — [0205. Todo plan strip clears on the next turn](0205-todo-plan-strip-clears-on-the-next-turn.md): The source note links to this decision directly.
- **`source-link`** — [0459. ACP as an automation-only protocol](0459-acp-as-an-automation-only-protocol.md): The source note links to this decision directly.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0165. Typed tool returns in Code Mode](0165-typed-tool-returns-in-code-mode.md): Shares source implementation: `packages/core/tools`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0032. Tool output spill policy](0032-tool-output-spill-policy.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0137-the-todo-write-tool-model-task-list-as-event-sourced-session-state.md`.
