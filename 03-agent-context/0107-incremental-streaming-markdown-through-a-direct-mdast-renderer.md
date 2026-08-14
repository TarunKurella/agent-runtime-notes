---
id: "dsh-note-0107"
title: "Incremental streaming markdown through a direct mdast renderer"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-06-web-markdown-incremental-ast-renderer.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/trust"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/protocols"
  - "domain/security"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/streaming"
aliases:
  - "MarkdownText"
  - "IncrementalMarkdownParser"
  - "mathCompatibility"
  - "parseGfm"
  - "parseGfmWithMath"
  - "extractMarkdownPlainText"
  - "offset"
  - "mdast-util-from-markdown"
  - "remarkMathCompatibility"
  - "position.end.offset"
  - "text-align"
  - "DOMParser"
  - ".katex-display"
  - ".katex-mathml"
search_regex: "(?i)(MarkdownText|IncrementalMarkdownParser|mathCompatibility|parseGfm|parseGfmWithMath|extractMarkdownPlainText|offset|mdast\\-util\\-from\\-markdown)"
---

# 0107. Incremental streaming markdown through a direct mdast renderer — implementation context

## Open this when

MarkdownText re-parsed the whole accumulated reply on every streaming publish: react-markdown's string-only API builds a fresh unified processor per render and runs micromark → mdast → hast → React over the full text, so per-chunk main-thread work grew linearly with the reply and the stream's cumulative cost grew quadratically. The existing mitigations (frame batching, the isolated streaming tail, the plain fence arm) bounded how often and how widely that work ran, never how much text each run re-parsed.

## Source decision

MarkdownText renders mdast directly and parses incrementally while streaming: Grammars (parse.ts): parseGfm (streaming arm and extractMarkdownPlainText) and parseGfmWithMath (settled arm) call mdast-util-from-markdown with the same micromark extensions the replaced remark plugins wrapped, so block boundaries are identical everywhere. mathCompatibility (ex remarkMathCompatibility) now exports its micromark extension directly. Incremental parsing (incremental.ts): CommonMark block parsing is line-based, so appended text reshapes only the parse frontier.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-06-web-markdown-incremental-ast-renderer.md](../02-notes/implemented/architecture/2026-08-06-web-markdown-incremental-ast-renderer.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-06-web-markdown-incremental-ast-renderer.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-06-web-markdown-incremental-ast-renderer.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-primitives/src/markdown/parse.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/parse.ts) | runtime implementation | The source note names this file directly. Defines `parseGfm`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/katex.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/katex.tsx) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/client/ui-primitives/src/markdown/render.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/render.tsx) | runtime implementation | The source note names this file directly. Contains the exact code literal `tests/fixtures/markdown-dom` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/client/ui-primitives/src/markdown/incremental.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/incremental.ts) | runtime implementation | The source note names this file directly. Defines `IncrementalMarkdownParser`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/fs/tool-fs/src/read.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts) | runtime implementation | Defines `offset`, a construct named by the note. | `symbol-definition` |
| [`packages/e2b/fs-e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts) | package entry point | Defines `offset`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent/src/inbox.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/inbox.ts) | runtime implementation | Defines `offset`, a construct named by the note. | `symbol-definition` |
| [`packages/web/tool-web/src/fetch.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts) | runtime implementation | Defines `offset`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/plain-text.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/plain-text.ts) | runtime implementation | Defines `extractMarkdownPlainText`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/MarkdownText.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/MarkdownText.tsx) | runtime implementation | Defines `MarkdownText`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/mathCompatibility.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/mathCompatibility.ts) | runtime implementation | Defines `mathCompatibility`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `MarkdownText` | `const` | [`packages/client/ui-primitives/src/markdown/MarkdownText.tsx:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/MarkdownText.tsx#L156) | `export const MarkdownText = memo(function MarkdownText({ text, streaming = false, codeLabels, fileMentions }: {` |
| `IncrementalMarkdownParser` | `class` | [`packages/client/ui-primitives/src/markdown/incremental.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/incremental.ts#L75) | `export class IncrementalMarkdownParser {` |
| `mathCompatibility` | `function` | [`packages/client/ui-primitives/src/markdown/mathCompatibility.ts:347`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/mathCompatibility.ts#L347) | `export function mathCompatibility(): Extension {` |
| `parseGfm` | `function` | [`packages/client/ui-primitives/src/markdown/parse.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/parse.ts#L26) | `export function parseGfm(text: string): Root {` |
| `parseGfmWithMath` | `function` | [`packages/client/ui-primitives/src/markdown/parse.ts:39`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/parse.ts#L39) | `export function parseGfmWithMath(text: string): Root {` |
| `extractMarkdownPlainText` | `function` | [`packages/client/ui-primitives/src/markdown/plain-text.ts:106`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/plain-text.ts#L106) | `export function extractMarkdownPlainText(` |
| `offset` | `const` | [`packages/core/agent/src/inbox.ts:167`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/inbox.ts#L167) | `const offset = Number.isNaN(truncatedStart) ? 0 : truncatedStart` |
| `offset` | `let` | [`packages/e2b/fs-e2b/src/index.ts:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L156) | `let offset = 0` |
| `offset` | `let` | [`packages/e2b/fs-e2b/src/index.ts:287`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L287) | `let offset = 0` |
| `offset` | `const` | [`packages/fs/tool-fs/src/read.ts:58`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts#L58) | `const offset = args.offset === undefined ? 1 : parsePositiveInteger(args.offset, 'offset')` |
| `offset` | `let` | [`packages/web/tool-web/src/fetch.ts:141`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts#L141) | `let offset = 0` |

### Tests and executable evidence

- [`packages/web/tool-web/tests/tool-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/tests/tool-web.spec.ts) — A test under the owning area exercises or imports `text-align`.
- [`packages/client/ui-primitives/tests/markdown.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown.client.spec.tsx) — A test under the owning area exercises or imports `MarkdownText`. A test under the owning area exercises or imports `mathCompatibility`.
- [`packages/client/ui-primitives/tests/code-block.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/code-block.client.spec.tsx) — A test under the owning area exercises or imports `MarkdownText`.
- [`packages/client/ui-primitives/tests/markdown-plain-text.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown-plain-text.client.spec.ts) — A test under the owning area exercises or imports `extractMarkdownPlainText`.
- [`packages/client/ui-primitives/tests/markdown-dom-parity.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown-dom-parity.client.spec.tsx) — A test under the owning area exercises or imports `MarkdownText`.
- [`packages/client/ui-primitives/tests/markdown-incremental.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown-incremental.client.spec.tsx) — A test under the owning area exercises or imports `MarkdownText`. A test under the owning area exercises or imports `parseGfm`.
- [`packages/client/ui-primitives/tests/markdown-render-units.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown-render-units.client.spec.tsx) — A test under the owning area exercises or imports `MarkdownText`.
- [`packages/client/ui-primitives/tests/fixtures/markdown-dom/table-with-alignment.settled.txt`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/fixtures/markdown-dom/table-with-alignment.settled.txt) — A test under the owning area exercises or imports `text-align`.

## How to read the implementation

1. Start with [`packages/client/ui-primitives/src/markdown/parse.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/parse.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/trust`, `domain/extensions`, `domain/filesystem`, `domain/protocols`, `domain/security`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/streaming`
- Aliases: `MarkdownText`, `IncrementalMarkdownParser`, `mathCompatibility`, `parseGfm`, `parseGfmWithMath`, `extractMarkdownPlainText`, `offset`, `mdast-util-from-markdown`, `remarkMathCompatibility`, `position.end.offset`, `text-align`, `DOMParser`, `.katex-display`, `.katex-mathml`
- Regex: `(?i)(MarkdownText|IncrementalMarkdownParser|mathCompatibility|parseGfm|parseGfmWithMath|extractMarkdownPlainText|offset|mdast\-util\-from\-markdown)`

```bash
rg -n --pcre2 "(?i)(MarkdownText|IncrementalMarkdownParser|mathCompatibility|parseGfm|parseGfmWithMath|extractMarkdownPlainText|offset|mdast\\-util\\-from\\-markdown)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0177. Safe assistant Markdown in the Web conversation](0177-safe-assistant-markdown-in-the-web-conversation.md): The source note links to this decision directly.
- **`shares-code-with`** — [0092. Typert Gateway Targeted Method Calls](0092-typert-gateway-targeted-method-calls.md): Shares source implementation: `packages/core/agent/src/inbox.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/fs/tool-fs/src/read.ts`.
- **`shares-code-with`** — [0072. Portable consumers over filesystem and subprocess execution worlds](0072-portable-consumers-over-filesystem-and-subprocess-execution-worlds.md): Shares source implementation: `packages/e2b/fs-e2b/src/index.ts`.
- **`shares-code-with`** — [0607. experimental subcommands gate behind `--experimental` or `DSH_EXPERIMENTAL=1`](0607-experimental-subcommands-gate-behind-experimental-or-dsh-experimental-1.md): Shares source implementation: `packages/fs/tool-fs/src/read.ts`, `packages/web/tool-web/src/fetch.ts`.
- **`shares-code-with`** — [0032. Tool output spill policy](0032-tool-output-spill-policy.md): Shares source implementation: `packages/web/tool-web/src/fetch.ts`.
- **`shares-code-with`** — [0084. Follow-up enqueue and owned run boundaries](0084-follow-up-enqueue-and-owned-run-boundaries.md): Shares source implementation: `packages/core/agent/src/inbox.ts`, `packages/e2b/fs-e2b/src/index.ts`.
- **`shares-code-with`** — [0019. Make `dsh-fs-observation-policy` an event-gate plugin, not a method interface](0019-make-dsh-fs-observation-policy-an-event-gate-plugin-not-a-method-interfa.md): Shares source implementation: `packages/fs/tool-fs/src/read.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0107-incremental-streaming-markdown-through-a-direct-mdast-renderer.md`.
