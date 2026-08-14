---
id: "dsh-note-0207"
title: "Web terminal card --- the bash render intent reaches the browser"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-28-web-terminal-card.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/agent-loop"
  - "domain/configuration"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "ConversationSnapshot"
  - "card"
  - "StateDot"
  - "DEFAULT_TERMINAL_MAX_LINES"
  - "TerminalBlock"
  - "writeClipboard"
  - "CodeBlock"
  - "terminalCardModel"
  - "BashRow"
  - "title"
  - "command"
  - "viewFor"
  - "terminal"
  - "workdir"
search_regex: "(?i)(ConversationSnapshot|card|StateDot|DEFAULT_TERMINAL_MAX_LINES|TerminalBlock|writeClipboard|CodeBlock|terminalCardModel)"
---

# 0207. Web terminal card --- the bash render intent reaches the browser — implementation context

## Open this when

The bash tool declares card: 'terminal' for both its call and its result (render-intent union): the call view carries the command, an optional model-authored description, and the working directory; the result view carries the output, exit code, and terminating signal. That view already reaches the browser --- host, connection, and runtime deliver it onto ConversationSnapshot as callView/resultView --- and the former TUI rendered it as a $-prompt card with an exit line and a head/tail height cap. The Web client ignored it.

## Source decision

TerminalBlock is a ui-primitives component that renders a shell command as a terminal surface, and both Web render sites for a bash call consume the terminal render intent through it: the chat tool row's expanded body and the details panel's Output section. packages/client/ui-tool/src/client/tool/models/terminal-card-model.ts is the single place that turns the snapshot's callView/resultView pair into the component's props, so the two sites cannot disagree about a command, its cwd, or its exit status.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-28-web-terminal-card.md](../02-notes/implemented/feature/2026-07-28-web-terminal-card.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-28-web-terminal-card.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-28-web-terminal-card.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client/ui-tool/src`. The source note names this file directly. | `named-directory-member, named-file` |
| [`packages/client/ui-tool/src/client/tool/models/tool-call-model.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/models/tool-call-model.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/client/ui-tool/src/client/tool/models/terminal-card-model.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/models/terminal-card-model.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client/ui-tool/src`. The source note names this file directly. | `named-directory-member, named-file, symbol-definition` |
| [`packages/terminal/terminal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/index.ts) | package entry point | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/terminal/terminal/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/client/ui-primitives/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/client/ui-primitives`. Core file in the package named by the note: `packages/client/ui-primitives`. | `named-directory-member, named-package-member` |
| [`packages/terminal/terminal/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/client/ui-primitives/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/client/ui-primitives`. Core file in the package named by the note: `packages/client/ui-primitives`. | `named-directory-member, named-package-member` |
| [`packages/client/ui-primitives/src/clipboard.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/clipboard.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client/ui-primitives`. Core file in the package named by the note: `packages/client/ui-primitives`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/client/ui-primitives/src/HoverCard.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client/ui-primitives`. Core file in the package named by the note: `packages/client/ui-primitives`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/client/ui-primitives/src/TerminalBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/TerminalBlock.tsx) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client/ui-primitives`. Core file in the package named by the note: `packages/client/ui-primitives`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client/ui-primitives`. Core file in the package named by the note: `packages/client/ui-primitives`. | `named-directory-member, named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ConversationSnapshot` | `interface` | [`packages/client/runtime/src/client/sessions/conversation.ts:433`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts#L433) | `export interface ConversationSnapshot {` |
| `card` | `const` | [`packages/client/ui-primitives/src/HoverCard.tsx:140`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx#L140) | `const card = cardRef.current` |
| `card` | `const` | [`packages/client/ui-primitives/src/HoverCard.tsx:149`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx#L149) | `const card = open && pos !== null && (` |
| `StateDot` | `function` | [`packages/client/ui-primitives/src/StateDot.tsx:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/StateDot.tsx#L24) | `export function StateDot({ state, size = 10, className }: {` |
| `DEFAULT_TERMINAL_MAX_LINES` | `const` | [`packages/client/ui-primitives/src/TerminalBlock.tsx:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/TerminalBlock.tsx#L22) | `export const DEFAULT_TERMINAL_MAX_LINES = 16` |
| `TerminalBlock` | `function` | [`packages/client/ui-primitives/src/TerminalBlock.tsx:174`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/TerminalBlock.tsx#L174) | `export function TerminalBlock({` |
| `writeClipboard` | `function` | [`packages/client/ui-primitives/src/clipboard.ts:11`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/clipboard.ts#L11) | `export async function writeClipboard(text: string): Promise<boolean> {` |
| `CodeBlock` | `function` | [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx#L26) | `export function CodeBlock({ code, lang, className, copyLabel = '复制', copiedLabel = '复制成功' }: CodeBlockProps) {` |
| `terminalCardModel` | `function` | [`packages/client/ui-tool/src/client/tool/models/terminal-card-model.ts:182`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/models/terminal-card-model.ts#L182) | `export function terminalCardModel(block: ToolCallBlock, sessionCwd?: string): TerminalCardModel \| null {` |
| `BashRow` | `function` | [`packages/client/ui-tool/src/client/tool/toolviews/bash-sample.tsx:56`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/toolviews/bash-sample.tsx#L56) | `export function BashRow({ toolName, block, sessionId, useSessions, inspect, t }: BashRowProps) {` |
| `title` | `const` | [`packages/client/web/src/app.tsx:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/app.tsx#L32) | `const title = useSessions((state) => {` |
| `command` | `const` | [`packages/goal/command-goal/src/index.ts:111`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/command-goal/src/index.ts#L111) | `const command = parseGoalCommand(invocation.rawInput)` |
| `viewFor` | `function` | [`packages/host/apiproxy/src/api-proxy.ts:744`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L744) | `function viewFor(` |
| `terminal` | `const` | [`packages/jobs/jobs/src/invariant.ts:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/invariant.ts#L30) | `const terminal = TERMINAL_STATUSES.has(snapshot.status)` |
| `workdir` | `const` | [`packages/shell/tool-bash/src/index.ts:340`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts#L340) | `const workdir = resolveWorkdir(args.workdir, exec, standingPolicy?.workspaceRoot)` |
| `reverse` | `const` | [`packages/test-support/acp-snapshot/src/suite.ts:978`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts#L978) | `const reverse = new Map<string, string>()` |

### Tests and executable evidence

- [`apps/web/tests/navigation-panes.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/navigation-panes.e2e.ts) — The source note names this file directly.
- [`packages/client/ui-primitives/tests/ansi.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/ansi.client.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `TerminalBlock`.
- [`packages/client/ui-tool/tests/terminal-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/terminal-card.client.spec.tsx) — The source note names this file directly. A test under the owning area exercises or imports `terminalCardModel`.
- [`packages/client/ui-primitives/tests/terminal-block.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/terminal-block.client.spec.tsx) — The source note names this file directly. A test under the owning area exercises or imports `TerminalBlock`.
- [`packages/shell/tool-bash/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/tests/tools.spec.ts) — A test under the owning area exercises or imports `workdir`.
- [`packages/client/runtime/tests/session.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/session.client.spec.ts) — A test under the owning area exercises or imports `ConversationSnapshot`.
- [`packages/client/ui-tool/tests/web-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/web-card.client.spec.tsx) — A test under the owning area exercises or imports `BashRow`.
- [`packages/client/ui-primitives/tests/toast.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/toast.client.spec.tsx) — A test under the owning area exercises or imports `aria-hidden`.
- Source verification intent: packages/client/ui-primitives/tests/ansi.client.spec.ts pins the parse layer: token mapping for the basic colors, literal rgb for the values with no token, the background-run pair, every decoration and the textDecoration collision between two of them, the sanitizing of OSC strings and non-CSI escapes and inert controls, the cursor replay (redraws leaving a longer frame's tail standing, a trailing backspace erasing nothing, erase-in-line in all three parameter forms, tab stops, wide characters, SGR threading across lines, and a cursor/erase sequence never entering a cell style), and CRLF preservation.

## How to read the implementation

1. Start with [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/agent-loop`, `domain/configuration`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`
- Aliases: `ConversationSnapshot`, `card`, `StateDot`, `DEFAULT_TERMINAL_MAX_LINES`, `TerminalBlock`, `writeClipboard`, `CodeBlock`, `terminalCardModel`, `BashRow`, `title`, `command`, `viewFor`, `terminal`, `workdir`
- Regex: `(?i)(ConversationSnapshot|card|StateDot|DEFAULT_TERMINAL_MAX_LINES|TerminalBlock|writeClipboard|CodeBlock|terminalCardModel)`

```bash
rg -n --pcre2 "(?i)(ConversationSnapshot|card|StateDot|DEFAULT_TERMINAL_MAX_LINES|TerminalBlock|writeClipboard|CodeBlock|terminalCardModel)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0023. Tagged render-intent union for tool-call presentation](0023-tagged-render-intent-union-for-tool-call-presentation.md): The source note links to this decision directly.
- **`source-link`** — [0045. Web client architecture --- the client cordis plugin tree, the slot system, and the React-free object layer](0045-web-client-architecture-the-client-cordis-plugin-tree-the-slot-system-an.md): The source note links to this decision directly.
- **`source-link`** — [0206. Tool-call file open in OS](0206-tool-call-file-open-in-os.md): The source note links to this decision directly.
- **`source-link`** — [0408. Prefer maintained dependencies over hand-rolling](0408-prefer-maintained-dependencies-over-hand-rolling.md): The source note links to this decision directly.
- **`shares-code-with`** — [0225. Web search card --- the grep and glob render intent reaches the browser](0225-web-search-card-the-grep-and-glob-render-intent-reaches-the-browser.md): Shares source implementation: `packages/client/ui-primitives/src/index.ts`, `packages/terminal/terminal/src/index.ts`.
- **`shares-code-with`** — [0223. Web result card frontend --- rendering the web render intent in the browser](0223-web-result-card-frontend-rendering-the-web-render-intent-in-the-browser.md): Shares source implementation: `packages/client/ui-primitives/src/index.ts`, `packages/terminal/terminal/src/index.ts`.
- **`shares-code-with`** — [0218. Web diff card --- the write/edit render intent reaches the browser](0218-web-diff-card-the-write-edit-render-intent-reaches-the-browser.md): Shares source implementation: `packages/client/ui-primitives/src/index.ts`, `packages/terminal/terminal/src/index.ts`.
- **`shares-code-with`** — [0224. Web result card --- a structured render intent for web_search and web_fetch](0224-web-result-card-a-structured-render-intent-for-web-search-and-web-fetch.md): Shares source implementation: `packages/terminal/terminal/src/index.ts`, `packages/terminal/terminal/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0207-web-terminal-card-the-bash-render-intent-reaches-the-browser.md`.
