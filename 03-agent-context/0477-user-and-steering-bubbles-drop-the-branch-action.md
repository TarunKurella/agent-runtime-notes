---
id: "dsh-note-0477"
title: "User and steering bubbles drop the branch action"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-08-06-user-bubbles-drop-the-branch-action.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/agent-loop"
  - "domain/llm"
  - "domain/session-state"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "PendingSteeringBubble"
  - "turn/end"
  - "MessageItem"
  - "showBranch"
  - "messageBranchSeqs"
  - "assistantBranchSeqs"
  - "apps/web"
  - "User and steering bubbles drop the branch action"
  - "simplification"
  - "boundary"
  - "cancellation timeout"
  - "evidence"
  - "human control"
  - "lifecycle"
search_regex: "(?i)(PendingSteeringBubble|turn/end|MessageItem|showBranch|messageBranchSeqs|assistantBranchSeqs|apps/web|User[- ]and[- ]steering[- ]bubbles[- ]drop[- ]the[- ]branch[- ]action)"
---

# 0477. User and steering bubbles drop the branch action — implementation context

## Open this when

Every user and consumed-steering bubble rendered the branch control under the completed-turn-tail gate of the completed-turn-tail decision. On those bubbles the gate is effectively permanent: a turn-opening user message is followed by its own turn's nodes, and a consumed steering message is mid-turn by construction, so the control could enable only when the turn ended with no node after the message at all --- a cancel before the first model event. Readers therefore saw a control that never enables, with a tooltip promising a state the button cannot reach.

## Source decision

User and steering bubbles render no branch action. MessageItem loses its fork props, PendingSteeringBubble loses its showBranch special case, and messageBranchSeqs narrows to assistantBranchSeqs: only a completed turn's transcript tail that is the turn's own content-text assistant may fork. The branch affordance lives solely under the settled answer. A turn containing a steer keeps its fork point unchanged: fork is a log-prefix cut at turn/end, and the steer is model-visible history the child must inherit, so the settled answer of a steered turn forks like any other.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-08-06-user-bubbles-drop-the-branch-action.md](../02-notes/implemented/simplification/2026-08-06-user-bubbles-drop-the-branch-action.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-08-06-user-bubbles-drop-the-branch-action.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-08-06-user-bubbles-drop-the-branch-action.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/web/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `apps/web`. | `named-directory-member` |
| [`apps/web`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/web) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/client/ui-conversation/src/client/chat/MessageItem.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/MessageItem.tsx) | runtime implementation | Defines `PendingSteeringBubble`, a construct named by the note. | `symbol-definition` |
| [`knip.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/knip.json) | composition and configuration | Contains the exact code literal `apps/web` named by the note. | `exact-code-occurrence` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `apps/web` named by the note. | `exact-code-occurrence` |
| [`docs/development.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/development.md) | package contract and examples | Contains the exact code literal `apps/web` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`tsconfig.client.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.client.json) | composition and configuration | Contains the exact code literal `apps/web` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/development.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/development.zh.md) | package contract and examples | Contains the exact code literal `apps/web` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `PendingSteeringBubble` | `function` | [`packages/client/ui-conversation/src/client/chat/MessageItem.tsx:213`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/MessageItem.tsx#L213) | `export function PendingSteeringBubble({ content, loadImage, t }: {` |

### Tests and executable evidence

- [`packages/client/ui-conversation/tests/chat-view.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-view.client.spec.tsx) — A test under the owning area exercises or imports `MessageItem`.
- [`packages/client/ui-conversation/tests/chat-branch-tails.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-branch-tails.client.spec.tsx) — A test under the owning area exercises or imports `MessageItem`.
- [`scripts/release/families.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/release/families.spec.ts) — Contains the exact code literal `apps/web` named by the note.

## How to read the implementation

1. Start with [`apps/web/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/package.json) because it has the strongest evidence link to the note.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `domain/agent-loop`, `domain/llm`, `domain/session-state`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `PendingSteeringBubble`, `turn/end`, `MessageItem`, `showBranch`, `messageBranchSeqs`, `assistantBranchSeqs`, `apps/web`, `User and steering bubbles drop the branch action`, `simplification`, `boundary`, `cancellation timeout`, `evidence`, `human control`, `lifecycle`
- Regex: `(?i)(PendingSteeringBubble|turn/end|MessageItem|showBranch|messageBranchSeqs|assistantBranchSeqs|apps/web|User[- ]and[- ]steering[- ]bubbles[- ]drop[- ]the[- ]branch[- ]action)`

```bash
rg -n --pcre2 "(?i)(PendingSteeringBubble|turn/end|MessageItem|showBranch|messageBranchSeqs|assistantBranchSeqs|apps/web|User[- ]and[- ]steering[- ]bubbles[- ]drop[- ]the[- ]branch[- ]action)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0336. Message fork actions require a completed turn tail](0336-message-fork-actions-require-a-completed-turn-tail.md): The source note links to this decision directly.
- **`shares-code-with`** — [0314. Web GUI changes close the loop on the existing URL](0314-web-gui-changes-close-the-loop-on-the-existing-url.md): Shares source implementation: `apps/web`, `apps/web/package.json`.
- **`shares-code-with`** — [0676. Explicit-config dsh entrypoint](0676-explicit-config-dsh-entrypoint.md): Shares source implementation: `knip.json`, `pnpm-lock.yaml`.
- **`shares-code-with`** — [0065. Make packed chunk rows the default JSONL layout](0065-make-packed-chunk-rows-the-default-jsonl-layout.md): Shares source implementation: `apps/web`, `apps/web/package.json`.
- **`shares-code-with`** — [0596. `dsh migrate`/`dsh upgrade` seed the first turn with a skill](0596-dsh-migrate-dsh-upgrade-seed-the-first-turn-with-a-skill.md): Shares source implementation: `apps/web`, `apps/web/package.json`.
- **`shares-code-with`** — [0672. Replace tool-web's regex HTML-to-markdown converter with turndown](0672-replace-tool-web-s-regex-html-to-markdown-converter-with-turndown.md): Shares source implementation: `apps/web`, `apps/web/package.json`.
- **`shares-code-with`** — [0183. Session List Browsing and Manual Workspace Order](0183-session-list-browsing-and-manual-workspace-order.md): Shares source implementation: `apps/web`, `apps/web/package.json`.
- **`shares-code-with`** — [0465. Local JSON tree renderer](0465-local-json-tree-renderer.md): Shares source implementation: `knip.json`, `pnpm-lock.yaml`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0477-user-and-steering-bubbles-drop-the-branch-action.md`.
