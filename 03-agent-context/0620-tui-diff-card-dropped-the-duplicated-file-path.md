---
id: "dsh-note-0620"
title: "TUI diff card dropped the duplicated file path"
status: "archived"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/bug-fix/2026-07-27-tui-diff-card-redundant-path-header.md"
implementation_evidence: "medium"
target_anchor: "optional UI or client layer"
tags:
  - "class/bug-fix"
  - "concern/evidence"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
aliases:
  - "FileDiff"
  - "renderBody"
  - "presentCall"
  - "presentResult"
  - "path"
  - "diffLines"
  - "palette.bold"
  - "showPath"
  - "ToolCardComponent.renderBody"
  - "resultView?.title ?? callView.title"
  - "String.includes"
  - "tui.spec.ts"
  - "a.txt"
  - "b.txt"
search_regex: "(?i)(FileDiff|renderBody|presentCall|presentResult|path|diffLines|palette\\.bold|showPath)"
---

# 0620. TUI diff card dropped the duplicated file path — implementation context

## Open this when

The edit and write tool cards printed the target path twice. Each tool's presentCall/presentResult returns a diff card whose title is Edit /Write and whose single FileDiff carries the same path. The TUI's diffLines unconditionally rendered palette.bold(diff.path) as a per-file header, so a one-file edit rendered: The existing snapshot fixture hid the bug: it titled the edit card Edit renderer (no path) and gave the result two diffs, so the title never matched a diff path and the header never looked redundant.

## Source decision

diffLines takes a showPath flag; ToolCardComponent.renderBody suppresses the per-file header for a diff card when there is exactly one diff and the effective card title (resultView?.title ?? callView.title) already contains that diff's path. Multi-file diff cards keep every per-file header. An empty or blank diff path collapses under the same String.includes check, which is the intended noise removal.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/bug-fix/2026-07-27-tui-diff-card-redundant-path-header.md](../02-notes/archived/bug-fix/2026-07-27-tui-diff-card-redundant-path-header.md)
- Pinned source: [.agents/notes/archived/bug-fix/2026-07-27-tui-diff-card-redundant-path-header.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/bug-fix/2026-07-27-tui-diff-card-redundant-path-header.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `path`, a construct named by the note. | `symbol-definition` |
| [`packages/web/tool-web/src/fetch.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts) | runtime implementation | Defines `renderBody`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/presentation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts) | runtime implementation | Defines `FileDiff`, a construct named by the note. | `symbol-definition` |
| [`packages/workflow/tool-ralph/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts) | package entry point | Defines `presentCall`, a construct named by the note. Defines `presentResult`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `FileDiff` | `interface` | [`packages/core/tools/src/presentation.ts:34`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts#L34) | `export interface FileDiff {` |
| `renderBody` | `function` | [`packages/web/tool-web/src/fetch.ts:224`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts#L224) | `function renderBody(body: WebFetchBody, maxInputChars: number): RenderedBody {` |
| `presentCall` | `function` | [`packages/workflow/tool-ralph/src/index.ts:394`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts#L394) | `function presentCall(args: RalphCallArgs): ToolCallView {` |
| `presentResult` | `function` | [`packages/workflow/tool-ralph/src/index.ts:398`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts#L398) | `function presentResult(args: RalphCallArgs, result: { content: ContentBlock[]; isError: boolean }): ToolResultView {` |
| `path` | `const` | [`vendor/hmr/src/index.ts:514`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L514) | `const path = relative(this.baseDir, fileURLToPath(filename))` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/workflow/tool-ralph/tests/tool-ralph.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/tests/tool-ralph.spec.ts) — A test under the owning area exercises or imports `presentCall`. A test under the owning area exercises or imports `presentResult`.
- Source verification intent: tui.spec.ts adds a focused case asserting the path appears exactly once for a single-diff card titled Edit src/only.ts. The advanced-cards- keyless snapshots re-recorded to show the title line immediately followed by the diff body with no repeated path header.

## How to read the implementation

1. Start with [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** optional UI or client layer.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/evidence`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`
- Aliases: `FileDiff`, `renderBody`, `presentCall`, `presentResult`, `path`, `diffLines`, `palette.bold`, `showPath`, `ToolCardComponent.renderBody`, `resultView?.title ?? callView.title`, `String.includes`, `tui.spec.ts`, `a.txt`, `b.txt`
- Regex: `(?i)(FileDiff|renderBody|presentCall|presentResult|path|diffLines|palette\.bold|showPath)`

```bash
rg -n --pcre2 "(?i)(FileDiff|renderBody|presentCall|presentResult|path|diffLines|palette\\.bold|showPath)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0206. Tool-call file open in OS](0206-tool-call-file-open-in-os.md): Shares source implementation: `packages/typert/generator/tests/fixtures/type-model/packages/write`, `packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`.
- **`shares-code-with`** — [0216. Search render intent --- grep and glob emit a structured search card](0216-search-render-intent-grep-and-glob-emit-a-structured-search-card.md): Shares source implementation: `packages/core/tools/src/presentation.ts`, `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- **`shares-code-with`** — [0151. Parallel tool-call execution by per-call safety](0151-parallel-tool-call-execution-by-per-call-safety.md): Shares source implementation: `packages/typert/generator/tests/fixtures/type-model/packages/write`, `packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`.
- **`shares-code-with`** — [0611. Keep the Code Mode result card complete](0611-keep-the-code-mode-result-card-complete.md): Shares source implementation: `packages/workflow/tool-ralph/src/index.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0478. Copy-only preset authoring, and the way into a preset's files](0478-copy-only-preset-authoring-and-the-way-into-a-preset-s-files.md): Shares source implementation: `packages/typert/generator/tests/fixtures/type-model/packages/write`, `packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`.
- **`shares-code-with`** — [0617. Intent draft echoes in the same tick](0617-intent-draft-echoes-in-the-same-tick.md): Shares source implementation: `packages/workflow/tool-ralph/src/index.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0594. Fixed `Tool / <name>` header for tool-call cards](0594-fixed-tool-name-header-for-tool-call-cards.md): Shares source implementation: `packages/workflow/tool-ralph/src/index.ts`, `packages/workflow/tool-ralph/tests/tool-ralph.spec.ts`.
- **`shares-code-with`** — [0661. Trim unreachable ACP bridge surface --- the branding knobs and the kind-sniffing fallback](0661-trim-unreachable-acp-bridge-surface-the-branding-knobs-and-the-kind-snif.md): Shares source implementation: `packages/typert/generator/tests/fixtures/type-model/packages/write`, `packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0620-tui-diff-card-dropped-the-duplicated-file-path.md`.
