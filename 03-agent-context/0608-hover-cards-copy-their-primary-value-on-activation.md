---
id: "dsh-note-0608"
title: "Hover cards copy their primary value on activation"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-31-hover-card-click-copy.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/context"
  - "domain/filesystem"
  - "domain/session-state"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/archived"
aliases:
  - "copyText"
  - "HoverCard"
  - "textContent"
  - "copyLabel"
  - "copiedLabel"
  - "Hover cards copy their primary value on activation"
  - "feature"
  - "boundary"
  - "discovery routing"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "recovery"
  - "context"
search_regex: "(?i)(copyText|HoverCard|textContent|copyLabel|copiedLabel|Hover[- ]cards[- ]copy[- ]their[- ]primary[- ]value[- ]on[- ]activation|feature|boundary)"
---

# 0608. Hover cards copy their primary value on activation — implementation context

## Open this when

Workspace and Session rows clip the two values their hover cards expose in full: the Workspace directory path and Session title. The reachable card permits text selection, but selecting and copying a single known value is a needlessly precise gesture, and the card gives no confirmation that the clipboard accepted it.

## Source decision

HoverCard accepts an optional copyText plus copyLabel and copiedLabel. With copyText, the whole card has button semantics for pointer and keyboard activation; its accessible name combines the localized action prefix with the exact value, it writes that value through the shared clipboard helper, and it replaces its content with the success label for up to one second only after the host accepts the write. The feedback retains the pre-copy card height and clears with the card. Without copyText, the atom retains its read/select-only behavior.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-31-hover-card-click-copy.md](../02-notes/archived/feature/2026-07-31-hover-card-click-copy.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-31-hover-card-click-copy.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-31-hover-card-click-copy.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-primitives/src/JsonTree.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/JsonTree.tsx) | runtime implementation | Defines `copyText`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/HoverCard.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx) | runtime implementation | Defines `HoverCard`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/DiffBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DiffBlock.tsx) | runtime implementation | Defines `copyText`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/SearchBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/SearchBlock.tsx) | runtime implementation | Defines `copyText`, a construct named by the note. | `symbol-definition` |
| [`packages/context/session-reference/src/projection.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/projection.ts) | runtime implementation | Defines `textContent`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `copyText` | `function` | [`packages/client/ui-primitives/src/DiffBlock.tsx:123`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DiffBlock.tsx#L123) | `function copyText(rows: DiffRow[]): string {` |
| `HoverCard` | `function` | [`packages/client/ui-primitives/src/HoverCard.tsx:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx#L31) | `export function HoverCard({` |
| `copyText` | `function` | [`packages/client/ui-primitives/src/JsonTree.tsx:374`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/JsonTree.tsx#L374) | `function copyText(target: CopyTarget, mode: 'json' \| 'path' \| 'prettyJson' \| 'value'): string {` |
| `copyText` | `function` | [`packages/client/ui-primitives/src/SearchBlock.tsx:93`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/SearchBlock.tsx#L93) | `function copyText(props: SearchBlockProps): string {` |
| `textContent` | `function` | [`packages/context/session-reference/src/projection.ts:140`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/projection.ts#L140) | `function textContent(content: readonly { type: string; text?: string }[]): string {` |

### Tests and executable evidence

- [`apps/web/tests/message-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/message-actions.e2e.ts) — A test under the owning area exercises or imports `Copy`.
- [`apps/web/tests/navigation-panes.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/navigation-panes.e2e.ts) — A test under the owning area exercises or imports `Copy`. A test under the owning area exercises or imports `Copied`.
- [`apps/web/tests/turn-tail-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/turn-tail-actions.e2e.ts) — A test under the owning area exercises or imports `Copy`.
- [`apps/web/tests/live-interactions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/live-interactions.e2e.ts) — A test under the owning area exercises or imports `copied`.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `copied`.
- [`apps/web/tests/workspace-management.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/workspace-management.e2e.ts) — A test under the owning area exercises or imports `Copy`. A test under the owning area exercises or imports `Copied`.
- [`apps/web/tests/chat-long-interactions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/chat-long-interactions.e2e.ts) — A test under the owning area exercises or imports `Copy`.
- [`apps/web/tests/agent-preset-authoring.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/agent-preset-authoring.e2e.ts) — A test under the owning area exercises or imports `copied`.

## How to read the implementation

1. Start with [`packages/client/ui-primitives/src/JsonTree.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/JsonTree.tsx) because it has the strongest evidence link to the note.
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
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/context`, `domain/filesystem`, `domain/session-state`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/archived`
- Aliases: `copyText`, `HoverCard`, `textContent`, `copyLabel`, `copiedLabel`, `Hover cards copy their primary value on activation`, `feature`, `boundary`, `discovery routing`, `evidence`, `lifecycle`, `ownership`, `recovery`, `context`
- Regex: `(?i)(copyText|HoverCard|textContent|copyLabel|copiedLabel|Hover[- ]cards[- ]copy[- ]their[- ]primary[- ]value[- ]on[- ]activation|feature|boundary)`

```bash
rg -n --pcre2 "(?i)(copyText|HoverCard|textContent|copyLabel|copiedLabel|Hover[- ]cards[- ]copy[- ]their[- ]primary[- ]value[- ]on[- ]activation|feature|boundary)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0341. The conversation column scrolls on one axis](0341-the-conversation-column-scrolls-on-one-axis.md): Shares source implementation: `packages/client/ui-primitives/src/DiffBlock.tsx`, `packages/client/ui-primitives/src/JsonTree.tsx`.
- **`shares-code-with`** — [0220. Web read card frontend --- the read tool's line window renders line-numbered and highlighted](0220-web-read-card-frontend-the-read-tool-s-line-window-renders-line-numbered.md): Shares source implementation: `packages/client/ui-primitives/src/HoverCard.tsx`.
- **`shares-code-with`** — [0207. Web terminal card --- the bash render intent reaches the browser](0207-web-terminal-card-the-bash-render-intent-reaches-the-browser.md): Shares source implementation: `apps/web/tests/navigation-panes.e2e.ts`.
- **`shares-code-with`** — [0194. Trajectory inspection ledger](0194-trajectory-inspection-ledger.md): Shares source implementation: `apps/web/tests/message-actions.e2e.ts`.
- **`shares-code-with`** — [0323. Hover popup pointer grace](0323-hover-popup-pointer-grace.md): Shares source implementation: `packages/client/ui-primitives/src/HoverCard.tsx`.
- **`shares-code-with`** — [0219. Steer a queued Web message into the active turn](0219-steer-a-queued-web-message-into-the-active-turn.md): Shares source implementation: `apps/cli/tests/web-agent-presets.e2e.ts`.
- **`shares-code-with`** — [0196. Web session fork actions](0196-web-session-fork-actions.md): Shares source implementation: `apps/web/tests/message-actions.e2e.ts`.
- **`shares-code-with`** — [0366. Preset cards clamp their description instead of sizing the roster](0366-preset-cards-clamp-their-description-instead-of-sizing-the-roster.md): Shares source implementation: `packages/client/ui-primitives/src/HoverCard.tsx`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0608-hover-cards-copy-their-primary-value-on-activation.md`.
