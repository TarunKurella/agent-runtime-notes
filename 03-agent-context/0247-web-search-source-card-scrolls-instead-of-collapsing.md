---
id: "dsh-note-0247"
title: "Web search source card scrolls instead of collapsing"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-03-web-search-source-scroll.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "TerminalBlock"
  - "WebSearchBlockProps"
  - "WebFetchBlockProps"
  - "WebBlock"
  - "truncated"
  - "capSources"
  - "web_search"
  - "packages/client/ui-primitives/src/WebBlock.tsx"
  - "maxSources"
  - "CHAT_WEB_MAX_SOURCES"
  - "… 其余 N 条来源"
  - "max - ceil"
  - "packages/web/web/src/index.ts"
  - "searchMaxResults"
search_regex: "(?i)(TerminalBlock|WebSearchBlockProps|WebFetchBlockProps|WebBlock|truncated|capSources|web_search|packages/client/ui\\-primitives/src/WebBlock\\.tsx)"
---

# 0247. Web search source card scrolls instead of collapsing — implementation context

## Open this when

The web_search result card (WebBlock, packages/client/ui-primitives/src/WebBlock.tsx) rendered its source list with a head/tail collapse: past a maxSources count (16 in the details panel, 8 in the chat row via CHAT_WEB_MAX_SOURCES) it drew the first ceil(max/2) sources, an … 其余 N 条来源 expand button, then the last max - ceil(max/2), mirroring TerminalBlock's output cap. A user reading the card saw 来源列表已截断 and assumed the frontend had dropped sources it was holding. It had not.

## Source decision

WebBlock's search arm renders every source it receives in one , with no head/tail slicing, no expand button, and no maxSources prop. .sources (WebBlock.module.css) gets a fixed max-height and overflow-y: auto, so a list longer than the card height scrolls in place rather than growing the card or hiding rows. The height is a design constant of the card geometry, so it lives in CSS, not a plugin config field. The model side is unchanged: the seam still caps sources at searchMaxResults, the model-facing render text is untouched, and the truncated flag and its 来源列表已截断 indicator stay.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-03-web-search-source-scroll.md](../02-notes/implemented/feature/2026-08-03-web-search-source-scroll.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-03-web-search-source-scroll.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-03-web-search-source-scroll.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/web/web/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web/src/index.ts) | package entry point | The source note names this file directly. Defines `capSources`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/client/ui-primitives/src/WebBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/WebBlock.tsx) | runtime implementation | The source note names this file directly. Defines `WebBlock`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/spill/spill-policy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/spill/spill-policy`. | `named-package-member` |
| [`packages/spill/spill-policy/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/spill/spill-policy`. | `named-package-member` |
| [`packages/spill/spill-policy/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/spill/spill-policy`. | `named-package-member` |
| [`packages/web/tool-web/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/web/tool-web`. | `named-directory-member` |
| [`packages/web/tool-web/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/web/tool-web`. | `named-directory-member` |
| [`packages/web/tool-web/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/web/tool-web`. | `named-directory-member` |
| [`packages/web/tool-web/src/fetch.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/web/tool-web`. Defines `truncated`, a construct named by the note. | `named-directory-member, symbol-definition` |
| [`packages/web/tool-web/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/web/tool-web`. | `named-directory-member` |
| [`packages/web/tool-web`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/spill/spill-policy`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `TerminalBlock` | `function` | [`packages/client/ui-primitives/src/TerminalBlock.tsx:174`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/TerminalBlock.tsx#L174) | `export function TerminalBlock({` |
| `WebSearchBlockProps` | `interface` | [`packages/client/ui-primitives/src/WebBlock.tsx:43`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/WebBlock.tsx#L43) | `export interface WebSearchBlockProps {` |
| `WebFetchBlockProps` | `interface` | [`packages/client/ui-primitives/src/WebBlock.tsx:56`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/WebBlock.tsx#L56) | `export interface WebFetchBlockProps {` |
| `WebBlock` | `function` | [`packages/client/ui-primitives/src/WebBlock.tsx:200`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/WebBlock.tsx#L200) | `export function WebBlock(props: WebBlockProps) {` |
| `truncated` | `const` | [`packages/web/tool-web/src/fetch.ts:314`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts#L314) | `const truncated = result.truncated \|\| rendered.sourceTruncated \|\| prefix.length > maxOutputChars` |
| `capSources` | `function` | [`packages/web/web/src/index.ts:197`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web/src/index.ts#L197) | `function capSources(result: WebSearchResult, maxResults: number \| undefined): WebSearchResult {` |

### Tests and executable evidence

- [`apps/web/tests/web-search-round.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/web-search-round.e2e.ts) — The source note names this file directly.
- [`packages/client/ui-tool/tests/web-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/web-card.client.spec.tsx) — The source note names this file directly.
- [`packages/client/ui-primitives/tests/web-block.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/web-block.client.spec.tsx) — The source note names this file directly. A test under the owning area exercises or imports `WebBlock`.
- [`packages/spill/spill-policy/tests/spill-policy.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/tests/spill-policy.spec.ts) — A test under the owning area exercises or imports `dsh-spill-policy`.
- [`packages/client/ui-primitives/tests/ansi.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/ansi.client.spec.ts) — A test under the owning area exercises or imports `TerminalBlock`.
- [`packages/client/ui-primitives/tests/terminal-block.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/terminal-block.client.spec.tsx) — A test under the owning area exercises or imports `TerminalBlock`.
- [`scripts/session-fixture-layout.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/session-fixture-layout.spec.ts) — Contains the exact code literal `assistant/chunk` named by the note.
- Source verification intent: packages/client/ui-primitives/tests/web-block.client.spec.tsx drops the collapse cases (head/tail slice, expand-on-click, collapsed-tail numbering, expander-out-of-numbering, head-alone, default cap) and adds: a 30-source card renders all 30 with no [aria-expanded] and no , every child is a source , and numbers 1..N contiguously. packages/client/ui-tool/tests/web-card.client.spec.tsx drops the CHAT_WEB_MAX_SOURCES cap assertion; the WebRow expansion test still asserts the card shows every source field. The packages/web/tool-web tests are unchanged --- the model side did not move.

## How to read the implementation

1. Start with [`packages/web/web/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `TerminalBlock`, `WebSearchBlockProps`, `WebFetchBlockProps`, `WebBlock`, `truncated`, `capSources`, `web_search`, `packages/client/ui-primitives/src/WebBlock.tsx`, `maxSources`, `CHAT_WEB_MAX_SOURCES`, `… 其余 N 条来源`, `max - ceil`, `packages/web/web/src/index.ts`, `searchMaxResults`
- Regex: `(?i)(TerminalBlock|WebSearchBlockProps|WebFetchBlockProps|WebBlock|truncated|capSources|web_search|packages/client/ui\-primitives/src/WebBlock\.tsx)`

```bash
rg -n --pcre2 "(?i)(TerminalBlock|WebSearchBlockProps|WebFetchBlockProps|WebBlock|truncated|capSources|web_search|packages/client/ui\\-primitives/src/WebBlock\\.tsx)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0223. Web result card frontend --- rendering the web render intent in the browser](0223-web-result-card-frontend-rendering-the-web-render-intent-in-the-browser.md): The source note links to this decision directly.
- **`source-link`** — [0224. Web result card --- a structured render intent for web_search and web_fetch](0224-web-result-card-a-structured-render-intent-for-web-search-and-web-fetch.md): The source note links to this decision directly.
- **`shares-code-with`** — [0216. Search render intent --- grep and glob emit a structured search card](0216-search-render-intent-grep-and-glob-emit-a-structured-search-card.md): Shares source implementation: `packages/spill/spill-policy/src/index.ts`, `packages/spill/spill-policy/src/invariant.ts`.
- **`shares-code-with`** — [0186. Spilling the durable copy of Code Mode sub-dispatch results](0186-spilling-the-durable-copy-of-code-mode-sub-dispatch-results.md): Shares source implementation: `packages/spill/spill-policy/src/index.ts`, `packages/spill/spill-policy/src/invariant.ts`.
- **`shares-code-with`** — [0655. Drop the unconsumed web observation surface --- the `providers-change` event and the status methods](0655-drop-the-unconsumed-web-observation-surface-the-providers-change-event-a.md): Shares source implementation: `packages/web/tool-web/README.md`, `packages/web/tool-web/src/index.ts`.
- **`shares-code-with`** — [0235. Default Web search in shipped compositions](0235-default-web-search-in-shipped-compositions.md): Shares source implementation: `packages/web/tool-web/src/index.ts`, `packages/web/web/src/index.ts`.
- **`shares-code-with`** — [0672. Replace tool-web's regex HTML-to-markdown converter with turndown](0672-replace-tool-web-s-regex-html-to-markdown-converter-with-turndown.md): Shares source implementation: `packages/web/tool-web/README.md`, `packages/web/tool-web/src/index.ts`.
- **`shares-code-with`** — [0032. Tool output spill policy](0032-tool-output-spill-policy.md): Shares source implementation: `packages/web/tool-web/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0247-web-search-source-card-scrolls-instead-of-collapsing.md`.
