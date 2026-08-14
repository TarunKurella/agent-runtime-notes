---
id: "dsh-note-0668"
title: "Ship the TUI without `todo_write`; keep it a one-line opt-in"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-07-21-tui-todo-write-opt-in.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/evidence"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/projection"
aliases:
  - "TodoItem"
  - "render"
  - "jsonl"
  - "todo_write"
  - "cordis.yml"
  - "@deepseek-ai/dsh-tool-todo"
  - "packages/ui/tui/src/index.ts"
  - "todo/write"
  - "TodoComponent.render"
  - "tool-todo"
  - "code-mode.cordis.yml"
  - "~/.dsh"
  - "@deepseek-ai/dsh-session"
  - "TodoComponent"
search_regex: "(?i)(TodoItem|render|jsonl|todo_write|cordis\\.yml|@deepseek\\-ai/dsh\\-tool\\-todo|packages/ui/tui/src/index\\.ts|todo/write)"
---

# 0668. Ship the TUI without `todo_write`; keep it a one-line opt-in — implementation context

## Open this when

The shipped tui-agent cordis.yml loaded @deepseek-ai/dsh-tool-todo, exposing todo_write by default. The tool is a task-tracking convenience, not a core coding affordance like bash or the read/write/edit fs tools; most TUI sessions never call it, yet shipping it enlarges the wire tool list and system prompt for every turn. Meanwhile the TUI's plan rendering is event-driven: packages/ui/tui/src/index.ts listens for the todo/write session event and TodoComponent.render returns nothing when the list is empty, so the front door already tolerates the tool being absent or present with no runtime coupling to the plugin.

## Source decision

The tui-agent cordis.yml no longer loads tool-todo; todo_write is opt-in. The code-mode.cordis.yml overlay inherits the base composition, so its generated SDK drops todo_write too. Enabling it is one entry --- add @deepseek-ai/dsh-tool-todo to cordis.yml (or a ~/.dsh personal overlay) --- after which the model logs the whole-list todo/write snapshot and the TUI renders the plan, unchanged. The TodoItem type and the todo/write event stay in @deepseek-ai/dsh-session and the TUI's plan rendering stays wired, so both the default (disabled) and opt-in (enabled) paths are first-class.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-07-21-tui-todo-write-opt-in.md](../02-notes/archived/simplification/2026-07-21-tui-todo-write-opt-in.md)
- Pinned source: [.agents/notes/archived/simplification/2026-07-21-tui-todo-write-opt-in.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-07-21-tui-todo-write-opt-in.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | The source note names this file directly. Defines `jsonl`, a construct named by the note. | `exact-code-occurrence, named-file, symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. Defines `TodoItem`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/todo/tool-todo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/index.ts) | package entry point | Core file in the package named by the note: `packages/todo/tool-todo`. | `named-package-member` |
| [`packages/todo/tool-todo/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/todo/tool-todo`. | `named-package-member` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/todo/tool-todo/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/todo/tool-todo`. | `named-package-member` |
| [`packages/core/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/session) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/todo/tool-todo`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/llm/src/error.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts) | runtime implementation | Defines `render`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/todo/tool-todo/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/README.md) | package contract and examples | Core file in the package named by the note: `packages/todo/tool-todo`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `TodoItem` | `interface` | [`packages/core/session/src/types.ts:189`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L189) | `export interface TodoItem {` |
| `render` | `const` | [`packages/llm/llm/src/error.ts:118`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts#L118) | `const render = (current: unknown): string => {` |
| `jsonl` | `const` | [`scripts/gen-doc-graphs.ts:731`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts#L731) | `const jsonl = nodeId('bundle', 'jsonl')` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `TodoItem`.
- [`packages/todo/tool-todo/tests/tool-todo.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/tool-todo.spec.ts) — A test under the owning area exercises or imports `todo_write`. A test under the owning area exercises or imports `tool-todo`.
- [`packages/todo/tool-todo/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/invariant.spec.ts) — A test under the owning area exercises or imports `ToolTodo`.
- [`packages/todo/tool-todo/tests/projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/projection.spec.ts) — A test under the owning area exercises or imports `tool-todo`. A test under the owning area exercises or imports `TodoItem`.
- [`packages/todo/tool-todo/tests/integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/integration.spec.ts) — A test under the owning area exercises or imports `todo_write`. A test under the owning area exercises or imports `ToolTodo`.
- Source verification intent: examples/tui-agent/tests/tui.snapshot.ts mounts ToolTodo only when a scenario sets enableTodo: only the todo-plan scenario does (the enabled-path proof, whose session.jsonl/terminal.expected.txt pin the rendered plan), while every other scenario runs the default todo-free composition. tests/harness.ts makes ToolTodo a todo opt-in that only tests/todo-write.e2e.ts sets, so the with-key todo e2e still drives the real tool while the other suites match the shipped stack. The keyless tests/tui-keyless-smoke.e2e.ts boots the real cordis.yml and asserts nothing about todo, so the default boot is unaffected.

## How to read the implementation

1. Start with [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/evidence`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/projection`
- Aliases: `TodoItem`, `render`, `jsonl`, `todo_write`, `cordis.yml`, `@deepseek-ai/dsh-tool-todo`, `packages/ui/tui/src/index.ts`, `todo/write`, `TodoComponent.render`, `tool-todo`, `code-mode.cordis.yml`, `~/.dsh`, `@deepseek-ai/dsh-session`, `TodoComponent`
- Regex: `(?i)(TodoItem|render|jsonl|todo_write|cordis\.yml|@deepseek\-ai/dsh\-tool\-todo|packages/ui/tui/src/index\.ts|todo/write)`

```bash
rg -n --pcre2 "(?i)(TodoItem|render|jsonl|todo_write|cordis\\.yml|@deepseek\\-ai/dsh\\-tool\\-todo|packages/ui/tui/src/index\\.ts|todo/write)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0447. Fold trace-only session facts into load-bearing events](0447-fold-trace-only-session-facts-into-load-bearing-events.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0061. Web client Agent-scope parity model and the provisioning channel (agents/scope / blank reuse / provide)](0061-web-client-agent-scope-parity-model-and-the-provisioning-channel-agents.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0038. Initiating Agent scope over AsyncLocalStorage](0038-initiating-agent-scope-over-asynclocalstorage.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0504. Frame-coalesced reasoning-chunk publication and browser stress validation](0504-frame-coalesced-reasoning-chunk-publication-and-browser-stress-validatio.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0331. Resume selector folds titles only](0331-resume-selector-folds-titles-only.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0231. Permission Settings default for new sessions](0231-permission-settings-default-for-new-sessions.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0668-ship-the-tui-without-todo-write-keep-it-a-one-line-opt-in.md`.
