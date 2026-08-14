---
id: "dsh-note-0190"
title: "Allow several `in_progress` todos at once"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-26-todo-parallel-in-progress.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "ToolRow"
  - "planSummary"
  - "done"
  - "total"
  - "summary"
  - "Config"
  - "suffix"
  - "find"
  - "pending"
  - "in_progress"
  - "todo_write"
  - "packages/todo/tool-todo/src/index.ts"
  - "Config.allowParallelInProgress"
  - "allowParallelInProgress"
search_regex: "(?i)(ToolRow|planSummary|done|total|summary|Config|suffix|find)"
---

# 0190. Allow several `in_progress` todos at once — implementation context

## Open this when

The original todo_write design enforced at most one in_progress task per list, both in execute and in the durable-log invariant. That invariant assumes sequential work, but the harness runs genuinely parallel work --- concurrent subagents through the delegation tool, background bash commands, workflow fan-out --- and a list that can name only one active task cannot represent it. The model was forced to either mislabel parallel tasks as pending or merge them into one vague item, and the UI progress checklist under-reported what was actually running.

## Source decision

Make the single-in_progress cap a deployment policy instead of a fixed rule, requiring every composition to choose: packages/todo/tool-todo/src/index.ts gains the required Config.allowParallelInProgress field. At true, execute accepts any number of active items and the description instructs the model to mark every actively-worked task --- several during parallel work, one for sequential work --- keeping at least one while work remains. At false, the description asks for exactly one and execute rejects a call marking more.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-26-todo-parallel-in-progress.md](../02-notes/implemented/feature/2026-07-26-todo-parallel-in-progress.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-26-todo-parallel-in-progress.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-26-todo-parallel-in-progress.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/todo/tool-todo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/index.ts) | package entry point | The source note names this file directly. Core file in the package named by the note: `packages/todo/tool-todo`. | `named-file, named-package-member, symbol-definition` |
| [`packages/todo/tool-todo/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/invariant.ts) | runtime contract checks | The source note names this file directly. Core file in the package named by the note: `packages/todo/tool-todo`. | `named-file, named-package-member` |
| [`packages/todo/tool-todo/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/todo/tool-todo`. | `named-package-member` |
| [`packages/client/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/client`. | `named-directory-member` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client`. | `named-directory-member` |
| [`packages/client/ui-conversation/src/client/service.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/service.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client`. | `named-directory-member` |
| [`packages/client/ui-conversation/src/client/chat/ChatView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client`. | `named-directory-member` |
| [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client`. | `named-directory-member` |
| [`packages/client/ui-primitives/src/markdown/mathCompatibility.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/mathCompatibility.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client`. | `named-directory-member` |
| [`packages/client/ui-workflow-run/src/client/WorkflowRunPanel.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workflow-run/src/client/WorkflowRunPanel.tsx) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client`. | `named-directory-member` |
| [`packages/client/ui-settings-models/src/client/DeepSeekModelsEditor.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/DeepSeekModelsEditor.tsx) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client`. | `named-directory-member` |
| [`packages/client`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client) | package or module directory | The source note names this implementation area directly. | `named-directory` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ToolRow` | `function` | [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx:128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx#L128) | `export function ToolRow({` |
| `planSummary` | `function` | [`packages/client/ui-tool/src/client/tool/toolviews/plan-summary.ts:50`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/toolviews/plan-summary.ts#L50) | `export function planSummary(todos: readonly PlanItemLike[]): PlanSummary {` |
| `done` | `const` | [`packages/core/agent-loop/src/agent.ts:144`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L144) | `const done = Promise.withResolvers<void>()` |
| `total` | `let` | [`packages/fs/fs-local/src/fsio.ts:704`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L704) | `let total = 0` |
| `summary` | `const` | [`packages/skill/tool-skill/src/index.ts:134`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts#L134) | `const summary = (await ctx.skills.list(lookup)).find(skill => skill.name === args.name)` |
| `Config` | `interface` | [`packages/todo/tool-todo/src/index.ts:29`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/index.ts#L29) | `export interface Config {` |
| `Config` | `const` | [`packages/todo/tool-todo/src/index.ts:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/index.ts#L41) | `export const Config: z<Config> = z.object({` |
| `suffix` | `const` | [`packages/web/tool-web/src/search.ts:64`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/search.ts#L64) | `const suffix = meta.length > 0 ? \` — ${meta.join(' ')}\` : ''` |
| `find` | `const` | [`scripts/rescope-vendor.ts:615`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/rescope-vendor.ts#L615) | `const find = reverse ? edit.replace : edit.find` |
| `pending` | `const` | [`vendor/hmr/src/index.ts:346`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L346) | `const pending: string[] = []` |

### Tests and executable evidence

- [`apps/web/tests/todo-row.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/todo-row.snapshot.ts) — The source note names this file directly.
- [`packages/client/ui-conversation/tests/todo-panel.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/todo-panel.client.spec.tsx) — The source note names this file directly.
- [`packages/todo/tool-todo/tests/tool-todo.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/tool-todo.spec.ts) — A test under the owning area exercises or imports `in_progress`. A test under the owning area exercises or imports `todo_write`.
- [`packages/todo/tool-todo/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/invariant.spec.ts) — A test under the owning area exercises or imports `in_progress`. A test under the owning area exercises or imports `allowParallelInProgress`.
- [`packages/todo/tool-todo/tests/projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/projection.spec.ts) — A test under the owning area exercises or imports `in_progress`. A test under the owning area exercises or imports `allowParallelInProgress`.
- [`packages/todo/tool-todo/tests/integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/integration.spec.ts) — A test under the owning area exercises or imports `in_progress`. A test under the owning area exercises or imports `todo_write`.
- [`packages/client/ui-tool/tests/todo-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/todo-row.client.spec.tsx) — A test under the owning area exercises or imports `planSummary`.
- [`packages/client/ui-tool/tests/web-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/web-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.

## How to read the implementation

1. Start with [`packages/todo/tool-todo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/ownership`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `ToolRow`, `planSummary`, `done`, `total`, `summary`, `Config`, `suffix`, `find`, `pending`, `in_progress`, `todo_write`, `packages/todo/tool-todo/src/index.ts`, `Config.allowParallelInProgress`, `allowParallelInProgress`
- Regex: `(?i)(ToolRow|planSummary|done|total|summary|Config|suffix|find)`

```bash
rg -n --pcre2 "(?i)(ToolRow|planSummary|done|total|summary|Config|suffix|find)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0137. The `todo_write` tool --- model task list as event-sourced session state](0137-the-todo-write-tool-model-task-list-as-event-sourced-session-state.md): The source note links to this decision directly.
- **`source-link`** — [0179. Web todo display --- snapshot side-effect channel + two render surfaces](0179-web-todo-display-snapshot-side-effect-channel-two-render-surfaces.md): The source note links to this decision directly.
- **`shares-code-with`** — [0668. Ship the TUI without `todo_write`; keep it a one-line opt-in](0668-ship-the-tui-without-todo-write-keep-it-a-one-line-opt-in.md): Shares source implementation: `packages/todo/tool-todo/src/index.ts`, `packages/todo/tool-todo/src/invariant.ts`.
- **`shares-code-with`** — [0468. Simplify Web image input version one](0468-simplify-web-image-input-version-one.md): Shares source implementation: `packages/client/ui-conversation/src/client/service.ts`, `packages/todo/tool-todo/src/index.ts`.
- **`shares-code-with`** — [0205. Todo plan strip clears on the next turn](0205-todo-plan-strip-clears-on-the-next-turn.md): Shares source implementation: `packages/todo/tool-todo/src/index.ts`, `packages/todo/tool-todo/src/invariant.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/todo/tool-todo/src/index.ts`.
- **`shares-code-with`** — [0385. Generated tool-schema catalog (boot-and-harvest)](0385-generated-tool-schema-catalog-boot-and-harvest.md): Shares source implementation: `packages/todo/tool-todo/src/index.ts`, `packages/todo/tool-todo/src/types.ts`.
- **`shares-code-with`** — [0113. Client Conversation business-node assembly and keyed Chat snapshots](0113-client-conversation-business-node-assembly-and-keyed-chat-snapshots.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/ChatView.tsx`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0190-allow-several-in-progress-todos-at-once.md`.
