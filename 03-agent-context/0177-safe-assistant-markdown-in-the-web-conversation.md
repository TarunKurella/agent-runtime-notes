---
id: "dsh-note-0177"
title: "Safe assistant Markdown in the Web conversation"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-23-web-assistant-markdown.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/trust"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "AssistantMarkdown"
  - "CodeBlock"
  - "MarkdownText"
  - "MessageText"
  - "mathCompatibility"
  - "@deepseek-ai/dsh-client-ui-primitives"
  - "ui-conversation"
  - "mdast-util-from-markdown"
  - "--shiki-*"
  - "@deepseek/md"
  - "markdown.css"
  - "code-block.css"
  - "--dsw-alias-markdown-*"
  - "--dsw-font-markdown-*"
search_regex: "(?i)(AssistantMarkdown|CodeBlock|MarkdownText|MessageText|mathCompatibility|@deepseek\\-ai/dsh\\-client\\-ui\\-primitives|ui\\-conversation|mdast\\-util\\-from\\-markdown)"
---

# 0177. Safe assistant Markdown in the Web conversation — implementation context

## Open this when

The Web conversation preserves assistant Markdown source through session events, history replay, and streaming accumulation, but its terminal text primitive renders that source literally. Changing the shared primitive would also format user and steering messages, while parsing in the runtime would mix presentation state into the React-free session projection.

## Source decision

@deepseek-ai/dsh-client-ui-primitives exports MarkdownText as the untrusted assistant-text renderer, and ui-conversation selects it only for assistant text blocks. Finalized history, the streaming tail, and interrupted partials already share AssistantMarkdown, so they receive the same renderer without changing events or snapshots. User and steering messages keep MessageText and remain literal.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-23-web-assistant-markdown.md](../02-notes/implemented/feature/2026-07-23-web-assistant-markdown.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-23-web-assistant-markdown.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-23-web-assistant-markdown.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-primitives/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/client/ui-conversation/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-primitives/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/client/ui-conversation/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-primitives/src/TerminalBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/TerminalBlock.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. Defines `CodeBlock`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/plain-text.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/plain-text.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/client/ui-conversation/src/client/input/machine.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/machine.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-primitives/src/markdown/mathCompatibility.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/mathCompatibility.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. Defines `mathCompatibility`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-conversation/src/client/chat/GenericCommandCard.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/GenericCommandCard.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-conversation/src/client/conversation-nodes/command.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/command.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `AssistantMarkdown` | `const` | [`packages/client/ui-conversation/src/client/chat/AssistantMarkdown.tsx:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/AssistantMarkdown.tsx#L37) | `export const AssistantMarkdown = memo(function AssistantMarkdown({` |
| `CodeBlock` | `function` | [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx#L26) | `export function CodeBlock({ code, lang, className, copyLabel = '复制', copiedLabel = '复制成功' }: CodeBlockProps) {` |
| `MarkdownText` | `const` | [`packages/client/ui-primitives/src/markdown/MarkdownText.tsx:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/MarkdownText.tsx#L156) | `export const MarkdownText = memo(function MarkdownText({ text, streaming = false, codeLabels, fileMentions }: {` |
| `MessageText` | `function` | [`packages/client/ui-primitives/src/markdown/MessageText.tsx:5`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/MessageText.tsx#L5) | `export function MessageText({ text }: { text: string }) {` |
| `mathCompatibility` | `function` | [`packages/client/ui-primitives/src/markdown/mathCompatibility.ts:347`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/mathCompatibility.ts#L347) | `export function mathCompatibility(): Extension {` |

### Tests and executable evidence

- [`packages/client/ui-primitives/tests/markdown.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown.client.spec.tsx) — A test under the owning area exercises or imports `MarkdownText`. A test under the owning area exercises or imports `MessageText`.
- [`packages/client/ui-primitives/tests/code-block.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/code-block.client.spec.tsx) — A test under the owning area exercises or imports `MarkdownText`. A test under the owning area exercises or imports `CodeBlock`.
- [`packages/client/ui-conversation/tests/image-labels.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/image-labels.client.spec.tsx) — A test under the owning area exercises or imports `AssistantMarkdown`.
- [`packages/client/ui-conversation/tests/reasoning-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/reasoning-row.client.spec.tsx) — A test under the owning area exercises or imports `AssistantMarkdown`.
- [`packages/client/ui-conversation/tests/coverage-tails.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/coverage-tails.client.spec.tsx) — A test under the owning area exercises or imports `AssistantMarkdown`.
- [`packages/client/ui-primitives/tests/markdown-dom-parity.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown-dom-parity.client.spec.tsx) — A test under the owning area exercises or imports `MarkdownText`.
- [`packages/client/ui-conversation/tests/chat-branch-tails.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-branch-tails.client.spec.tsx) — A test under the owning area exercises or imports `AssistantMarkdown`.
- [`packages/client/ui-conversation/tests/gate-branch-tails.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/gate-branch-tails.client.spec.tsx) — A test under the owning area exercises or imports `AssistantMarkdown`.

## How to read the implementation

1. Start with [`packages/client/ui-primitives/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/trust`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `AssistantMarkdown`, `CodeBlock`, `MarkdownText`, `MessageText`, `mathCompatibility`, `@deepseek-ai/dsh-client-ui-primitives`, `ui-conversation`, `mdast-util-from-markdown`, `--shiki-*`, `@deepseek/md`, `markdown.css`, `code-block.css`, `--dsw-alias-markdown-*`, `--dsw-font-markdown-*`
- Regex: `(?i)(AssistantMarkdown|CodeBlock|MarkdownText|MessageText|mathCompatibility|@deepseek\-ai/dsh\-client\-ui\-primitives|ui\-conversation|mdast\-util\-from\-markdown)`

```bash
rg -n --pcre2 "(?i)(AssistantMarkdown|CodeBlock|MarkdownText|MessageText|mathCompatibility|@deepseek\\-ai/dsh\\-client\\-ui\\-primitives|ui\\-conversation|mdast\\-util\\-from\\-markdown)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0107. Incremental streaming markdown through a direct mdast renderer](0107-incremental-streaming-markdown-through-a-direct-mdast-renderer.md): The source note links to this decision directly.
- **`source-link`** — [0222. Remote Web Markdown images](0222-remote-web-markdown-images.md): The source note links to this decision directly.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0220. Web read card frontend --- the read tool's line window renders line-numbered and highlighted](0220-web-read-card-frontend-the-read-tool-s-line-window-renders-line-numbered.md): Shares source implementation: `packages/client/ui-primitives/src/TerminalBlock.tsx`, `packages/client/ui-primitives/src/index.ts`.
- **`shares-code-with`** — [0208. Ask-question Web presentation](0208-ask-question-web-presentation.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0294. Web attachment display aligns with DeepSeek Chat via attachment atoms](0294-web-attachment-display-aligns-with-deepseek-chat-via-attachment-atoms.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0412. Web client syntax highlighting --- synchronous fine-grained shiki](0412-web-client-syntax-highlighting-synchronous-fine-grained-shiki.md): Shares source implementation: `packages/client/ui-primitives/src/index.ts`, `packages/client/ui-primitives/src/invariant.ts`.
- **`shares-code-with`** — [0223. Web result card frontend --- rendering the web render intent in the browser](0223-web-result-card-frontend-rendering-the-web-render-intent-in-the-browser.md): Shares source implementation: `packages/client/ui-primitives/src/index.ts`, `packages/client/ui-primitives/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0177-safe-assistant-markdown-in-the-web-conversation.md`.
