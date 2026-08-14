---
id: "dsh-note-0205"
title: "Todo plan strip clears on the next turn"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-28-todo-plan-clears-on-next-turn.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/agent-loop"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
aliases:
  - "projections"
  - "apply"
  - "completed"
  - "todos"
  - "renderEvent"
  - "todo_write"
  - "todo/write"
  - "turn/start"
  - "turn/end"
  - "dsh-tool-todo"
  - "stateVersion"
  - "dsh-host-apiproxy"
  - "session/projection"
  - "useProjection"
search_regex: "(?i)(projections|apply|completed|todos|renderEvent|todo_write|todo/write|turn/start)"
---

# 0205. Todo plan strip clears on the next turn — implementation context

## Open this when

todo_write stores whole-list snapshots on the session log, and interactive hosts render the latest list as a plan strip (web TodoPanel via the todos projection, TUI Plan panel). After a turn finished, that strip stayed on screen into the next user turn --- a completed or abandoned checklist from the previous task. Readers treat the strip as "what this turn is doing," so a stale list across the turn boundary is the wrong product lifetime.

## Source decision

The standing plan is the latest todo/write that is not followed by a later turn/start. turn/end keeps the list visible so the finished checklist remains while the user reads the answer; the next turn/start clears it until the model writes again. dsh-tool-todo's todos projection unit folds the rule: apply takes the whole list from each todo/write and returns null on each turn/start (stateVersion 2). Carriers (dsh-host-apiproxy) serve that value on the history tail projections block and push session/projection frames; the web dock reads it through useProjection('todos').

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-28-todo-plan-clears-on-next-turn.md](../02-notes/implemented/feature/2026-07-28-todo-plan-clears-on-next-turn.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-28-todo-plan-clears-on-next-turn.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-28-todo-plan-clears-on-next-turn.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/host/apiproxy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/host/apiproxy`. | `named-package-member` |
| [`packages/todo/tool-todo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/index.ts) | package entry point | Core file in the package named by the note: `packages/todo/tool-todo`. Defines `todos`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/todo/tool-todo/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/todo/tool-todo`. | `named-package-member` |
| [`packages/host/apiproxy/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/host/apiproxy`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Core file in the package named by the note: `packages/host/apiproxy`. Defines `projections`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/todo/tool-todo/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/todo/tool-todo`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/host/apiproxy`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/todo/tool-todo`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `completed`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts) | runtime implementation | Defines `renderEvent`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/README.md) | package contract and examples | Core file in the package named by the note: `packages/host/apiproxy`. | `named-package-member` |
| [`packages/todo/tool-todo/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/README.md) | package contract and examples | Core file in the package named by the note: `packages/todo/tool-todo`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `projections` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1568`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1568) | `const projections = includeProjections ? detachedProjectionsFor(ctx, source.events) : undefined` |
| `projections` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1572`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1572) | `const projections = includeProjections ? projectionsFor(ctx, source.session) : undefined` |
| `projections` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1729`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1729) | `const projections = listProjectionsFor(ctx, session.header, session)` |
| `projections` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1750`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1750) | `const projections = listProjectionsFor(ctx, meta, undefined)` |
| `apply` | `const` | [`packages/host/apiproxy/src/invariant.ts:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/invariant.ts#L32) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `completed` | `let` | [`packages/llm/llm/src/index.ts:872`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L872) | `let completed = false` |
| `todos` | `const` | [`packages/todo/tool-todo/src/index.ts:92`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/index.ts#L92) | `const todos: TodoItem[] = []` |
| `apply` | `function` | [`packages/todo/tool-todo/src/index.ts:128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/index.ts#L128) | `export function apply(ctx: Context, config: Config): void {` |
| `todos` | `const` | [`packages/todo/tool-todo/src/index.ts:207`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/index.ts#L207) | `const todos = toTodoList(args.todos, allowParallel)` |
| `apply` | `const` | [`packages/todo/tool-todo/src/invariant.ts:65`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/invariant.ts#L65) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `renderEvent` | `function` | [`packages/typert/generator/src/cordis-catalog.ts:955`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts#L955) | `function renderEvent(e: EventEntry, onPage: string, linkedTypePages: Readonly<Record<string, string>>): string[] {` |

### Tests and executable evidence

- [`packages/todo/tool-todo/tests/tool-todo.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/tool-todo.spec.ts) — A test under the owning area exercises or imports `todo_write`. A test under the owning area exercises or imports `todos`.
- [`packages/todo/tool-todo/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/invariant.spec.ts) — A test under the owning area exercises or imports `todos`. A test under the owning area exercises or imports `dsh-tool-todo`.
- [`packages/todo/tool-todo/tests/projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/projection.spec.ts) — A test under the owning area exercises or imports `todos`. A test under the owning area exercises or imports `dsh-tool-todo`.
- [`packages/todo/tool-todo/tests/integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/integration.spec.ts) — A test under the owning area exercises or imports `todo_write`. A test under the owning area exercises or imports `todos`.
- [`packages/host/apiproxy/tests/api-proxy-fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-fork.spec.ts) — A test under the owning area exercises or imports `dsh-host-apiproxy`.
- [`packages/host/apiproxy/tests/client-handler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/client-handler.spec.ts) — A test under the owning area exercises or imports `dsh-host-apiproxy`.
- [`packages/host/apiproxy/tests/api-proxy-blank.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-blank.spec.ts) — A test under the owning area exercises or imports `dsh-host-apiproxy`.
- [`packages/host/apiproxy/tests/api-proxy-rename.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-rename.spec.ts) — A test under the owning area exercises or imports `dsh-host-apiproxy`.

## How to read the implementation

1. Start with [`packages/host/apiproxy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/agent-loop`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/projection`
- Aliases: `projections`, `apply`, `completed`, `todos`, `renderEvent`, `todo_write`, `todo/write`, `turn/start`, `turn/end`, `dsh-tool-todo`, `stateVersion`, `dsh-host-apiproxy`, `session/projection`, `useProjection`
- Regex: `(?i)(projections|apply|completed|todos|renderEvent|todo_write|todo/write|turn/start)`

```bash
rg -n --pcre2 "(?i)(projections|apply|completed|todos|renderEvent|todo_write|todo/write|turn/start)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0137. The `todo_write` tool --- model task list as event-sourced session state](0137-the-todo-write-tool-model-task-list-as-event-sourced-session-state.md): The source note links to this decision directly.
- **`source-link`** — [0179. Web todo display --- snapshot side-effect channel + two render surfaces](0179-web-todo-display-snapshot-side-effect-channel-two-render-surfaces.md): The source note links to this decision directly.
- **`source-link`** — [0475. Remove the TUI package](0475-remove-the-tui-package.md): The source note links to this decision directly.
- **`shares-code-with`** — [0197. Web past-session search](0197-web-past-session-search.md): Shares source implementation: `packages/host/apiproxy`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0375. Bound cold blank-session verification](0375-bound-cold-blank-session-verification.md): Shares source implementation: `packages/host/apiproxy`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0513. Record last activity in the session index](0513-record-last-activity-in-the-session-index.md): Shares source implementation: `packages/host/apiproxy`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0668. Ship the TUI without `todo_write`; keep it a one-line opt-in](0668-ship-the-tui-without-todo-write-keep-it-a-one-line-opt-in.md): Shares source implementation: `packages/todo/tool-todo`, `packages/todo/tool-todo/src/index.ts`.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0205-todo-plan-strip-clears-on-the-next-turn.md`.
