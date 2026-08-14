---
id: "dsh-note-0222"
title: "Remote Web Markdown images"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-30-web-remote-markdown-images.md"
implementation_evidence: "medium"
target_anchor: "append-only event and session store"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/recovery"
  - "domain/session-state"
  - "domain/storage"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
aliases:
  - "MarkdownText"
  - "referrerPolicy=\"no-referrer"
  - "Remote Web Markdown images"
  - "feature"
  - "boundary"
  - "discovery routing"
  - "evidence"
  - "recovery"
  - "session state"
  - "storage"
  - "ui interaction"
  - "web retrieval"
  - "implemented"
  - "event sourcing"
search_regex: "(?i)(MarkdownText|referrerPolicy=\"no\\-referrer|Remote[- ]Web[- ]Markdown[- ]images|feature|boundary|discovery[- ]routing|evidence|recovery)"
---

# 0222. Remote Web Markdown images — implementation context

## Open this when

Assistant Markdown can name diagrams and screenshots with standard image syntax, but the Web renderer replaces every image with italic alt text. Even absolute HTTP(S) destinations therefore lose ordinary Markdown behavior.

## Source decision

MarkdownText renders absolute HTTP(S) image destinations as lazy, responsive elements with asynchronous decoding and referrerPolicy="no-referrer". Relative paths, absolute local paths, file: URLs, and unsupported schemes retain the existing alt-text fallback. Raw HTML stays disabled, so an assistant cannot bypass the Markdown image component with a hand-authored . The image component reuses the renderer's absolute-URL policy without adding a host proxy, local-file route, Session dependency, sanitizer, or image fetcher.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-30-web-remote-markdown-images.md](../02-notes/implemented/feature/2026-07-30-web-remote-markdown-images.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-30-web-remote-markdown-images.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-30-web-remote-markdown-images.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-primitives/src/markdown/MarkdownText.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/MarkdownText.tsx) | runtime implementation | Defines `MarkdownText`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `MarkdownText` | `const` | [`packages/client/ui-primitives/src/markdown/MarkdownText.tsx:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/MarkdownText.tsx#L156) | `export const MarkdownText = memo(function MarkdownText({ text, streaming = false, codeLabels, fileMentions }: {` |

### Tests and executable evidence

- [`packages/client/ui-primitives/tests/markdown.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown.client.spec.tsx) — A test under the owning area exercises or imports `MarkdownText`.
- [`packages/client/ui-primitives/tests/code-block.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/code-block.client.spec.tsx) — A test under the owning area exercises or imports `MarkdownText`.
- [`packages/client/ui-primitives/tests/markdown-dom-parity.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown-dom-parity.client.spec.tsx) — A test under the owning area exercises or imports `MarkdownText`.
- [`packages/client/ui-primitives/tests/markdown-incremental.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown-incremental.client.spec.tsx) — A test under the owning area exercises or imports `MarkdownText`.
- [`packages/client/ui-primitives/tests/markdown-render-units.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown-render-units.client.spec.tsx) — A test under the owning area exercises or imports `MarkdownText`.

## How to read the implementation

1. Start with [`packages/client/ui-primitives/src/markdown/MarkdownText.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/MarkdownText.tsx) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** append-only event and session store.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/recovery`, `domain/session-state`, `domain/storage`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`
- Aliases: `MarkdownText`, `referrerPolicy="no-referrer`, `Remote Web Markdown images`, `feature`, `boundary`, `discovery routing`, `evidence`, `recovery`, `session state`, `storage`, `ui interaction`, `web retrieval`, `implemented`, `event sourcing`
- Regex: `(?i)(MarkdownText|referrerPolicy="no\-referrer|Remote[- ]Web[- ]Markdown[- ]images|feature|boundary|discovery[- ]routing|evidence|recovery)`

```bash
rg -n --pcre2 "(?i)(MarkdownText|referrerPolicy=\"no\\-referrer|Remote[- ]Web[- ]Markdown[- ]images|feature|boundary|discovery[- ]routing|evidence|recovery)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0644. Personal staging maintenance skills](0644-personal-staging-maintenance-skills.md): Shares source implementation: `packages/client/ui-primitives/tests/markdown-dom-parity.client.spec.tsx`.
- **`shares-code-with`** — [0107. Incremental streaming markdown through a direct mdast renderer](0107-incremental-streaming-markdown-through-a-direct-mdast-renderer.md): Shares source implementation: `packages/client/ui-primitives/src/markdown/MarkdownText.tsx`.
- **`shares-code-with`** — [0412. Web client syntax highlighting --- synchronous fine-grained shiki](0412-web-client-syntax-highlighting-synchronous-fine-grained-shiki.md): Shares source implementation: `packages/client/ui-primitives/src/markdown/MarkdownText.tsx`.
- **`same-design-pressure`** — [0177. Safe assistant Markdown in the Web conversation](0177-safe-assistant-markdown-in-the-web-conversation.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/recovery`.
- **`same-design-pressure`** — [0220. Web read card frontend --- the read tool's line window renders line-numbered and highlighted](0220-web-read-card-frontend-the-read-tool-s-line-window-renders-line-numbered.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/evidence`.
- **`same-design-pressure`** — [0224. Web result card --- a structured render intent for web_search and web_fetch](0224-web-result-card-a-structured-render-intent-for-web-search-and-web-fetch.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/evidence`.
- **`same-design-pressure`** — [0219. Steer a queued Web message into the active turn](0219-steer-a-queued-web-message-into-the-active-turn.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/evidence`.
- **`same-design-pressure`** — [0207. Web terminal card --- the bash render intent reaches the browser](0207-web-terminal-card-the-bash-render-intent-reaches-the-browser.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/evidence`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0222-remote-web-markdown-images.md`.
