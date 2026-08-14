---
id: "dsh-note-0589"
title: "TUI QuestionDialog renders options across multiple lines"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-24-tui-question-dialog-multiline.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/shell-terminal"
  - "domain/ui-interaction"
  - "lifecycle/archived"
aliases:
  - "detail"
  - "ctx.userInteraction.ask"
  - "InlineModalComponent"
  - "questionDialogWidth"
  - "questionDialogMaxHeight"
  - "renderOptionBlock"
  - "↑ N lines hidden"
  - "… lines A-B/N • PgUp/PgDn"
  - "windowBlocks"
  - "maxQuestionOptions"
  - "↑ N more"
  - "↓ N more"
  - "lines A-B/N • PgUp/PgDn"
  - "TUI QuestionDialog renders options across multiple lines"
search_regex: "(?i)(detail|ctx\\.userInteraction\\.ask|InlineModalComponent|questionDialogWidth|questionDialogMaxHeight|renderOptionBlock|↑[- ]N[- ]lines[- ]hidden|…[- ]lines[- ]A\\-B/N[- ]•[- ]PgUp/PgDn)"
---

# 0589. TUI QuestionDialog renders options across multiple lines — implementation context

## Open this when

ctx.userInteraction.ask() must keep question text, supporting detail, option labels, descriptions, validation, and controls readable inside configured width and height bounds. The question panel also belongs directly above the editor: placing it at the terminal edge separates the pending decision from both the transcript that prompted it and the input that follows it.

## Source decision

The TUI renders a pending question as an inline modal between the transcript/status area and the editor while retaining the shared FIFO with model and plugin overlays: InlineModalComponent applies questionDialogWidth and questionDialogMaxHeight inside the normal component flow. The effective question height is additionally clamped to the current viewport after reserving the editor, so the editor remains below the question during resize. renderOptionBlock wraps each label beneath its cursor/number prefix and renders the muted description on separately wrapped, equally indented lines.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-24-tui-question-dialog-multiline.md](../02-notes/archived/feature/2026-07-24-tui-question-dialog-multiline.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-24-tui-question-dialog-multiline.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-24-tui-question-dialog-multiline.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `detail`, a construct named by the note. | `symbol-definition` |
| [`vendor/loader/src/config/entry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/entry.ts) | runtime implementation | Defines `detail`, a construct named by the note. | `symbol-definition` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Defines `detail`, a construct named by the note. | `symbol-definition` |
| [`packages/jobs/tool-jobs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts) | package entry point | Defines `detail`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `detail` | `const` | [`packages/acp/acp/src/index.ts:314`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L314) | `const detail = error instanceof Error ? error.message : String(error)` |
| `detail` | `const` | [`packages/acp/acp/src/index.ts:393`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L393) | `const detail = failures.map(failure => errorChain(failure)).join('; ')` |
| `detail` | `const` | [`packages/boot/app-boot/src/index.ts:791`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L791) | `const detail = cause instanceof Error ? cause.message : String(cause)` |
| `detail` | `const` | [`packages/jobs/tool-jobs/src/index.ts:148`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L148) | `const detail = \` (${snapshot.kind}: ${snapshot.label}) finished ${statusLine(snapshot)}\`` |
| `detail` | `const` | [`vendor/loader/src/config/entry.ts:25`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/entry.ts#L25) | `const detail = cause instanceof Error ? cause.message : String(cause)` |

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.

## How to read the implementation

1. Start with [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/shell-terminal`, `domain/ui-interaction`, `lifecycle/archived`
- Aliases: `detail`, `ctx.userInteraction.ask`, `InlineModalComponent`, `questionDialogWidth`, `questionDialogMaxHeight`, `renderOptionBlock`, `↑ N lines hidden`, `… lines A-B/N • PgUp/PgDn`, `windowBlocks`, `maxQuestionOptions`, `↑ N more`, `↓ N more`, `lines A-B/N • PgUp/PgDn`, `TUI QuestionDialog renders options across multiple lines`
- Regex: `(?i)(detail|ctx\.userInteraction\.ask|InlineModalComponent|questionDialogWidth|questionDialogMaxHeight|renderOptionBlock|↑[- ]N[- ]lines[- ]hidden|…[- ]lines[- ]A\-B/N[- ]•[- ]PgUp/PgDn)`

```bash
rg -n --pcre2 "(?i)(detail|ctx\\.userInteraction\\.ask|InlineModalComponent|questionDialogWidth|questionDialogMaxHeight|renderOptionBlock|\u2191[- ]N[- ]lines[- ]hidden|\u2026[- ]lines[- ]A\\-B/N[- ]\u2022[- ]PgUp/PgDn)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares source implementation: `packages/acp/acp/src/index.ts`.
- **`shares-code-with`** — [0459. ACP as an automation-only protocol](0459-acp-as-an-automation-only-protocol.md): Shares source implementation: `packages/acp/acp/src/index.ts`.
- **`shares-code-with`** — [0200. Continuable subagents](0200-continuable-subagents.md): Shares source implementation: `packages/acp/acp/src/index.ts`.
- **`shares-code-with`** — [0144. The approval seam --- one-shot permission decisions over a waterfall of answerers](0144-the-approval-seam-one-shot-permission-decisions-over-a-waterfall-of-answ.md): Shares source implementation: `packages/acp/acp/src/index.ts`.
- **`shares-code-with`** — [0045. Web client architecture --- the client cordis plugin tree, the slot system, and the React-free object layer](0045-web-client-architecture-the-client-cordis-plugin-tree-the-slot-system-an.md): Shares source implementation: `vendor/loader/src/config/entry.ts`.
- **`shares-code-with`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): Shares source implementation: `vendor/loader/src/config/entry.ts`.
- **`shares-code-with`** — [0015. The background job runtime (`ctx.jobs`) and generic task control tools](0015-the-background-job-runtime-ctx-jobs-and-generic-task-control-tools.md): Shares source implementation: `packages/jobs/tool-jobs/src/index.ts`.
- **`shares-code-with`** — [0342. Load sessions from the pre-react-loop format](0342-load-sessions-from-the-pre-react-loop-format.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `vendor/loader/src/config/entry.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0589-tui-questiondialog-renders-options-across-multiple-lines.md`.
