---
id: "dsh-note-0627"
title: "TUI long-session render costs --- shared step-timing scan and card line caches"
status: "archived"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/bug-fix/2026-08-03-tui-long-session-render-costs.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
aliases:
  - "invalidate"
  - "render"
  - "stepTimingAt"
  - "ToolCardComponent.render"
  - "ContextCardComponent.render"
  - "new Text"
  - "new Markdown"
  - "packages/ui/tui/src/chat/timing.ts"
  - "StepTimingTracker"
  - "createTuiChat"
  - "StreamingAssistantComponent"
  - "StepTimingComponent"
  - "step/end"
  - "ToolCardComponent"
search_regex: "(?i)(invalidate|render|stepTimingAt|ToolCardComponent\\.render|ContextCardComponent\\.render|new[- ]Text|new[- ]Markdown|packages/ui/tui/src/chat/timing\\.ts)"
---

# 0627. TUI long-session render costs --- shared step-timing scan and card line caches — implementation context

## Open this when

On a long resumed session (196k events, 2.2k steps, 1.8k tool cards) the TUI took ~12 s to render the transcript and ~800 ms to echo one keystroke. Profiling attributed both to the render path, not to session load (zstd + parse + surface seed is ~1.7 s): Every step's timing footer called stepTimingAt, which replayed the whole event log from index 0 per footer --- O(steps × events) on the initial render, ~6 s of CPU. pi-tui re-renders every component each frame and relies on per-component line caches (its own Text/Markdown cache by (text, width)).

## Source decision

packages/ui/tui/src/chat/timing.ts replaces stepTimingAt with StepTimingTracker: one accumulator per chat mount, created in createTuiChat and threaded through StreamingAssistantComponent into each StepTimingComponent. A query advances a cursor over events appended since the previous query and keeps per-step bucket state in a map, so all footers together cost O(events). The open bucket is accumulated to the query clock at lookup, and a step is pinned at its step/end. The tracker requires the append-only session log (the seq = log length contract).

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/bug-fix/2026-08-03-tui-long-session-render-costs.md](../02-notes/archived/bug-fix/2026-08-03-tui-long-session-render-costs.md)
- Pinned source: [.agents/notes/archived/bug-fix/2026-08-03-tui-long-session-render-costs.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/bug-fix/2026-08-03-tui-long-session-render-costs.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/cordis-core-api.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-core-api.ts) | repository automation | Defines `render`, a construct named by the note. | `symbol-definition` |
| [`scripts/gen-tool-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-tool-catalog.ts) | repository automation | Defines `render`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/error.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts) | runtime implementation | Defines `render`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `render`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-skill/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-skill/src/client/index.ts) | package entry point | Defines `invalidate`, a construct named by the note. | `symbol-definition` |
| [`packages/code-runtime/code-runtime-worker-thread/src/bootstrap.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/bootstrap.ts) | runtime implementation | Defines `render`, a construct named by the note. | `symbol-definition` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `step/end` named by the note. Contains the exact code literal `step/start` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `step/end` named by the note. Contains the exact code literal `step/start` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `step/end` named by the note. Contains the exact code literal `step/start` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Contains the exact code literal `step/end` named by the note. Contains the exact code literal `step/start` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.zh.md) | package contract and examples | Contains the exact code literal `step/end` named by the note. Contains the exact code literal `step/start` named by the note. | `exact-code-occurrence` |
| [`docs/persistence-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/persistence-catalog.md) | package contract and examples | Contains the exact code literal `step/end` named by the note. Contains the exact code literal `step/start` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `invalidate` | `const` | [`packages/client/ui-skill/src/client/index.ts:117`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-skill/src/client/index.ts#L117) | `const invalidate = (key: SessionId): void => {` |
| `render` | `const` | [`packages/code-runtime/code-runtime-worker-thread/src/bootstrap.ts:113`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/bootstrap.ts#L113) | `const render = (args: unknown[]): string =>` |
| `render` | `const` | [`packages/core/tools/src/index.ts:886`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L886) | `const render = SDK_RENDERERS[runtime.language]` |
| `render` | `const` | [`packages/llm/llm/src/error.ts:118`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts#L118) | `const render = (current: unknown): string => {` |
| `render` | `const` | [`scripts/cordis-core-api.ts:300`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-core-api.ts#L300) | `const render = (groups: Map<string, Member[]>, prefix: string): MemberDoc[] =>` |
| `render` | `function` | [`scripts/gen-tool-catalog.ts:685`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-tool-catalog.ts#L685) | `export function render(catalog: ToolCatalog): string {` |

### Tests and executable evidence

- [`scripts/verify-md-links.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-md-links.spec.ts) — A test under the owning area exercises or imports `Markdown`.
- [`scripts/project-doc-site.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.spec.ts) — A test under the owning area exercises or imports `Markdown`.
- [`scripts/translation-prompt.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-prompt.spec.ts) — A test under the owning area exercises or imports `Markdown`.
- [`packages/core/tools/tests/py-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/py-types.spec.ts) — A test under the owning area exercises or imports `findIndex`.
- [`apps/web/tests/subagent-conversation.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/subagent-conversation.e2e.ts) — A test under the owning area exercises or imports `findIndex`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `findIndex`.
- [`packages/sdk/server/tests/plugin-apply.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/tests/plugin-apply.spec.ts) — A test under the owning area exercises or imports `findIndex`.
- [`packages/core/agent-loop/tests/tool-calls.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/tool-calls.spec.ts) — A test under the owning area exercises or imports `findIndex`.

## How to read the implementation

1. Start with [`scripts/cordis-core-api.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-core-api.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `domain/agent-loop`, `domain/build-release`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/projection`
- Aliases: `invalidate`, `render`, `stepTimingAt`, `ToolCardComponent.render`, `ContextCardComponent.render`, `new Text`, `new Markdown`, `packages/ui/tui/src/chat/timing.ts`, `StepTimingTracker`, `createTuiChat`, `StreamingAssistantComponent`, `StepTimingComponent`, `step/end`, `ToolCardComponent`
- Regex: `(?i)(invalidate|render|stepTimingAt|ToolCardComponent\.render|ContextCardComponent\.render|new[- ]Text|new[- ]Markdown|packages/ui/tui/src/chat/timing\.ts)`

```bash
rg -n --pcre2 "(?i)(invalidate|render|stepTimingAt|ToolCardComponent\\.render|ContextCardComponent\\.render|new[- ]Text|new[- ]Markdown|packages/ui/tui/src/chat/timing\\.ts)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0018. Web capability seam - stable tools over multiple providers](0018-web-capability-seam-stable-tools-over-multiple-providers.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/llm/llm/src/error.ts`.
- **`shares-code-with`** — [0654. Drop `GenerateOptions.prefill` and `ToolSchema.strict` --- request knobs with no working end-to-end path](0654-drop-generateoptions-prefill-and-toolschema-strict-request-knobs-with-no.md): Shares source implementation: `packages/core/tools/src/index.ts`, `scripts/gen-tool-catalog.ts`.
- **`shares-code-with`** — [0494. Property-based testing for protocol-shaped code](0494-property-based-testing-for-protocol-shaped-code.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/llm/llm/src/error.ts`.
- **`shares-code-with`** — [0137. The `todo_write` tool --- model task list as event-sourced session state](0137-the-todo-write-tool-model-task-list-as-event-sourced-session-state.md): Shares source implementation: `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0425. The documentation site carries its own images](0425-the-documentation-site-carries-its-own-images.md): Shares source implementation: `scripts/project-doc-site.spec.ts`, `scripts/verify-md-links.spec.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0561. Plan mode --- a logged per-agent session mode](0561-plan-mode-a-logged-per-agent-session-mode.md): Shares source implementation: `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/tools/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0627-tui-long-session-render-costs-shared-step-timing-scan-and-card-line-cach.md`.
