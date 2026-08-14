---
id: "dsh-note-0312"
title: "The scrollbar tokens get their consumer, and the workspace list reserves its gutter"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-28-themed-scrollbars-and-reserved-gutter.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "InputBar"
  - "TodoPanel"
  - "DirectoryBrowser"
  - "Menu"
  - "Modal"
  - "html"
  - "build"
  - "QuestionComposer"
  - "body"
  - "stable"
  - "bundle"
  - "design-platform.css"
  - "--dsw-alias-scrollbar-*"
  - "bg-l1"
search_regex: "(?i)(InputBar|TodoPanel|DirectoryBrowser|Menu|Modal|html|build|QuestionComposer)"
---

# 0312. The scrollbar tokens get their consumer, and the workspace list reserves its gutter — implementation context

## Open this when

design-platform.css declares four --dsw-alias-scrollbar- tokens (bg-l1, bg-l2, hover-l1, hover-l2) in both palettes, and no rule anywhere in the client read them. A defined token with no consumer is not a theme: every scrolling region rendered the user agent's own scrollbar, which knows nothing about the palette, so the dark theme showed a light native bar against dark surfaces. The visible symptom that surfaced the gap was elsewhere.

## Source decision

packages/client/ui-theme/src/styles/scrollbar.css is the sole consumer of the four tokens, and the fifth ui-theme sheet in the shell's import chain (packages/client/web/src/base.css). It follows design-platform.css there because it reads that sheet's tokens. The rules sit on body, not html. design-platform.css declares the --dsw-alias- tokens on body, with the dark overrides on body[data-ds-dark-theme], and custom properties inherit only downward; an html rule resolves them to the guaranteed-invalid value, at which point scrollbar-color computes to auto and no theming happens at all.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-28-themed-scrollbars-and-reserved-gutter.md](../02-notes/implemented/bug-fix/2026-07-28-themed-scrollbars-and-reserved-gutter.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-28-themed-scrollbars-and-reserved-gutter.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-28-themed-scrollbars-and-reserved-gutter.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/web/src/base.css`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/base.css) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/client/ui-theme/src/styles/scrollbar.css`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/styles/scrollbar.css) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/client/ui-workspace/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-workspace`. | `named-package-member` |
| [`packages/client/ui-workspace/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-workspace`. | `named-package-member` |
| [`packages/client/ui-workspace`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/fs/tool-fs/src/read.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts) | runtime implementation | Defines `body`, a construct named by the note. | `symbol-definition` |
| [`scripts/publish-npm-baseline.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publish-npm-baseline.ts) | repository automation | Defines `bundle`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-slots/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts) | package entry point | Defines `build`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/Menu.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Menu.tsx) | runtime implementation | Defines `Menu`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/Modal.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Modal.tsx) | runtime implementation | Defines `Modal`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/acp-snapshot/src/suite.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts) | runtime implementation | Defines `stable`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx) | runtime implementation | Defines `html`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `InputBar` | `function` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L46) | `export function InputBar({` |
| `TodoPanel` | `function` | [`packages/client/ui-conversation/src/client/skeleton/TodoPanel.tsx:95`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/TodoPanel.tsx#L95) | `export function TodoPanel({ todos, t }: TodoPanelProps) {` |
| `DirectoryBrowser` | `function` | [`packages/client/ui-directory-picker-browse/src/client/DirectoryBrowser.tsx:262`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-directory-picker-browse/src/client/DirectoryBrowser.tsx#L262) | `export function DirectoryBrowser({ open, listDirectory, createDirectory, onOpen, onClose, busy, t }: DirectoryBrowserProps) {` |
| `Menu` | `function` | [`packages/client/ui-primitives/src/Menu.tsx:90`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Menu.tsx#L90) | `export function Menu({ open, anchor, items, selectedId, selectedIds, onSelect, onClose, align = 'start', side = 'bottom', portal = false, closeOnPointerLeave = false, dense = false, compact = false, getAnchorRect, footer` |
| `Modal` | `function` | [`packages/client/ui-primitives/src/Modal.tsx:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Modal.tsx#L30) | `export function Modal({` |
| `html` | `const` | [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx#L32) | `const html = useMemo(() => highlightToHtml(trimmed, lang), [trimmed, lang, loaded])` |
| `build` | `const` | [`packages/client/ui-slots/src/index.ts:980`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L980) | `const build = (name: string, seen: Set<string>): LiveSlotNode \| undefined => {` |
| `QuestionComposer` | `function` | [`packages/client/ui-user-questions/src/client/QuestionComposer.tsx:60`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/QuestionComposer.tsx#L60) | `export function QuestionComposer(props: QuestionComposerProps) {` |
| `body` | `const` | [`packages/fs/tool-fs/src/read.ts:180`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts#L180) | `const body = /^<path>[^\n]*<\/path>\n<type>file<\/type>\n<content>\n([\s\S]*)\n<\/content>$/u.exec(text)?.[1]` |
| `stable` | `let` | [`packages/test-support/acp-snapshot/src/suite.ts:658`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts#L658) | `let stable = content` |
| `bundle` | `const` | [`scripts/publish-npm-baseline.ts:384`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publish-npm-baseline.ts#L384) | `const bundle = new ReleaseBundle(directory, manifest)` |

### Tests and executable evidence

- [`apps/web/tests/sidebar-scrollbar.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/sidebar-scrollbar.e2e.ts) — The source note names this file directly. Contains the exact code literal `packages/client/ui-theme/src/styles/scrollbar.css` named by the note.
- [`packages/client/ui-theme/tests/scrollbar-styles.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/scrollbar-styles.client.spec.ts) — The source note names this file directly.
- [`packages/client/ui-workspace/tests/rows.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/tests/rows.client.spec.tsx) — A test under the owning area exercises or imports `Menu`.
- [`packages/client/ui-primitives/tests/atoms.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/atoms.client.spec.tsx) — A test under the owning area exercises or imports `Menu`. A test under the owning area exercises or imports `Modal`.
- [`packages/client/ui-primitives/tests/markdown.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown.client.spec.tsx) — A test under the owning area exercises or imports `html`.
- [`packages/client/ui-primitives/tests/code-block.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/code-block.client.spec.tsx) — A test under the owning area exercises or imports `html`.
- [`packages/client/ui-conversation/tests/skeleton.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/skeleton.client.spec.tsx) — A test under the owning area exercises or imports `InputBar`.
- [`packages/client/ui-conversation/tests/input-bar.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-bar.client.spec.tsx) — A test under the owning area exercises or imports `InputBar`.
- Source verification intent: Three unit specs read the CSS text on disk. ui-theme/tests/scrollbar-styles.spec.ts scans the scrollbar token set out of design-platform.css rather than hardcoding it, so adding, renaming, or dropping a token moves the assertions with it, and checks that every token has a consumer and that each elevated surface rebinds a complete pair. It also pins the path split by source offset: the standard properties inside the gate block, the ::-webkit-scrollbar rules and every read of the hover indirection outside it.

## How to read the implementation

1. Start with [`packages/client/web/src/base.css`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/base.css) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `domain/build-release`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `InputBar`, `TodoPanel`, `DirectoryBrowser`, `Menu`, `Modal`, `html`, `build`, `QuestionComposer`, `body`, `stable`, `bundle`, `design-platform.css`, `--dsw-alias-scrollbar-*`, `bg-l1`
- Regex: `(?i)(InputBar|TodoPanel|DirectoryBrowser|Menu|Modal|html|build|QuestionComposer)`

```bash
rg -n --pcre2 "(?i)(InputBar|TodoPanel|DirectoryBrowser|Menu|Modal|html|build|QuestionComposer)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0250. The sidebar's scrollbars follow the pointer](0250-the-sidebar-s-scrollbars-follow-the-pointer.md): The source note links to this decision directly.
- **`shares-code-with`** — [0472. One route to add a Workspace](0472-one-route-to-add-a-workspace.md): Shares source implementation: `packages/client/ui-workspace`, `packages/client/ui-workspace/src/index.ts`.
- **`shares-code-with`** — [0629. Collapsed sidebar upper controls share one entry motion](0629-collapsed-sidebar-upper-controls-share-one-entry-motion.md): Shares source implementation: `packages/client/ui-workspace`, `packages/client/ui-workspace/src/index.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/fs/tool-fs/src/read.ts`.
- **`shares-code-with`** — [0221. Read card --- the read tool's structured line window reaches the client](0221-read-card-the-read-tool-s-structured-line-window-reaches-the-client.md): Shares source implementation: `packages/fs/tool-fs/src/read.ts`.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-slots/src/index.ts`.
- **`shares-code-with`** — [0398. GUI testing system --- the three-tier structure](0398-gui-testing-system-the-three-tier-structure.md): Shares source implementation: `packages/client/ui-slots/src/index.ts`.
- **`shares-code-with`** — [0528. Prune dead public API and result fields](0528-prune-dead-public-api-and-result-fields.md): Shares source implementation: `packages/fs/tool-fs/src/read.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0312-the-scrollbar-tokens-get-their-consumer-and-the-workspace-list-reserves.md`.
