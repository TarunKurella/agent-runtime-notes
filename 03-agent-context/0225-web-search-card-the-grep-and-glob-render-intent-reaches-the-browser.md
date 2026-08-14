---
id: "dsh-note-0225"
title: "Web search card --- the grep and glob render intent reaches the browser"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-30-web-search-card.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/context"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "ConversationSnapshot"
  - "paths"
  - "card"
  - "DEFAULT_SEARCH_MAX_LINES"
  - "SearchMatchesBlockProps"
  - "SearchPathsBlockProps"
  - "SearchRow"
  - "SearchBlock"
  - "TerminalBlock"
  - "CodeBlock"
  - "search"
  - "ToolRow"
  - "CHAT_SEARCH_MAX_LINES"
  - "searchCardModel"
search_regex: "(?i)(ConversationSnapshot|paths|card|DEFAULT_SEARCH_MAX_LINES|SearchMatchesBlockProps|SearchPathsBlockProps|SearchRow|SearchBlock)"
---

# 0225. Web search card --- the grep and glob render intent reaches the browser — implementation context

## Open this when

The grep and glob tools declare a result-time card: 'search' render intent (search render card): a SearchMatchesResultView (shape: 'matches') carrying grep's matches grouped by file, or a SearchPathsResultView (shape: 'paths') carrying glob's flat path list, both with a truncated/total capping signal. That view already reaches the browser --- host, connection, and runtime deliver it onto ConversationSnapshot as resultView --- but the Web client ignored it: every non-terminal, non-diff tool result fell through to the generic card, which renders the model-facing text.

## Source decision

SearchBlock is a ui-primitives component that renders a completed search as either shape, and the Web render sites for a grep/glob call consume the search render intent through it. ui-tool/src/client/tool/models/search-card-model.ts is the single place that turns the snapshot's resultView into the component's props, so no render site re-derives the shape.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-30-web-search-card.md](../02-notes/implemented/feature/2026-07-30-web-search-card.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-30-web-search-card.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-30-web-search-card.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | The source note names this file directly. Contains the exact code literal `packages/client/ui-tool/tests/search-card.client.spec.tsx` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/terminal/terminal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/index.ts) | package entry point | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/terminal/terminal/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/client/ui-primitives/src/ansi.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ansi.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/client/ui-primitives/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/terminal/terminal/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/client/ui-primitives/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/client/ui-primitives/src/HoverCard.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. Defines `card`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-primitives/src/SearchBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/SearchBlock.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. Defines `SearchBlock`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-primitives/src/TerminalBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/TerminalBlock.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. Defines `TerminalBlock`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-tool/src/client/tool/ToolDetails.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/ToolDetails.tsx) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client/ui-tool/src`. Defines `search`, a construct named by the note. | `named-directory-member, symbol-definition` |
| [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client/ui-tool/src`. Defines `ToolRow`, a construct named by the note. | `named-directory-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ConversationSnapshot` | `interface` | [`packages/client/runtime/src/client/sessions/conversation.ts:433`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts#L433) | `export interface ConversationSnapshot {` |
| `paths` | `const` | [`packages/client/ui-primitives/src/DiffBlock.tsx:78`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DiffBlock.tsx#L78) | `const paths = new Set<string>()` |
| `card` | `const` | [`packages/client/ui-primitives/src/HoverCard.tsx:140`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx#L140) | `const card = cardRef.current` |
| `card` | `const` | [`packages/client/ui-primitives/src/HoverCard.tsx:149`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx#L149) | `const card = open && pos !== null && (` |
| `DEFAULT_SEARCH_MAX_LINES` | `const` | [`packages/client/ui-primitives/src/SearchBlock.tsx:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/SearchBlock.tsx#L22) | `export const DEFAULT_SEARCH_MAX_LINES = 16` |
| `SearchMatchesBlockProps` | `interface` | [`packages/client/ui-primitives/src/SearchBlock.tsx:58`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/SearchBlock.tsx#L58) | `export interface SearchMatchesBlockProps extends SearchBlockCommon {` |
| `SearchPathsBlockProps` | `interface` | [`packages/client/ui-primitives/src/SearchBlock.tsx:65`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/SearchBlock.tsx#L65) | `export interface SearchPathsBlockProps extends SearchBlockCommon {` |
| `SearchRow` | `type` | [`packages/client/ui-primitives/src/SearchBlock.tsx:81`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/SearchBlock.tsx#L81) | `type SearchRow =` |
| `SearchBlock` | `function` | [`packages/client/ui-primitives/src/SearchBlock.tsx:173`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/SearchBlock.tsx#L173) | `export function SearchBlock(props: SearchBlockProps) {` |
| `TerminalBlock` | `function` | [`packages/client/ui-primitives/src/TerminalBlock.tsx:174`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/TerminalBlock.tsx#L174) | `export function TerminalBlock({` |
| `CodeBlock` | `function` | [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx#L26) | `export function CodeBlock({ code, lang, className, copyLabel = '复制', copiedLabel = '复制成功' }: CodeBlockProps) {` |
| `search` | `const` | [`packages/client/ui-tool/src/client/tool/ToolDetails.tsx:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/ToolDetails.tsx#L41) | `const search = searchCardModel(block)` |
| `ToolRow` | `function` | [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx:128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx#L128) | `export function ToolRow({` |
| `CHAT_SEARCH_MAX_LINES` | `const` | [`packages/client/ui-tool/src/client/tool/models/search-card-model.ts:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/models/search-card-model.ts#L45) | `export const CHAT_SEARCH_MAX_LINES = 8` |
| `searchCardModel` | `function` | [`packages/client/ui-tool/src/client/tool/models/search-card-model.ts:130`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/models/search-card-model.ts#L130) | `export function searchCardModel(block: ToolCallBlock): SearchCardModel \| null {` |
| `terminalCardModel` | `function` | [`packages/client/ui-tool/src/client/tool/models/terminal-card-model.ts:182`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/models/terminal-card-model.ts#L182) | `export function terminalCardModel(block: ToolCallBlock, sessionCwd?: string): TerminalCardModel \| null {` |

### Tests and executable evidence

- [`apps/web/tests/search-card.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/search-card.snapshot.ts) — The source note names this file directly.
- [`packages/client/ui-tool/tests/search-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/search-card.client.spec.tsx) — The source note names this file directly. A test under the owning area exercises or imports `searchCardModel`.
- [`packages/client/ui-primitives/tests/search-block.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/search-block.client.spec.tsx) — The source note names this file directly. A test under the owning area exercises or imports `grep`.
- [`apps/web/tests/snapshots/search-card`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/search-card) — The source note names this implementation area directly.
- [`packages/client/runtime/tests/session.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/session.client.spec.ts) — A test under the owning area exercises or imports `ConversationSnapshot`.
- [`packages/client/ui-tool/tests/web-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/web-card.client.spec.tsx) — A test under the owning area exercises or imports `BashRow`. A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/tool-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/tool-row.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`. A test under the owning area exercises or imports `GenericToolCard`.
- [`packages/client/ui-primitives/tests/ansi.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/ansi.client.spec.ts) — A test under the owning area exercises or imports `TerminalBlock`.
- Source verification intent: packages/client/ui-primitives/tests/search-block.client.spec.tsx pins the component at per-file 100%: both kinds, the folded pre-cap total in the summary, the empty arm, per-file collapse/re-expand without touching neighbours, a file header counting as one capped row alongside its matches, the tail slice restoring its owning file header when the cut falls mid-file, the head/tail cap and its expand control across both shapes and the no-tail and default-cap edges, and the copy control writing the whole structured result on the accepted and refused clipboard paths.

## How to read the implementation

1. Start with [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/context`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`
- Aliases: `ConversationSnapshot`, `paths`, `card`, `DEFAULT_SEARCH_MAX_LINES`, `SearchMatchesBlockProps`, `SearchPathsBlockProps`, `SearchRow`, `SearchBlock`, `TerminalBlock`, `CodeBlock`, `search`, `ToolRow`, `CHAT_SEARCH_MAX_LINES`, `searchCardModel`
- Regex: `(?i)(ConversationSnapshot|paths|card|DEFAULT_SEARCH_MAX_LINES|SearchMatchesBlockProps|SearchPathsBlockProps|SearchRow|SearchBlock)`

```bash
rg -n --pcre2 "(?i)(ConversationSnapshot|paths|card|DEFAULT_SEARCH_MAX_LINES|SearchMatchesBlockProps|SearchPathsBlockProps|SearchRow|SearchBlock)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0023. Tagged render-intent union for tool-call presentation](0023-tagged-render-intent-union-for-tool-call-presentation.md): The source note links to this decision directly.
- **`source-link`** — [0207. Web terminal card --- the bash render intent reaches the browser](0207-web-terminal-card-the-bash-render-intent-reaches-the-browser.md): The source note links to this decision directly.
- **`source-link`** — [0216. Search render intent --- grep and glob emit a structured search card](0216-search-render-intent-grep-and-glob-emit-a-structured-search-card.md): The source note links to this decision directly.
- **`source-link`** — [0226. Web tool-row unified expand and trajectory Inspect](0226-web-tool-row-unified-expand-and-trajectory-inspect.md): The source note links to this decision directly.
- **`shares-code-with`** — [0223. Web result card frontend --- rendering the web render intent in the browser](0223-web-result-card-frontend-rendering-the-web-render-intent-in-the-browser.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/client/ui-primitives/src/ansi.ts`.
- **`shares-code-with`** — [0218. Web diff card --- the write/edit render intent reaches the browser](0218-web-diff-card-the-write-edit-render-intent-reaches-the-browser.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/client/ui-primitives/src/index.ts`.
- **`shares-code-with`** — [0220. Web read card frontend --- the read tool's line window renders line-numbered and highlighted](0220-web-read-card-frontend-the-read-tool-s-line-window-renders-line-numbered.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/client/ui-primitives/src/ansi.ts`.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-primitives/src/ansi.ts`, `packages/client/ui-primitives/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0225-web-search-card-the-grep-and-glob-render-intent-reaches-the-browser.md`.
