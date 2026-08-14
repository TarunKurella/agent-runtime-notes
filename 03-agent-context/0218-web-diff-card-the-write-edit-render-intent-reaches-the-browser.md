---
id: "dsh-note-0218"
title: "Web diff card --- the write/edit render intent reaches the browser"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-30-web-diff-card.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/registry"
aliases:
  - "ConversationSnapshot"
  - "DEFAULT_DIFF_MAX_LINES"
  - "DiffBlock"
  - "card"
  - "TerminalBlock"
  - "CodeBlock"
  - "CHAT_DIFF_MAX_LINES"
  - "diffCardModel"
  - "GenericToolCard"
  - "FileMutationRow"
  - "FileDiff"
  - "diffs"
  - "terminal"
  - "meta"
search_regex: "(?i)(ConversationSnapshot|DEFAULT_DIFF_MAX_LINES|DiffBlock|card|TerminalBlock|CodeBlock|CHAT_DIFF_MAX_LINES|diffCardModel)"
---

# 0218. Web diff card --- the write/edit render intent reaches the browser — implementation context

## Open this when

The write and edit tools declare card: 'diff' for both their call and their result (render-intent union): the call view carries the intended change derived from the arguments, and the result view carries the applied contextual hunks (FileDiff[], computed by packages/fs/tool-fs/src/diff.ts and persisted in the result meta so replay reproduces it). That view already reaches the browser --- host, connection, and runtime deliver it onto ConversationSnapshot as callView/resultView --- and the TUI already renders it as per-file +/- blocks with a +A -R · N file(s) footer. The Web client ignored it.

## Source decision

DiffBlock is a ui-primitives component that renders a file mutation as an inline diff surface, and both Web render sites for a write/edit call consume the diff render intent through it: the chat tool row's body and the details panel's Output section. ui-tool/src/client/tool/models/diff-card-model.ts is the single place that turns the snapshot's callView/resultView pair into the component's props, so the two sites cannot disagree about a change.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-30-web-diff-card.md](../02-notes/implemented/feature/2026-07-30-web-diff-card.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-30-web-diff-card.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-30-web-diff-card.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/fs/tool-fs/src/diff.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/diff.ts) | runtime implementation | The source note names this file directly. Defines `diffs`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/terminal/terminal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/index.ts) | package entry point | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/terminal/terminal/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/client/ui-primitives/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/terminal/terminal/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/client/ui-primitives/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/client/ui-primitives/src/HoverCard.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. Defines `card`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-primitives/src/DiffBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DiffBlock.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. Defines `DiffBlock`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-primitives/src/TerminalBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/TerminalBlock.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. Defines `TerminalBlock`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. Defines `CodeBlock`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/terminal/terminal`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ConversationSnapshot` | `interface` | [`packages/client/runtime/src/client/sessions/conversation.ts:433`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts#L433) | `export interface ConversationSnapshot {` |
| `DEFAULT_DIFF_MAX_LINES` | `const` | [`packages/client/ui-primitives/src/DiffBlock.tsx:21`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DiffBlock.tsx#L21) | `export const DEFAULT_DIFF_MAX_LINES = 16` |
| `DiffBlock` | `function` | [`packages/client/ui-primitives/src/DiffBlock.tsx:141`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DiffBlock.tsx#L141) | `export function DiffBlock({ diffs, maxLines = DEFAULT_DIFF_MAX_LINES, className }: DiffBlockProps) {` |
| `card` | `const` | [`packages/client/ui-primitives/src/HoverCard.tsx:140`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx#L140) | `const card = cardRef.current` |
| `card` | `const` | [`packages/client/ui-primitives/src/HoverCard.tsx:149`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx#L149) | `const card = open && pos !== null && (` |
| `TerminalBlock` | `function` | [`packages/client/ui-primitives/src/TerminalBlock.tsx:174`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/TerminalBlock.tsx#L174) | `export function TerminalBlock({` |
| `CodeBlock` | `function` | [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx#L26) | `export function CodeBlock({ code, lang, className, copyLabel = '复制', copiedLabel = '复制成功' }: CodeBlockProps) {` |
| `CHAT_DIFF_MAX_LINES` | `const` | [`packages/client/ui-tool/src/client/tool/models/diff-card-model.ts:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/models/diff-card-model.ts#L22) | `export const CHAT_DIFF_MAX_LINES = 8` |
| `diffCardModel` | `function` | [`packages/client/ui-tool/src/client/tool/models/diff-card-model.ts:84`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/models/diff-card-model.ts#L84) | `export function diffCardModel(block: ToolCallBlock): DiffCardModel \| null {` |
| `GenericToolCard` | `function` | [`packages/client/ui-tool/src/client/tool/toolviews/GenericToolCard.tsx:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/toolviews/GenericToolCard.tsx#L36) | `export function GenericToolCard({ toolName, block, cwd, openFile, inspect, t }: GenericToolCardProps) {` |
| `FileMutationRow` | `function` | [`packages/client/ui-tool/src/client/tool/toolviews/file-mutation-row.tsx:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/toolviews/file-mutation-row.tsx#L32) | `export function FileMutationRow({ toolName, block, cwd, openFile, inspect, t }: FileMutationRowProps) {` |
| `FileDiff` | `interface` | [`packages/core/tools/src/presentation.ts:34`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts#L34) | `export interface FileDiff {` |
| `diffs` | `const` | [`packages/fs/tool-fs/src/diff.ts:35`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/diff.ts#L35) | `const diffs: FileDiff[] = []` |
| `terminal` | `const` | [`packages/jobs/jobs/src/invariant.ts:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/invariant.ts#L30) | `const terminal = TERMINAL_STATUSES.has(snapshot.status)` |
| `meta` | `const` | [`vendor/cordis/src/fiber.ts:444`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts#L444) | `const meta: EffectMeta = { label, children: [] }` |
| `diff` | `const` | [`vendor/loader/src/config/entry.ts:157`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/entry.ts#L157) | `const diff = Object` |

### Tests and executable evidence

- [`apps/web/tests/built-boot.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/built-boot.snapshot.ts) — The source note names this file directly.
- [`packages/client/ui-tool/tests/diff-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/diff-card.client.spec.tsx) — The source note names this file directly. A test under the owning area exercises or imports `GenericToolCard`.
- [`packages/client/ui-primitives/tests/diff-block.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/diff-block.client.spec.tsx) — The source note names this file directly. A test under the owning area exercises or imports `diffs`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/fs/tool-fs/tests/diff.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/diff.spec.ts) — A test under the owning area exercises or imports `diffs`.
- [`packages/fs/tool-fs/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/tools.spec.ts) — A test under the owning area exercises or imports `diffs`.
- Source verification intent: packages/client/ui-primitives/tests/diff-block.client.spec.tsx pins the component: the create arm (added-only, no removed side), the edit arm (removed above added), the same-file ⋯ gap versus a new file's own header, the empty-diffs null render, the footer counts and their singular/plural, the head/tail cap with its aria-expanded toggle, and the copy control asserting the prefixed diff text on both the accepted and refused clipboard paths. Per-file 100%.

## How to read the implementation

1. Start with [`packages/fs/tool-fs/src/diff.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/diff.ts) because it has the strongest evidence link to the note.
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
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/registry`
- Aliases: `ConversationSnapshot`, `DEFAULT_DIFF_MAX_LINES`, `DiffBlock`, `card`, `TerminalBlock`, `CodeBlock`, `CHAT_DIFF_MAX_LINES`, `diffCardModel`, `GenericToolCard`, `FileMutationRow`, `FileDiff`, `diffs`, `terminal`, `meta`
- Regex: `(?i)(ConversationSnapshot|DEFAULT_DIFF_MAX_LINES|DiffBlock|card|TerminalBlock|CodeBlock|CHAT_DIFF_MAX_LINES|diffCardModel)`

```bash
rg -n --pcre2 "(?i)(ConversationSnapshot|DEFAULT_DIFF_MAX_LINES|DiffBlock|card|TerminalBlock|CodeBlock|CHAT_DIFF_MAX_LINES|diffCardModel)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0023. Tagged render-intent union for tool-call presentation](0023-tagged-render-intent-union-for-tool-call-presentation.md): The source note links to this decision directly.
- **`source-link`** — [0045. Web client architecture --- the client cordis plugin tree, the slot system, and the React-free object layer](0045-web-client-architecture-the-client-cordis-plugin-tree-the-slot-system-an.md): The source note links to this decision directly.
- **`source-link`** — [0207. Web terminal card --- the bash render intent reaches the browser](0207-web-terminal-card-the-bash-render-intent-reaches-the-browser.md): The source note links to this decision directly.
- **`shares-code-with`** — [0225. Web search card --- the grep and glob render intent reaches the browser](0225-web-search-card-the-grep-and-glob-render-intent-reaches-the-browser.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/client/ui-primitives/src/index.ts`.
- **`shares-code-with`** — [0223. Web result card frontend --- rendering the web render intent in the browser](0223-web-result-card-frontend-rendering-the-web-render-intent-in-the-browser.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/client/ui-primitives/src/index.ts`.
- **`shares-code-with`** — [0224. Web result card --- a structured render intent for web_search and web_fetch](0224-web-result-card-a-structured-render-intent-for-web-search-and-web-fetch.md): Shares source implementation: `packages/fs/tool-fs/src/diff.ts`, `packages/terminal/terminal/src/index.ts`.
- **`shares-code-with`** — [0220. Web read card frontend --- the read tool's line window renders line-numbered and highlighted](0220-web-read-card-frontend-the-read-tool-s-line-window-renders-line-numbered.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/client/ui-primitives/src/index.ts`.
- **`shares-code-with`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares source implementation: `packages/terminal/terminal/src/index.ts`, `packages/terminal/terminal/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0218-web-diff-card-the-write-edit-render-intent-reaches-the-browser.md`.
