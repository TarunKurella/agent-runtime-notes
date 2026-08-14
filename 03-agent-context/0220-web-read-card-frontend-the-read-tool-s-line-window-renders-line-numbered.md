---
id: "dsh-note-0220"
title: "Web read card frontend --- the read tool's line window renders line-numbered and highlighted"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-30-web-read-card-frontend.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/agent-loop"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
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
  - "mechanism/streaming"
aliases:
  - "DetailsPanel"
  - "card"
  - "ReadBlock"
  - "TerminalBlock"
  - "lines"
  - "inject"
  - "CodeBlock"
  - "LAZY_GRAMMARS"
  - "ensureGrammar"
  - "highlightToHtml"
  - "highlightLines"
  - "lang"
  - "CHAT_READ_MAX_LINES"
  - "readCardModel"
search_regex: "(?i)(DetailsPanel|card|ReadBlock|TerminalBlock|lines|inject|CodeBlock|LAZY_GRAMMARS)"
---

# 0220. Web read card frontend --- the read tool's line window renders line-numbered and highlighted — implementation context

## Open this when

The read backend added a fourth render-intent card, card: 'read', to ToolResultView: a settled read now carries { path, lines: [{ number, text }], totalLines, lang? } onto the conversation snapshot as resultView. That data reaches the browser, but the Web client had no consumer for it. Every read row derived from args alone and the details panel flattened the result's content blocks into one , so a read showed as N: text-prefixed plain text with no gutter, no syntax highlighting, and no "showing N of M" affordance for a windowed read.

## Source decision

ReadBlock is a ui-primitives component that renders a read result as a line-numbered, optionally syntax-highlighted file view, and both Web render sites for a read consume the read render intent through it: the chat tool row (resident under the summary line) and the details panel's Output section. ui-tool/src/client/tool/models/read-card-model.ts is the single place that turns the snapshot's resultView into the component's props, so the two sites cannot disagree. A new ReadBlock primitive, not an extension of CodeBlock.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-30-web-read-card-frontend.md](../02-notes/implemented/feature/2026-07-30-web-read-card-frontend.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-30-web-read-card-frontend.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-30-web-read-card-frontend.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/client/ui-primitives/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/client/ui-primitives/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-primitives`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-primitives/src/HoverCard.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. Defines `card`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-primitives/src/TerminalBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/TerminalBlock.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. Defines `TerminalBlock`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/highlight.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/highlight.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. Defines `highlightLines`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/mathCompatibility.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/mathCompatibility.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/client/ui-primitives/src/ansi.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ansi.ts) | runtime implementation | Defines `lines`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/ReadBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ReadBlock.tsx) | runtime implementation | Defines `ReadBlock`, a construct named by the note. Contains the exact code literal `markdown/highlight.ts` named by the note. | `exact-code-occurrence, symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/render.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/render.tsx) | runtime implementation | Defines `lang`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx) | runtime implementation | Defines `CodeBlock`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `DetailsPanel` | `function` | [`packages/client/ui-conversation/src/client/skeleton/DetailsPanel.tsx:66`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/DetailsPanel.tsx#L66) | `export function DetailsPanel({ useSession, useSessions, sessionId, useStore, renderSlot, closeDetails, t }: DetailsPanelProps) {` |
| `card` | `const` | [`packages/client/ui-primitives/src/HoverCard.tsx:140`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx#L140) | `const card = cardRef.current` |
| `card` | `const` | [`packages/client/ui-primitives/src/HoverCard.tsx:149`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx#L149) | `const card = open && pos !== null && (` |
| `ReadBlock` | `function` | [`packages/client/ui-primitives/src/ReadBlock.tsx:70`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ReadBlock.tsx#L70) | `export function ReadBlock({` |
| `TerminalBlock` | `function` | [`packages/client/ui-primitives/src/TerminalBlock.tsx:174`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/TerminalBlock.tsx#L174) | `export function TerminalBlock({` |
| `lines` | `const` | [`packages/client/ui-primitives/src/TerminalBlock.tsx:198`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/TerminalBlock.tsx#L198) | `const lines = useMemo(() => {` |
| `lines` | `const` | [`packages/client/ui-primitives/src/ansi.ts:435`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ansi.ts#L435) | `const lines: AnsiSpan[][] = [current]` |
| `inject` | `const` | [`packages/client/ui-primitives/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `CodeBlock` | `function` | [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx#L26) | `export function CodeBlock({ code, lang, className, copyLabel = '复制', copiedLabel = '复制成功' }: CodeBlockProps) {` |
| `LAZY_GRAMMARS` | `const` | [`packages/client/ui-primitives/src/markdown/highlight.ts:53`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/highlight.ts#L53) | `const LAZY_GRAMMARS = new Map<string, () => Promise<LangModule>>([` |
| `ensureGrammar` | `function` | [`packages/client/ui-primitives/src/markdown/highlight.ts:228`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/highlight.ts#L228) | `function ensureGrammar(resolved: string): boolean {` |
| `highlightToHtml` | `function` | [`packages/client/ui-primitives/src/markdown/highlight.ts:263`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/highlight.ts#L263) | `export function highlightToHtml(code: string, lang: string \| undefined): string \| undefined {` |
| `highlightLines` | `function` | [`packages/client/ui-primitives/src/markdown/highlight.ts:296`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/highlight.ts#L296) | `export function highlightLines(code: string, lang: string \| undefined): HighlightSpan[][] \| undefined {` |
| `lines` | `const` | [`packages/client/ui-primitives/src/markdown/highlight.ts:307`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/highlight.ts#L307) | `const lines = tokens.length > 1 && last !== undefined && last.length === 0` |
| `lang` | `const` | [`packages/client/ui-primitives/src/markdown/render.tsx:312`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/render.tsx#L312) | `const lang = language === undefined ? undefined : /^[\w-]+/.exec(language)?.[0]` |
| `CHAT_READ_MAX_LINES` | `const` | [`packages/client/ui-tool/src/client/tool/models/read-card-model.ts:29`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/models/read-card-model.ts#L29) | `export const CHAT_READ_MAX_LINES = 8` |

### Tests and executable evidence

- [`packages/client/ui-tool/tests/read-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/read-card.client.spec.tsx) — The source note names this file directly. A test under the owning area exercises or imports `readCardModel`.
- [`packages/client/ui-primitives/tests/read-block.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/read-block.client.spec.tsx) — The source note names this file directly. A test under the owning area exercises or imports `ReadBlock`.
- [`packages/fs/tool-fs/tests/read-render.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/read-render.spec.ts) — A test under the owning area exercises or imports `langFromPath`.
- [`packages/workflow/tool-ralph/tests/tool-ralph.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/tests/tool-ralph.spec.ts) — A test under the owning area exercises or imports `presentResult`.
- [`packages/client/ui-tool/tests/web-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/web-card.client.spec.tsx) — A test under the owning area exercises or imports `BashRow`. A test under the owning area exercises or imports `GenericToolCard`.
- [`packages/client/ui-tool/tests/tool-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/tool-row.client.spec.tsx) — A test under the owning area exercises or imports `GenericToolCard`.
- [`packages/client/ui-primitives/tests/ansi.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/ansi.client.spec.ts) — A test under the owning area exercises or imports `TerminalBlock`.
- [`packages/client/ui-tool/tests/diff-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/diff-card.client.spec.tsx) — A test under the owning area exercises or imports `GenericToolCard`.
- Source verification intent: packages/client/ui-primitives/tests/read-block.client.spec.tsx pins the primitive and the token path: highlightLines' per-line css-variables runs, its trailing-terminator-line drop and the genuinely-blank-final-line case, its undefined for an unknown/absent language, and its lazy path (a lazy grammar returns plain on first touch, then highlights after the import registers and the subscriber fires); and ReadBlock's gutter-numbered rows keeping the file's own numbers, the highlighted-vs-plain content arms, the banner (label, language, the count note only when the read is a window), the head/tail height cap.

## How to read the implementation

1. Start with [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/simplification`, `domain/agent-loop`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `DetailsPanel`, `card`, `ReadBlock`, `TerminalBlock`, `lines`, `inject`, `CodeBlock`, `LAZY_GRAMMARS`, `ensureGrammar`, `highlightToHtml`, `highlightLines`, `lang`, `CHAT_READ_MAX_LINES`, `readCardModel`
- Regex: `(?i)(DetailsPanel|card|ReadBlock|TerminalBlock|lines|inject|CodeBlock|LAZY_GRAMMARS)`

```bash
rg -n --pcre2 "(?i)(DetailsPanel|card|ReadBlock|TerminalBlock|lines|inject|CodeBlock|LAZY_GRAMMARS)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0023. Tagged render-intent union for tool-call presentation](0023-tagged-render-intent-union-for-tool-call-presentation.md): The source note links to this decision directly.
- **`source-link`** — [0207. Web terminal card --- the bash render intent reaches the browser](0207-web-terminal-card-the-bash-render-intent-reaches-the-browser.md): The source note links to this decision directly.
- **`source-link`** — [0221. Read card --- the read tool's structured line window reaches the client](0221-read-card-the-read-tool-s-structured-line-window-reaches-the-client.md): The source note links to this decision directly.
- **`source-link`** — [0226. Web tool-row unified expand and trajectory Inspect](0226-web-tool-row-unified-expand-and-trajectory-inspect.md): The source note links to this decision directly.
- **`shares-code-with`** — [0223. Web result card frontend --- rendering the web render intent in the browser](0223-web-result-card-frontend-rendering-the-web-render-intent-in-the-browser.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/client/ui-primitives/src/ansi.ts`.
- **`shares-code-with`** — [0225. Web search card --- the grep and glob render intent reaches the browser](0225-web-search-card-the-grep-and-glob-render-intent-reaches-the-browser.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/client/ui-primitives/src/ansi.ts`.
- **`shares-code-with`** — [0177. Safe assistant Markdown in the Web conversation](0177-safe-assistant-markdown-in-the-web-conversation.md): Shares source implementation: `packages/client/ui-primitives/src/TerminalBlock.tsx`, `packages/client/ui-primitives/src/index.ts`.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-primitives/src/ansi.ts`, `packages/client/ui-primitives/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0220-web-read-card-frontend-the-read-tool-s-line-window-renders-line-numbered.md`.
