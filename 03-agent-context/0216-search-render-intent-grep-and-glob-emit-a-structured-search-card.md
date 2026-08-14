---
id: "dsh-note-0216"
title: "Search render intent --- grep and glob emit a structured search card"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-30-search-render-card.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "card"
  - "search"
  - "JsonValue"
  - "PostToolDecision"
  - "ToolCallKind"
  - "GenericCallView"
  - "ToolResultView"
  - "SearchLineMatch"
  - "SearchFileMatches"
  - "SearchMatchesResultView"
  - "SearchPathsResultView"
  - "SearchResultView"
  - "total"
  - "presentGlobResult"
search_regex: "(?i)(card|search|JsonValue|PostToolDecision|ToolCallKind|GenericCallView|ToolResultView|SearchLineMatch)"
---

# 0216. Search render intent --- grep and glob emit a structured search card — implementation context

## Open this when

grep and glob return structured canonical values --- grep a flat { matches: [{ path, lineNumber, line }] }, glob a { paths: string[] } --- but every UI only ever saw their model-facing render text: grep groups its matches under file headers with Line N: rows, glob prints a newline-joined path list, and both append a spill footer when the inline cap (grepMaxMatches, default 250; globMaxResults, default 100) drops later results to a spill file. A web frontend that wants to render a search result as an expandable per-file group of matches, or as a selectable path list, had to re-parse that text.

## Source decision

packages/core/tools/src/presentation.ts adds card: 'search' to the ToolResultView union as SearchResultView, a shape-discriminated view that expresses both tools' shapes: SearchMatchesResultView (shape: 'matches') carries grep's matches grouped by file as files: { path, matches: { lineNumber, line }[] }[], and SearchPathsResultView (shape: 'paths') carries glob's flat paths: string[]. Both carry truncated: boolean and total: number.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-30-search-render-card.md](../02-notes/implemented/feature/2026-07-30-search-render-card.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-30-search-render-card.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-30-search-render-card.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/tools/src/presentation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts) | runtime implementation | The source note names this file directly. Defines `GenericCallView`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/fs/tool-fs-search/src/presentation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/presentation.ts) | runtime implementation | The source note names this file directly. Defines `grepSearchMeta`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/spill/spill-policy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/spill/spill-policy`. Defines `maxInlineBytes`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/spill/spill-policy/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/spill/spill-policy`. | `named-package-member` |
| [`packages/spill/spill-policy/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/spill/spill-policy`. | `named-package-member` |
| [`packages/spill/spill-policy`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/cordis/src/fiber.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts) | runtime implementation | Defines `meta`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/src/diff.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/diff.ts) | runtime implementation | Defines `diffsFromMeta`, a construct named by the note. Defines `diffs`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/fs-local/src/fsio.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts) | runtime implementation | Defines `total`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `PostToolDecision`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/json.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/json.ts) | runtime implementation | Defines `JsonValue`, a construct named by the note. | `symbol-definition` |
| [`packages/web/tool-web/src/fetch.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts) | runtime implementation | Defines `truncated`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `card` | `const` | [`packages/client/ui-primitives/src/HoverCard.tsx:140`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx#L140) | `const card = cardRef.current` |
| `search` | `const` | [`packages/client/ui-tool/src/client/tool/ToolDetails.tsx:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/ToolDetails.tsx#L41) | `const search = searchCardModel(block)` |
| `JsonValue` | `type` | [`packages/core/session/src/json.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/json.ts#L13) | `export type JsonValue = null \| boolean \| number \| string \| JsonValue[] \| { [key: string]: JsonValue }` |
| `PostToolDecision` | `type` | [`packages/core/tools/src/index.ts:597`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L597) | `export type PostToolDecision =` |
| `ToolCallKind` | `type` | [`packages/core/tools/src/presentation.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts#L15) | `export type ToolCallKind = 'read' \| 'edit' \| 'delete' \| 'move' \| 'search' \| 'execute' \| 'fetch' \| 'other'` |
| `GenericCallView` | `interface` | [`packages/core/tools/src/presentation.ts:53`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts#L53) | `export interface GenericCallView {` |
| `ToolResultView` | `type` | [`packages/core/tools/src/presentation.ts:140`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts#L140) | `export type ToolResultView = GenericResultView \| TerminalResultView \| DiffResultView \| SearchResultView \| ReadResultView \| WebResultView` |
| `SearchLineMatch` | `interface` | [`packages/core/tools/src/presentation.ts:193`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts#L193) | `export interface SearchLineMatch {` |
| `SearchFileMatches` | `interface` | [`packages/core/tools/src/presentation.ts:201`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts#L201) | `export interface SearchFileMatches {` |
| `SearchMatchesResultView` | `interface` | [`packages/core/tools/src/presentation.ts:216`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts#L216) | `export interface SearchMatchesResultView {` |
| `SearchPathsResultView` | `interface` | [`packages/core/tools/src/presentation.ts:238`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts#L238) | `export interface SearchPathsResultView {` |
| `SearchResultView` | `type` | [`packages/core/tools/src/presentation.ts:267`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts#L267) | `export type SearchResultView = SearchMatchesResultView \| SearchPathsResultView` |
| `total` | `let` | [`packages/fs/fs-local/src/fsio.ts:704`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L704) | `let total = 0` |
| `presentGlobResult` | `function` | [`packages/fs/tool-fs-search/src/glob.ts:283`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/glob.ts#L283) | `export function presentGlobResult(_args: { pattern: string; path?: string }, result: ToolResult): SearchResultView \| undefined {` |
| `presentGrepResult` | `function` | [`packages/fs/tool-fs-search/src/grep.ts:258`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/grep.ts#L258) | `export function presentGrepResult(` |
| `SearchMeta` | `type` | [`packages/fs/tool-fs-search/src/presentation.ts:60`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/presentation.ts#L60) | `export type SearchMeta =` |

### Tests and executable evidence

- [`packages/fs/tool-fs-search/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/tests/tools.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `presentGrepResult`.
- [`packages/fs/tool-fs-search/tests/presentation.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/tests/presentation.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `grepSearchMeta`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/fs/tool-fs/tests/diff.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/diff.spec.ts) — A test under the owning area exercises or imports `diffsFromMeta`. A test under the owning area exercises or imports `diffs`.
- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `PostToolDecision`.
- [`packages/fs/tool-fs/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/tools.spec.ts) — A test under the owning area exercises or imports `diffs`.
- Source verification intent: packages/fs/tool-fs-search/tests/presentation.spec.ts pins the pure layer: groupMatchesByFile's first-seen file order; grepSearchMeta/globSearchMeta projection over a shared retention outcome with total reporting the pre-cap count and truncated carried through; the per-line preview budget the retention pass applied; the serialized-meta byte cap dropping trailing groups/paths while keeping a single oversized item; and searchViewFromMeta's narrowing of both good shapes, the zero-result empty card, and every malformed case (non-object/array meta, missing or mistyped truncated/total, unknown shape, malformed files.

## How to read the implementation

1. Start with [`packages/core/tools/src/presentation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `card`, `search`, `JsonValue`, `PostToolDecision`, `ToolCallKind`, `GenericCallView`, `ToolResultView`, `SearchLineMatch`, `SearchFileMatches`, `SearchMatchesResultView`, `SearchPathsResultView`, `SearchResultView`, `total`, `presentGlobResult`
- Regex: `(?i)(card|search|JsonValue|PostToolDecision|ToolCallKind|GenericCallView|ToolResultView|SearchLineMatch)`

```bash
rg -n --pcre2 "(?i)(card|search|JsonValue|PostToolDecision|ToolCallKind|GenericCallView|ToolResultView|SearchLineMatch)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0023. Tagged render-intent union for tool-call presentation](0023-tagged-render-intent-union-for-tool-call-presentation.md): The source note links to this decision directly.
- **`source-link`** — [0049. Canonical tool output contract](0049-canonical-tool-output-contract.md): The source note links to this decision directly.
- **`source-link`** — [0207. Web terminal card --- the bash render intent reaches the browser](0207-web-terminal-card-the-bash-render-intent-reaches-the-browser.md): The source note links to this decision directly.
- **`shares-code-with`** — [0186. Spilling the durable copy of Code Mode sub-dispatch results](0186-spilling-the-durable-copy-of-code-mode-sub-dispatch-results.md): Shares source implementation: `packages/spill/spill-policy`, `packages/spill/spill-policy/src/index.ts`.
- **`shares-code-with`** — [0247. Web search source card scrolls instead of collapsing](0247-web-search-source-card-scrolls-instead-of-collapsing.md): Shares source implementation: `packages/spill/spill-policy/src/index.ts`, `packages/spill/spill-policy/src/invariant.ts`.
- **`shares-code-with`** — [0151. Parallel tool-call execution by per-call safety](0151-parallel-tool-call-execution-by-per-call-safety.md): Shares source implementation: `packages/typert/generator/tests/fixtures/type-model/packages/write`, `packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`.
- **`shares-code-with`** — [0212. Current sandbox policy context](0212-current-sandbox-policy-context.md): Shares source implementation: `packages/typert/generator/tests/fixtures/type-model/packages/write`, `packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`.
- **`shares-code-with`** — [0284. A minimal read_image tool over existing seams](0284-a-minimal-read-image-tool-over-existing-seams.md): Shares source implementation: `packages/typert/generator/tests/fixtures/type-model/packages/write`, `packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0216-search-render-intent-grep-and-glob-emit-a-structured-search-card.md`.
