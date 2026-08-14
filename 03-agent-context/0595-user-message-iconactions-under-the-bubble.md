---
id: "dsh-note-0595"
title: "User-message IconActions under the bubble"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-27-user-message-icon-actions.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/recovery"
  - "domain/filesystem"
  - "domain/session-state"
  - "domain/testing"
  - "lifecycle/archived"
aliases:
  - "IconBranchOutline16"
  - "IconCopyOutline16"
  - "IconEditOutline16"
  - "User_Bubble/message_container"
  - "MessageItem"
  - "align-items: flex-end"
  - "navigator.clipboard.writeText"
  - "writeText"
  - "execCommand"
  - "User-message IconActions under the bubble"
  - "feature"
  - "evidence"
  - "human control"
  - "recovery"
search_regex: "(?i)(IconBranchOutline16|IconCopyOutline16|IconEditOutline16|User_Bubble/message_container|MessageItem|align\\-items:[- ]flex\\-end|navigator\\.clipboard\\.writeText|writeText)"
---

# 0595. User-message IconActions under the bubble — implementation context

## Open this when

The chat user bubble had no under-bubble action chrome. The Harness design (figma User_Bubble/message_container) shows three IconActions --- copy, branch in new chat, and edit --- right-aligned under the bubble, matching the product action-bar pattern used elsewhere.

## Source decision

MessageItem owns the actions for kind: 'user' only. Layout is a column (align-items: flex-end, 6px gap): bubble, then a 28px action row with 10px gaps and 28px circular icon buttons (IconCopyOutline16, IconBranchOutline16, IconEditOutline16). Tooltips carry Chinese labels. Actions stay visible by default; @media (hover: hover) hides them until the row is hovered or focus-within, so touch / hover: none devices keep discoverable controls (opacity alone still hit-tests). Copy writes the bubble's joined text blocks to the clipboard (navigator.clipboard.writeText, with an execCommand fallback).

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-27-user-message-icon-actions.md](../02-notes/archived/feature/2026-07-27-user-message-icon-actions.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-27-user-message-icon-actions.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-27-user-message-icon-actions.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-primitives/src/icons/index.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/icons/index.tsx) | package entry point | Defines `IconCopyOutline16`, a construct named by the note. Defines `IconBranchOutline16`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/MessageItem.module.css`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/MessageItem.module.css) | runtime implementation | Contains the exact code literal `User_Bubble/message_container` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `IconBranchOutline16` | `const` | [`packages/client/ui-primitives/src/icons/index.tsx:149`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/icons/index.tsx#L149) | `export const IconBranchOutline16 = ({ size = 16, className }: IconProps) => (` |
| `IconCopyOutline16` | `const` | [`packages/client/ui-primitives/src/icons/index.tsx:235`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/icons/index.tsx#L235) | `export const IconCopyOutline16 = ({ size = 16, className }: IconProps) => (` |
| `IconEditOutline16` | `const` | [`packages/client/ui-primitives/src/icons/index.tsx:323`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/icons/index.tsx#L323) | `export const IconEditOutline16 = ({ size = 16, className }: IconProps) => (` |

### Tests and executable evidence

- [`packages/fs/fs/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/tests/service.spec.ts) — A test under the owning area exercises or imports `writeText`.
- [`packages/fs/tool-fs/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/tools.spec.ts) — A test under the owning area exercises or imports `writeText`.
- [`packages/e2b/fs-e2b/tests/filesystem.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/tests/filesystem.spec.ts) — A test under the owning area exercises or imports `writeText`.
- [`packages/fs/fs-local/tests/filesystem.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/tests/filesystem.spec.ts) — A test under the owning area exercises or imports `writeText`.
- [`packages/fs/fs-sandbox/tests/fs-sandbox.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-sandbox/tests/fs-sandbox.spec.ts) — A test under the owning area exercises or imports `writeText`.
- [`examples/headless-agent/tests/fixtures/e2b/e2b/bin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/headless-agent/tests/fixtures/e2b/e2b/bin.ts) — A test under the owning area exercises or imports `writeText`.
- [`packages/client/ui-workspace/tests/rows.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/tests/rows.client.spec.tsx) — A test under the owning area exercises or imports `writeText`.
- [`packages/fs/tool-str-replace-editor/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-str-replace-editor/tests/tools.spec.ts) — A test under the owning area exercises or imports `writeText`.

## How to read the implementation

1. Start with [`packages/client/ui-primitives/src/icons/index.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/icons/index.tsx) because it has the strongest evidence link to the note.
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
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/evidence`, `concern/human-control`, `concern/recovery`, `domain/filesystem`, `domain/session-state`, `domain/testing`, `lifecycle/archived`
- Aliases: `IconBranchOutline16`, `IconCopyOutline16`, `IconEditOutline16`, `User_Bubble/message_container`, `MessageItem`, `align-items: flex-end`, `navigator.clipboard.writeText`, `writeText`, `execCommand`, `User-message IconActions under the bubble`, `feature`, `evidence`, `human control`, `recovery`
- Regex: `(?i)(IconBranchOutline16|IconCopyOutline16|IconEditOutline16|User_Bubble/message_container|MessageItem|align\-items:[- ]flex\-end|navigator\.clipboard\.writeText|writeText)`

```bash
rg -n --pcre2 "(?i)(IconBranchOutline16|IconCopyOutline16|IconEditOutline16|User_Bubble/message_container|MessageItem|align\\-items:[- ]flex\\-end|navigator\\.clipboard\\.writeText|writeText)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0188. Code Mode chat rendering --- sub-calls as native rows under the parent](0188-code-mode-chat-rendering-sub-calls-as-native-rows-under-the-parent.md): Shares source implementation: `packages/client/ui-primitives/src/icons/index.tsx`.
- **`shares-code-with`** — [0626. TUI diff context lines stay neutral](0626-tui-diff-context-lines-stay-neutral.md): Shares source implementation: `packages/fs/tool-fs/tests/tools.spec.ts`.
- **`shares-code-with`** — [0358. The minimal preset owns the complete RL agent composition](0358-the-minimal-preset-owns-the-complete-rl-agent-composition.md): Shares source implementation: `packages/fs/fs-local/tests/filesystem.spec.ts`.
- **`shares-code-with`** — [0221. Read card --- the read tool's structured line window reaches the client](0221-read-card-the-read-tool-s-structured-line-window-reaches-the-client.md): Shares source implementation: `packages/fs/tool-fs/tests/tools.spec.ts`.
- **`shares-code-with`** — [0465. Local JSON tree renderer](0465-local-json-tree-renderer.md): Shares source implementation: `packages/client/ui-primitives/tests/json-tree.client.spec.tsx`.
- **`shares-code-with`** — [0317. Isolate pnpm setup per GitHub Actions runner](0317-isolate-pnpm-setup-per-github-actions-runner.md): Shares source implementation: `packages/e2b/fs-e2b/tests/filesystem.spec.ts`.
- **`same-design-pressure`** — [0597. `dsh meta` boots the TUI over the harness checkout](0597-dsh-meta-boots-the-tui-over-the-harness-checkout.md): Shares design concerns: `concern/evidence`, `concern/human-control`, `concern/recovery`.
- **`same-design-pressure`** — [0579. Product-level TUI session resume](0579-product-level-tui-session-resume.md): Shares design concerns: `concern/evidence`, `concern/human-control`, `concern/recovery`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0595-user-message-iconactions-under-the-bubble.md`.
