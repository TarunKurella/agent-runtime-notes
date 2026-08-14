---
id: "dsh-note-0412"
title: "Web client syntax highlighting --- synchronous fine-grained shiki"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-26-web-syntax-highlighting-shiki.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/streaming"
aliases:
  - "json"
  - "CodeBlock"
  - "MarkdownText"
  - "highlightToHtml"
  - "constructor"
  - "run_code"
  - "ui-primitives"
  - "shiki/core"
  - "@shikijs/langs"
  - "createHighlighterCoreSync"
  - "createJavaScriptRegexEngine"
  - "ui-primitives/src/markdown/highlight.ts"
  - "HighlighterCore"
  - "dangerouslySetInnerHTML"
search_regex: "(?i)(json|CodeBlock|MarkdownText|highlightToHtml|constructor|run_code|ui\\-primitives|shiki/core)"
---

# 0412. Web client syntax highlighting --- synchronous fine-grained shiki — implementation context

## Open this when

The client rendered every code surface --- markdown fences in assistant prose, the run_code program body, the details panel's args --- as flat monospace text. The stack's primary payload is model-written TypeScript; unhighlighted programs are measurably harder to scan, and the repo already ships shiki-highlighted code on its VitePress site, so the web app was the one code-rendering surface without it.

## Source decision

Shiki in its synchronous fine-grained form, as one ui-primitives singleton, themed exclusively through CSS custom properties. Dependency: shiki/core + @shikijs/langs, composed via createHighlighterCoreSync with createJavaScriptRegexEngine({ forgiving: true }) --- no oniguruma WASM, no async init, bundle-friendly. Grammar allowlist: typescript (embeds JS), shellscript, json --- the languages the harness actually renders; everything else falls back to a geometry-identical plain block, never an error.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-26-web-syntax-highlighting-shiki.md](../02-notes/implemented/process/2026-07-26-web-syntax-highlighting-shiki.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-26-web-syntax-highlighting-shiki.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-26-web-syntax-highlighting-shiki.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-primitives/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/client/ui-primitives/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/client/ui-primitives/src/markdown/highlight.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/highlight.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. Defines `highlightToHtml`, a construct named by the note. | `exact-code-occurrence, named-package-member, symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. Defines `CodeBlock`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/MarkdownText.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/MarkdownText.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. Defines `MarkdownText`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-primitives`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/cordis/src/service.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/service.ts) | runtime implementation | Defines `constructor`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/README.md) | package contract and examples | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/client/ui-primitives/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/package.json) | composition and configuration | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `CodeBlock` | `function` | [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx#L26) | `export function CodeBlock({ code, lang, className, copyLabel = '复制', copiedLabel = '复制成功' }: CodeBlockProps) {` |
| `MarkdownText` | `const` | [`packages/client/ui-primitives/src/markdown/MarkdownText.tsx:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/MarkdownText.tsx#L156) | `export const MarkdownText = memo(function MarkdownText({ text, streaming = false, codeLabels, fileMentions }: {` |
| `highlightToHtml` | `function` | [`packages/client/ui-primitives/src/markdown/highlight.ts:263`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/highlight.ts#L263) | `export function highlightToHtml(code: string, lang: string \| undefined): string \| undefined {` |
| `constructor` | `let` | [`vendor/cordis/src/service.ts:106`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/service.ts#L106) | `let constructor = instance.constructor` |

### Tests and executable evidence

- [`packages/client/ui-primitives/tests/markdown.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown.client.spec.tsx) — A test under the owning area exercises or imports `CodeBlock`. A test under the owning area exercises or imports `css`.
- [`packages/client/ui-primitives/tests/code-block.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/code-block.client.spec.tsx) — A test under the owning area exercises or imports `highlightToHtml`. A test under the owning area exercises or imports `CodeBlock`.
- [`packages/client/ui-primitives/tests/read-block.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/read-block.client.spec.tsx) — A test under the owning area exercises or imports `shiki`.
- [`packages/client/ui-primitives/tests/markdown-dom-parity.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown-dom-parity.client.spec.tsx) — A test under the owning area exercises or imports `MarkdownText`. A test under the owning area exercises or imports `pre`.
- [`packages/client/ui-primitives/tests/markdown-incremental.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown-incremental.client.spec.tsx) — A test under the owning area exercises or imports `MarkdownText`.
- [`packages/client/ui-primitives/tests/markdown-render-units.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown-render-units.client.spec.tsx) — A test under the owning area exercises or imports `MarkdownText`.
- [`packages/client/ui-primitives/tests/fixtures/markdown-dom/code-fences.settled.txt`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/fixtures/markdown-dom/code-fences.settled.txt) — A test under the owning area exercises or imports `pre`. A test under the owning area exercises or imports `shiki`.
- [`packages/client/ui-primitives/tests/fixtures/markdown-dom/code-fences.streaming.txt`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/fixtures/markdown-dom/code-fences.streaming.txt) — A test under the owning area exercises or imports `pre`.

## How to read the implementation

1. Start with [`packages/client/ui-primitives/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
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

- Tags: `class/process`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/shell-terminal`, `domain/storage`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/streaming`
- Aliases: `json`, `CodeBlock`, `MarkdownText`, `highlightToHtml`, `constructor`, `run_code`, `ui-primitives`, `shiki/core`, `@shikijs/langs`, `createHighlighterCoreSync`, `createJavaScriptRegexEngine`, `ui-primitives/src/markdown/highlight.ts`, `HighlighterCore`, `dangerouslySetInnerHTML`
- Regex: `(?i)(json|CodeBlock|MarkdownText|highlightToHtml|constructor|run_code|ui\-primitives|shiki/core)`

```bash
rg -n --pcre2 "(?i)(json|CodeBlock|MarkdownText|highlightToHtml|constructor|run_code|ui\\-primitives|shiki/core)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0188. Code Mode chat rendering --- sub-calls as native rows under the parent](0188-code-mode-chat-rendering-sub-calls-as-native-rows-under-the-parent.md): The source note links to this decision directly.
- **`source-link`** — [0207. Web terminal card --- the bash render intent reaches the browser](0207-web-terminal-card-the-bash-render-intent-reaches-the-browser.md): The source note links to this decision directly.
- **`source-link`** — [0397. Web styling system --- the token framework and engineering constraints](0397-web-styling-system-the-token-framework-and-engineering-constraints.md): The source note links to this decision directly.
- **`shares-code-with`** — [0465. Local JSON tree renderer](0465-local-json-tree-renderer.md): Shares source implementation: `packages/client/ui-primitives`, `packages/client/ui-primitives/README.md`.
- **`shares-code-with`** — [0220. Web read card frontend --- the read tool's line window renders line-numbered and highlighted](0220-web-read-card-frontend-the-read-tool-s-line-window-renders-line-numbered.md): Shares source implementation: `packages/client/ui-primitives/src/index.ts`, `packages/client/ui-primitives/src/invariant.ts`.
- **`shares-code-with`** — [0177. Safe assistant Markdown in the Web conversation](0177-safe-assistant-markdown-in-the-web-conversation.md): Shares source implementation: `packages/client/ui-primitives/src/index.ts`, `packages/client/ui-primitives/src/invariant.ts`.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-primitives/src/index.ts`, `packages/client/ui-primitives/src/invariant.ts`.
- **`shares-code-with`** — [0108. Web shell dist chunk split and directory layout](0108-web-shell-dist-chunk-split-and-directory-layout.md): Shares source implementation: `packages/client/ui-primitives/src/markdown/CodeBlock.tsx`, `packages/client/ui-primitives/src/markdown/highlight.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0412-web-client-syntax-highlighting-synchronous-fine-grained-shiki.md`.
