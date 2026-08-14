---
id: "dsh-note-0609"
title: "Card tool rows collapse through one ToolRow"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-31-web-cards-toolrow.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/llm"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/projection"
aliases:
  - "web"
  - "DetailsPanel"
  - "DiffBlock"
  - "DisclosureRow"
  - "ReadBlock"
  - "SearchRow"
  - "SearchBlock"
  - "WebBlock"
  - "search"
  - "ToolRow"
  - "errorSummary"
  - "GenericToolCard"
  - "AskQuestionRow"
  - "BashRow"
search_regex: "(?i)(DetailsPanel|DiffBlock|DisclosureRow|ReadBlock|SearchRow|SearchBlock|WebBlock|search)"
---

# 0609. Card tool rows collapse through one ToolRow — implementation context

## Open this when

The Web client grew five card render intents over successive PRs --- terminal, diff, read, search, web --- each landing as a keyed toolview registrant under packages/client/ui-conversation/src/client/toolviews/. They diverged in two ways the earlier PRs each acknowledged but deferred: Chrome duplication. read-row, search-row, web-row, and file-mutation-row each hand-drew the summary row (leading state slot, visually-hidden status, title, separator dot, path-link/summary) as their own with a private .module.css, instead of composing the shared ToolRow.

## Source decision

ToolRow owns every card kind, and every keyed card row composes it. ToolRow already took terminal and diff card material; it now also takes read, search, and web, rendering whichever is present in its collapsed-by-default expanded body through the matching primitive (capped at the chat CHAT_ bounds). A call carries at most one card kind, so the props are mutually exclusive and the body picks the first present.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-31-web-cards-toolrow.md](../02-notes/archived/feature/2026-07-31-web-cards-toolrow.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-31-web-cards-toolrow.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-31-web-cards-toolrow.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/terminal/terminal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/index.ts) | package entry point | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/terminal/terminal/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/terminal/terminal/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`apps/web`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/web) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/terminal/terminal`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal) | package or module directory | The note names this package or capability. | `named-package` |
| [`apps/cli/src/args.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts) | runtime implementation | Defines `web`, a construct named by the note. | `symbol-definition` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `children`, a construct named by the note. | `symbol-definition` |
| [`vendor/cosmokit/src/string.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cosmokit/src/string.ts) | runtime implementation | Defines `output`, a construct named by the note. | `symbol-definition` |
| [`vendor/loader/src/config/entry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/entry.ts) | runtime implementation | Defines `diff`, a construct named by the note. | `symbol-definition` |
| [`packages/jobs/jobs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/invariant.ts) | runtime contract checks | Defines `terminal`, a construct named by the note. | `symbol-definition` |
| [`packages/lsp/tool-lsp/src/render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/tool-lsp/src/render.ts) | runtime implementation | Defines `filePath`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/WebBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/WebBlock.tsx) | runtime implementation | Defines `WebBlock`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `web` | `const` | [`apps/cli/src/args.ts:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts#L156) | `const web = program.command('web').description('boot the web profile (alias of --profile web); the web app\'s own flags follow')` |
| `DetailsPanel` | `function` | [`packages/client/ui-conversation/src/client/skeleton/DetailsPanel.tsx:66`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/DetailsPanel.tsx#L66) | `export function DetailsPanel({ useSession, useSessions, sessionId, useStore, renderSlot, closeDetails, t }: DetailsPanelProps) {` |
| `DiffBlock` | `function` | [`packages/client/ui-primitives/src/DiffBlock.tsx:141`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DiffBlock.tsx#L141) | `export function DiffBlock({ diffs, maxLines = DEFAULT_DIFF_MAX_LINES, className }: DiffBlockProps) {` |
| `DisclosureRow` | `function` | [`packages/client/ui-primitives/src/DisclosureRow.tsx:33`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DisclosureRow.tsx#L33) | `export function DisclosureRow({` |
| `ReadBlock` | `function` | [`packages/client/ui-primitives/src/ReadBlock.tsx:70`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ReadBlock.tsx#L70) | `export function ReadBlock({` |
| `SearchRow` | `type` | [`packages/client/ui-primitives/src/SearchBlock.tsx:81`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/SearchBlock.tsx#L81) | `type SearchRow =` |
| `SearchBlock` | `function` | [`packages/client/ui-primitives/src/SearchBlock.tsx:173`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/SearchBlock.tsx#L173) | `export function SearchBlock(props: SearchBlockProps) {` |
| `WebBlock` | `function` | [`packages/client/ui-primitives/src/WebBlock.tsx:200`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/WebBlock.tsx#L200) | `export function WebBlock(props: WebBlockProps) {` |
| `search` | `const` | [`packages/client/ui-tool/src/client/tool/ToolDetails.tsx:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/ToolDetails.tsx#L41) | `const search = searchCardModel(block)` |
| `ToolRow` | `function` | [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx:128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx#L128) | `export function ToolRow({` |
| `errorSummary` | `const` | [`packages/client/ui-tool/src/client/tool/models/tool-call-model.ts:229`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/models/tool-call-model.ts#L229) | `const errorSummary = state === 'error' && output !== null ? firstLine(output) : null` |
| `GenericToolCard` | `function` | [`packages/client/ui-tool/src/client/tool/toolviews/GenericToolCard.tsx:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/toolviews/GenericToolCard.tsx#L36) | `export function GenericToolCard({ toolName, block, cwd, openFile, inspect, t }: GenericToolCardProps) {` |
| `AskQuestionRow` | `function` | [`packages/client/ui-tool/src/client/tool/toolviews/ask-question-row.tsx:47`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/toolviews/ask-question-row.tsx#L47) | `export function AskQuestionRow({ toolName, block, inspect, t }: AskQuestionRowProps) {` |
| `BashRow` | `function` | [`packages/client/ui-tool/src/client/tool/toolviews/bash-sample.tsx:56`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/toolviews/bash-sample.tsx#L56) | `export function BashRow({ toolName, block, sessionId, useSessions, inspect, t }: BashRowProps) {` |
| `FileMutationRow` | `function` | [`packages/client/ui-tool/src/client/tool/toolviews/file-mutation-row.tsx:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/toolviews/file-mutation-row.tsx#L32) | `export function FileMutationRow({ toolName, block, cwd, openFile, inspect, t }: FileMutationRowProps) {` |
| `ReadRow` | `function` | [`packages/client/ui-tool/src/client/tool/toolviews/read-row.tsx:27`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/toolviews/read-row.tsx#L27) | `export function ReadRow({ toolName, block, cwd, openFile, inspect, t }: ReadRowProps) {` |

### Tests and executable evidence

- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `diff`. A test under the owning area exercises or imports `search`.
- [`apps/web/tests/workflow-run.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/workflow-run.e2e.ts) — A test under the owning area exercises or imports `children`.
- [`apps/web/tests/todo-row.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/todo-row.snapshot.ts) — A test under the owning area exercises or imports `ToolRow`.
- [`apps/web/tests/pwsh-terminal.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/pwsh-terminal.e2e.ts) — A test under the owning area exercises or imports `search`.
- [`apps/web/tests/plugin-config.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/plugin-config.e2e.ts) — A test under the owning area exercises or imports `search`.
- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — A test under the owning area exercises or imports `diff`.
- [`apps/web/tests/produced-files.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/produced-files.e2e.ts) — A test under the owning area exercises or imports `css`.
- [`apps/web/tests/built-boot.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/built-boot.snapshot.ts) — A test under the owning area exercises or imports `ToolRow`. A test under the owning area exercises or imports `WebBlock`.

## How to read the implementation

1. Start with [`packages/terminal/terminal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
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

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/llm`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/projection`
- Aliases: `web`, `DetailsPanel`, `DiffBlock`, `DisclosureRow`, `ReadBlock`, `SearchRow`, `SearchBlock`, `WebBlock`, `search`, `ToolRow`, `errorSummary`, `GenericToolCard`, `AskQuestionRow`, `BashRow`
- Regex: `(?i)(DetailsPanel|DiffBlock|DisclosureRow|ReadBlock|SearchRow|SearchBlock|WebBlock|search)`

```bash
rg -n --pcre2 "(?i)(DetailsPanel|DiffBlock|DisclosureRow|ReadBlock|SearchRow|SearchBlock|WebBlock|search)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0470. Capability-neutral sandbox policy context](0470-capability-neutral-sandbox-policy-context.md): Shares source implementation: `packages/jobs/jobs/src/invariant.ts`, `packages/terminal/terminal`.
- **`shares-code-with`** — [0683. Snapshot semantic terminal state for the TUI](0683-snapshot-semantic-terminal-state-for-the-tui.md): Shares source implementation: `packages/terminal/terminal`, `packages/terminal/terminal/src/index.ts`.
- **`shares-code-with`** — [0225. Web search card --- the grep and glob render intent reaches the browser](0225-web-search-card-the-grep-and-glob-render-intent-reaches-the-browser.md): Shares source implementation: `packages/terminal/terminal/src/index.ts`, `packages/terminal/terminal/src/invariant.ts`.
- **`shares-code-with`** — [0223. Web result card frontend --- rendering the web render intent in the browser](0223-web-result-card-frontend-rendering-the-web-render-intent-in-the-browser.md): Shares source implementation: `packages/terminal/terminal/src/index.ts`, `packages/terminal/terminal/src/invariant.ts`.
- **`shares-code-with`** — [0218. Web diff card --- the write/edit render intent reaches the browser](0218-web-diff-card-the-write-edit-render-intent-reaches-the-browser.md): Shares source implementation: `packages/terminal/terminal/src/index.ts`, `packages/terminal/terminal/src/invariant.ts`.
- **`shares-code-with`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares source implementation: `packages/terminal/terminal/src/index.ts`, `packages/terminal/terminal/src/invariant.ts`.
- **`shares-code-with`** — [0342. Load sessions from the pre-react-loop format](0342-load-sessions-from-the-pre-react-loop-format.md): Shares source implementation: `apps/cli/src/args.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0207. Web terminal card --- the bash render intent reaches the browser](0207-web-terminal-card-the-bash-render-intent-reaches-the-browser.md): Shares source implementation: `packages/terminal/terminal/src/index.ts`, `packages/terminal/terminal/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0609-card-tool-rows-collapse-through-one-toolrow.md`.
