---
id: "dsh-note-0244"
title: "Web thinking tail scroll --- collapsed reasoning follows live output"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-02-web-thinking-tail-scroll.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/performance"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
aliases:
  - "AssistantMarkdown"
  - "ToolRow"
  - "scrollWidth - clientWidth"
  - "scrollLeft"
  - "packages/client/ui-conversation/tests/reasoning-row.client.spec.tsx"
  - "scrollLeft = 0"
  - "apps/web/tests/lifecycle-chrome.e2e.ts"
  - "Web thinking tail scroll --- collapsed reasoning follows live output"
  - "feature"
  - "boundary"
  - "discovery routing"
  - "evidence"
  - "lifecycle"
  - "performance"
search_regex: "(?i)(AssistantMarkdown|ToolRow|scrollWidth[- ]\\-[- ]clientWidth|scrollLeft|scrollLeft[- ]=[- ]0|apps/web/tests/lifecycle\\-chrome\\.e2e\\.ts|feature|boundary)"
---

# 0244. Web thinking tail scroll --- collapsed reasoning follows live output — implementation context

## Open this when

The Web Think row rendered the first reasoning line as its collapsed summary for both settled and streaming blocks. Once that first line existed, every later reasoning delta changed hidden body text only. A fast model therefore looked stationary while it was thinking, and the user had to expand the full chain of thought to verify that output was still moving. The product backlog already called for "thinking: scrolling chain-of-thought updates, expandable"; the current row satisfied only the second half.

## Source decision

Only a collapsed Think row whose reasoning block is the active streaming tail follows live output. Its summary is the latest non-blank line instead of the settled first line, and the existing single-line summary element becomes a programmatic horizontal scrollport pinned to scrollWidth - clientWidth after each text update. Direct scrollLeft assignment deliberately follows real deltas without inventing an independent marquee speed: fast tokens move fast, a paused model stops, and short text stays still because the scroll range is zero. The behavior is owned by the existing presentation components.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-02-web-thinking-tail-scroll.md](../02-notes/implemented/feature/2026-08-02-web-thinking-tail-scroll.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-02-web-thinking-tail-scroll.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-02-web-thinking-tail-scroll.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx) | runtime implementation | Defines `ToolRow`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/AssistantMarkdown.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/AssistantMarkdown.tsx) | runtime implementation | Defines `AssistantMarkdown`, a construct named by the note. | `symbol-definition` |
| [`tsconfig.host.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.host.json) | composition and configuration | Contains the exact code literal `apps/web/tests/lifecycle-chrome.e2e.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `AssistantMarkdown` | `const` | [`packages/client/ui-conversation/src/client/chat/AssistantMarkdown.tsx:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/AssistantMarkdown.tsx#L37) | `export const AssistantMarkdown = memo(function AssistantMarkdown({` |
| `ToolRow` | `function` | [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx:128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx#L128) | `export function ToolRow({` |

### Tests and executable evidence

- [`apps/web/tests/lifecycle-chrome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/lifecycle-chrome.e2e.ts) — The source note names this file directly. A test under the owning area exercises or imports `scrollLeft`.
- [`packages/client/ui-conversation/tests/reasoning-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/reasoning-row.client.spec.tsx) — The source note names this file directly. A test under the owning area exercises or imports `scrollLeft`.
- [`apps/web/tests/conversation-column-overflow.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/conversation-column-overflow.e2e.ts) — A test under the owning area exercises or imports `scrollLeft`.
- [`packages/client/ui-tool/tests/web-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/web-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/tool-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/tool-row.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/diff-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/diff-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/read-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/read-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/search-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/search-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- Source verification intent: packages/client/ui-conversation/tests/reasoning-row.client.spec.tsx pins the latest-line selection, the calculated right-edge scroll position, and the settlement reset to the first line and scrollLeft = 0. The keyless assembled Chromium scenario in apps/web/tests/lifecycle-chrome.e2e.ts replays real recorded reasoning chunks at observable pacing, narrows the viewport until the summary overflows, and asserts that the live collapsed Think row reaches its actual browser scroll extent. Its settled replay golden remains unchanged, proving the historical summary contract stays stable.

## How to read the implementation

1. Start with [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/performance`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`
- Aliases: `AssistantMarkdown`, `ToolRow`, `scrollWidth - clientWidth`, `scrollLeft`, `packages/client/ui-conversation/tests/reasoning-row.client.spec.tsx`, `scrollLeft = 0`, `apps/web/tests/lifecycle-chrome.e2e.ts`, `Web thinking tail scroll --- collapsed reasoning follows live output`, `feature`, `boundary`, `discovery routing`, `evidence`, `lifecycle`, `performance`
- Regex: `(?i)(AssistantMarkdown|ToolRow|scrollWidth[- ]\-[- ]clientWidth|scrollLeft|scrollLeft[- ]=[- ]0|apps/web/tests/lifecycle\-chrome\.e2e\.ts|feature|boundary)`

```bash
rg -n --pcre2 "(?i)(AssistantMarkdown|ToolRow|scrollWidth[- ]\\-[- ]clientWidth|scrollLeft|scrollLeft[- ]=[- ]0|apps/web/tests/lifecycle\\-chrome\\.e2e\\.ts|feature|boundary)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0220. Web read card frontend --- the read tool's line window renders line-numbered and highlighted](0220-web-read-card-frontend-the-read-tool-s-line-window-renders-line-numbered.md): Shares source implementation: `packages/client/ui-tool/tests/read-card.client.spec.tsx`.
- **`shares-code-with`** — [0606. Web context injection disclosure](0606-web-context-injection-disclosure.md): Shares source implementation: `packages/client/ui-tool/src/client/tool/components/ToolRow.tsx`, `packages/client/ui-tool/tests/web-card.client.spec.tsx`.
- **`shares-code-with`** — [0207. Web terminal card --- the bash render intent reaches the browser](0207-web-terminal-card-the-bash-render-intent-reaches-the-browser.md): Shares source implementation: `packages/client/ui-tool/src/client/tool/components/ToolRow.tsx`.
- **`shares-code-with`** — [0247. Web search source card scrolls instead of collapsing](0247-web-search-source-card-scrolls-instead-of-collapsing.md): Shares source implementation: `packages/client/ui-tool/tests/web-card.client.spec.tsx`.
- **`shares-code-with`** — [0225. Web search card --- the grep and glob render intent reaches the browser](0225-web-search-card-the-grep-and-glob-render-intent-reaches-the-browser.md): Shares source implementation: `packages/client/ui-tool/tests/search-card.client.spec.tsx`.
- **`shares-code-with`** — [0223. Web result card frontend --- rendering the web render intent in the browser](0223-web-result-card-frontend-rendering-the-web-render-intent-in-the-browser.md): Shares source implementation: `packages/client/ui-tool/tests/web-card.client.spec.tsx`.
- **`shares-code-with`** — [0218. Web diff card --- the write/edit render intent reaches the browser](0218-web-diff-card-the-write-edit-render-intent-reaches-the-browser.md): Shares source implementation: `packages/client/ui-tool/tests/diff-card.client.spec.tsx`.
- **`shares-code-with`** — [0237. opening a produced file from the web UI](0237-opening-a-produced-file-from-the-web-ui.md): Shares source implementation: `packages/client/ui-tool/src/client/tool/components/ToolRow.tsx`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0244-web-thinking-tail-scroll-collapsed-reasoning-follows-live-output.md`.
