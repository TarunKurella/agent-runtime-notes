---
id: "dsh-note-0226"
title: "Web tool-row unified expand and trajectory Inspect"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-30-web-tool-row-unified-expand-and-inspect.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/registry"
aliases:
  - "resultText"
  - "inspect"
  - "ToolCallViewProps"
  - "ToolRow"
  - "terminalFailed"
  - "toolRowModel"
  - "errorSummary"
  - "output"
  - "distIndex"
  - "isError:false"
  - "aria-expanded"
  - "stopPropagation"
  - "plainBody"
  - "ToolCallOwnerProps.inspect"
search_regex: "(?i)(resultText|inspect|ToolCallViewProps|ToolRow|terminalFailed|toolRowModel|errorSummary|output)"
---

# 0226. Web tool-row unified expand and trajectory Inspect — implementation context

## Open this when

The chat view's tool rows had drifted into per-surface interaction dialects: ToolRow expanded through a leading-icon toggle and only for calls with an args body, the bash sample had its own expand affordance, todo/ask-question rows expanded raw args only, single-file tools were not expandable at all, and a call's OUTPUT was reachable only through the details panel. A failing bash command (exit≠0 settles isError:false) showed no collapsed-row failure signal.

## Source decision

Every expandable tool row shares one interaction --- the whole row toggles (click / Enter / Space) with an icon→chevron hover preview --- and one expanded body: an IN/OUT gutter-labeled card with per-section scroll caps; a hover-revealed Inspect pill jumps to the call's trajectory record through a one-shot store handoff; the chat view preserves its semantic reading position across view switches through an in-memory per-session map.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-30-web-tool-row-unified-expand-and-inspect.md](../02-notes/implemented/feature/2026-07-30-web-tool-row-unified-expand-and-inspect.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-30-web-tool-row-unified-expand-and-inspect.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-30-web-tool-row-unified-expand-and-inspect.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/cosmokit/src/string.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cosmokit/src/string.ts) | runtime implementation | Defines `output`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `output`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts) | package entry point | Defines `output`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) | package entry point | Defines `output`, a construct named by the note. | `symbol-definition` |
| [`packages/host/frontend-static/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/frontend-static/src/index.ts) | package entry point | Defines `distIndex`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-skill/src/client/SkillRow.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-skill/src/client/SkillRow.tsx) | runtime implementation | Defines `resultText`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `resultText`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-tool/src/client/contract/slots.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/contract/slots.ts) | runtime implementation | Defines `ToolCallViewProps`, a construct named by the note. | `symbol-definition` |
| [`packages/extensions/ui-cordis/src/client/card-model.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/card-model.ts) | runtime implementation | Defines `resultText`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx) | runtime implementation | Defines `resultText`, a construct named by the note. | `symbol-definition` |
| [`packages/extensions/cordis-client-runner/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-client-runner/src/client/index.ts) | package entry point | Defines `inspect`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx) | runtime implementation | Defines `ToolRow`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `resultText` | `const` | [`packages/client/connection/src/client/fixture.ts:734`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L734) | `const resultText = event.data.message.content[0].content.map(b => (b.type === 'text' ? b.text : '')).join('')` |
| `inspect` | `const` | [`packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx:151`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx#L151) | `const inspect = useStore(s => s.inspect ?? null)` |
| `resultText` | `function` | [`packages/client/ui-skill/src/client/SkillRow.tsx:50`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-skill/src/client/SkillRow.tsx#L50) | `function resultText(block: ToolCallViewProps['block']): string \| null {` |
| `ToolCallViewProps` | `type` | [`packages/client/ui-tool/src/client/contract/slots.ts:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/contract/slots.ts#L44) | `export type ToolCallViewProps = PropsRuntime<'tool.call.toolview'>` |
| `ToolRow` | `function` | [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx:128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx#L128) | `export function ToolRow({` |
| `terminalFailed` | `function` | [`packages/client/ui-tool/src/client/tool/models/terminal-card-model.ts:72`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/models/terminal-card-model.ts#L72) | `export function terminalFailed(model: TerminalCardModel): boolean {` |
| `resultText` | `function` | [`packages/client/ui-tool/src/client/tool/models/tool-call-model.ts:107`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/models/tool-call-model.ts#L107) | `export function resultText(node: ToolResultNode): string {` |
| `toolRowModel` | `function` | [`packages/client/ui-tool/src/client/tool/models/tool-call-model.ts:211`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/models/tool-call-model.ts#L211) | `export function toolRowModel(toolName: string, block: ToolCallBlock, cwd?: string): ToolRowModel {` |
| `errorSummary` | `const` | [`packages/client/ui-tool/src/client/tool/models/tool-call-model.ts:229`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/models/tool-call-model.ts#L229) | `const errorSummary = state === 'error' && output !== null ? firstLine(output) : null` |
| `resultText` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx:989`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx#L989) | `const resultText = useMemo(` |
| `output` | `const` | [`packages/core/tools/src/index.ts:1039`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1039) | `const output = (definition as Partial<ToolDefinition>).output` |
| `output` | `const` | [`packages/core/tools/src/index.ts:1243`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1243) | `const output = snapshotJsonValue(definition.output.schema)` |
| `inspect` | `const` | [`packages/extensions/cordis-client-runner/src/client/index.ts:189`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-client-runner/src/client/index.ts#L189) | `const inspect = new ClientCordisInspectRegistry({` |
| `resultText` | `function` | [`packages/extensions/ui-cordis/src/client/card-model.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/card-model.ts#L71) | `function resultText(block: Extract<Block, { kind: 'tool-result' }>): string \| null {` |
| `distIndex` | `const` | [`packages/host/frontend-static/src/index.ts:94`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/frontend-static/src/index.ts#L94) | `const distIndex = config.distIndex` |
| `output` | `const` | [`packages/sdk/server/src/index.ts:55`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts#L55) | `const output = config.output ?? process.stdout` |

### Tests and executable evidence

- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `aria-expanded`.
- [`apps/web/tests/workflow-run.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/workflow-run.e2e.ts) — A test under the owning area exercises or imports `aria-expanded`.
- [`apps/web/tests/queue-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/queue-actions.e2e.ts) — A test under the owning area exercises or imports `aria-expanded`.
- [`apps/web/tests/todo-row.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/todo-row.snapshot.ts) — A test under the owning area exercises or imports `aria-expanded`.
- [`apps/web/tests/pwsh-terminal.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/pwsh-terminal.e2e.ts) — A test under the owning area exercises or imports `aria-expanded`.
- [`apps/web/tests/schedule-after.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/schedule-after.e2e.ts) — A test under the owning area exercises or imports `aria-expanded`.
- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — A test under the owning area exercises or imports `aria-expanded`.
- [`apps/web/tests/skill-tool-row.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/skill-tool-row.e2e.ts) — A test under the owning area exercises or imports `aria-expanded`.

## How to read the implementation

1. Start with [`vendor/cosmokit/src/string.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cosmokit/src/string.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/registry`
- Aliases: `resultText`, `inspect`, `ToolCallViewProps`, `ToolRow`, `terminalFailed`, `toolRowModel`, `errorSummary`, `output`, `distIndex`, `isError:false`, `aria-expanded`, `stopPropagation`, `plainBody`, `ToolCallOwnerProps.inspect`
- Regex: `(?i)(resultText|inspect|ToolCallViewProps|ToolRow|terminalFailed|toolRowModel|errorSummary|output)`

```bash
rg -n --pcre2 "(?i)(resultText|inspect|ToolCallViewProps|ToolRow|terminalFailed|toolRowModel|errorSummary|output)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0560. Subagent lifecycle enrichment --- lastAssistantMessage (observe-only)](0560-subagent-lifecycle-enrichment-lastassistantmessage-observe-only.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/sdk/server/src/index.ts`.
- **`shares-code-with`** — [0576. TUI footer shows the session cache hit rate](0576-tui-footer-shows-the-session-cache-hit-rate.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/sdk/server/src/index.ts`.
- **`shares-code-with`** — [0461. Collapse agent-loop events around the observable state machine](0461-collapse-agent-loop-events-around-the-observable-state-machine.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0517. Pre-tool input rewrite --- a consistent design](0517-pre-tool-input-rewrite-a-consistent-design.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/shell/tool-bash/src/index.ts`.
- **`shares-code-with`** — [0611. Keep the Code Mode result card complete](0611-keep-the-code-mode-result-card-complete.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0190. Allow several `in_progress` todos at once](0190-allow-several-in-progress-todos-at-once.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`.
- **`shares-code-with`** — [0220. Web read card frontend --- the read tool's line window renders line-numbered and highlighted](0220-web-read-card-frontend-the-read-tool-s-line-window-renders-line-numbered.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`.
- **`shares-code-with`** — [0165. Typed tool returns in Code Mode](0165-typed-tool-returns-in-code-mode.md): Shares source implementation: `packages/core/tools/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0226-web-tool-row-unified-expand-and-trajectory-inspect.md`.
