---
id: "dsh-note-0622"
title: "Web conversation UI polish sweep"
status: "archived"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/bug-fix/2026-07-28-web-conversation-polish-sweep.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
aliases:
  - "body"
  - "toolRowModel"
  - "cwd"
  - "useLayoutEffect"
  - "ToolRowOwnerProps.cwd"
  - "mask-image"
  - "-webkit-font-smoothing"
  - "chat-view.spec.tsx"
  - "chat-tool-row.spec.tsx"
  - "atoms.spec.tsx"
  - "workspace-picker.spec.tsx"
  - "Web conversation UI polish sweep"
  - "bug fix"
  - "boundary"
search_regex: "(?i)(body|toolRowModel|useLayoutEffect|ToolRowOwnerProps\\.cwd|mask\\-image|\\-webkit\\-font\\-smoothing|chat\\-view\\.spec\\.tsx|chat\\-tool\\-row\\.spec\\.tsx)"
---

# 0622. Web conversation UI polish sweep — implementation context

## Open this when

A design review of the web GUI's conversation surfaces found a batch of presentation defects: portal menus painted one frame at the wrong position before repositioning (visible open jump), the chat column split one tool run into several groups whenever a step message carried only tool-call heads, tool row summaries printed workspace-absolute paths that consumed most of the row, the running-row sweep was implemented as an alpha mask that dimmed the whole row, the hero workspace chip resurrected a deleted workspace's folder name from the session cwd, and the header showed a turns counter nobody asked for next.

## Source decision

The sweep lands as presentation-layer changes only; nothing enters the session log. Portal menus pre-render hidden and measure before paint. The menu list mounts with visibility: hidden at (0,0), measures in useLayoutEffect, and becomes visible already at its final position. Menus keep 12px viewport clearance with internal scroll; workspace create actions pin in a non-scrolling footer. The chat flow skips assistant nodes that render nothing.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/bug-fix/2026-07-28-web-conversation-polish-sweep.md](../02-notes/archived/bug-fix/2026-07-28-web-conversation-polish-sweep.md)
- Pinned source: [.agents/notes/archived/bug-fix/2026-07-28-web-conversation-polish-sweep.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/bug-fix/2026-07-28-web-conversation-polish-sweep.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/fs/tool-fs/src/read.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts) | runtime implementation | Defines `body`, a construct named by the note. | `symbol-definition` |
| [`packages/api/gateway/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts) | package entry point | Defines `body`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Defines `body`, a construct named by the note. | `symbol-definition` |
| [`packages/jobs/tool-jobs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts) | package entry point | Defines `body`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/src/session-cwd.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts) | runtime implementation | Defines `cwd`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `cwd`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs-search/src/search-core.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/search-core.ts) | runtime implementation | Defines `cwd`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-tool/src/client/tool/models/tool-call-model.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/models/tool-call-model.ts) | runtime implementation | Defines `toolRowModel`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `body` | `const` | [`packages/api/gateway/src/index.ts:567`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts#L567) | `const body = source.slice(open + 1, close).trim()` |
| `toolRowModel` | `function` | [`packages/client/ui-tool/src/client/tool/models/tool-call-model.ts:211`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/models/tool-call-model.ts#L211) | `export function toolRowModel(toolName: string, block: ToolCallBlock, cwd?: string): ToolRowModel {` |
| `body` | `const` | [`packages/core/tools/src/py-types.ts:812`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L812) | `const body = bodyLines.join('\n')` |
| `cwd` | `const` | [`packages/fs/tool-fs-search/src/search-core.ts:223`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/search-core.ts#L223) | `const cwd = exec.agent?.session.header.cwd` |
| `body` | `const` | [`packages/fs/tool-fs/src/read.ts:180`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts#L180) | `const body = /^<path>[^\n]*<\/path>\n<type>file<\/type>\n<content>\n([\s\S]*)\n<\/content>$/u.exec(text)?.[1]` |
| `cwd` | `const` | [`packages/fs/tool-fs/src/session-cwd.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts#L24) | `const cwd = exec.agent?.session.header.cwd` |
| `cwd` | `const` | [`packages/fs/tool-fs/src/session-cwd.ts:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts#L41) | `const cwd = policyWorkspaceRoot ?? sessionCwd(exec, requestedPath)` |
| `cwd` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2180`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2180) | `const cwd = workspace?.path ?? request.payload.cwd ?? defaults.cwd` |
| `cwd` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:3224`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L3224) | `const cwd = session.header.cwd` |
| `body` | `const` | [`packages/jobs/tool-jobs/src/index.ts:246`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L246) | `const body = value.text.length > 0 ? value.text : '(no new output)'` |
| `body` | `const` | [`packages/jobs/tool-jobs/src/index.ts:325`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L325) | `const body = value.text.length > 0 ? value.text : '(no new output)'` |

### Tests and executable evidence

- [`scripts/publint-all.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.spec.ts) — A test under the owning area exercises or imports `tsx`.
- [`scripts/coverage-exempt.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/coverage-exempt.spec.ts) — A test under the owning area exercises or imports `tsx`.
- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — A test under the owning area exercises or imports `tsx`.
- [`scripts/client-bundle-purity.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-purity.spec.ts) — A test under the owning area exercises or imports `tsx`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `tsx`.
- [`scripts/translation-pairing-merge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing-merge.spec.ts) — A test under the owning area exercises or imports `tsx`.
- [`packages/client/ui-tool/tests/tool-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/tool-row.client.spec.tsx) — A test under the owning area exercises or imports `toolRowModel`.
- Source verification intent: chat-view.spec.tsx pins the render-nothing grouping (including the interrupted exception); chat-tool-row.spec.tsx pins cwd relativization inside/outside the workspace and with an empty cwd; atoms.spec.tsx and workspace-picker.spec.tsx cover the menu and chip states; the full ui-conversation, ui-primitives, and ui-workspace suites pass.

## How to read the implementation

1. Start with [`packages/fs/tool-fs/src/read.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts) because it has the strongest evidence link to the note.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `domain/build-release`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/archived`, `mechanism/event-sourcing`
- Aliases: `body`, `toolRowModel`, `cwd`, `useLayoutEffect`, `ToolRowOwnerProps.cwd`, `mask-image`, `-webkit-font-smoothing`, `chat-view.spec.tsx`, `chat-tool-row.spec.tsx`, `atoms.spec.tsx`, `workspace-picker.spec.tsx`, `Web conversation UI polish sweep`, `bug fix`, `boundary`
- Regex: `(?i)(body|toolRowModel|useLayoutEffect|ToolRowOwnerProps\.cwd|mask\-image|\-webkit\-font\-smoothing|chat\-view\.spec\.tsx|chat\-tool\-row\.spec\.tsx)`

```bash
rg -n --pcre2 "(?i)(body|toolRowModel|useLayoutEffect|ToolRowOwnerProps\\.cwd|mask\\-image|\\-webkit\\-font\\-smoothing|chat\\-view\\.spec\\.tsx|chat\\-tool\\-row\\.spec\\.tsx)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0619. Tool-card single-row fields render inline](0619-tool-card-single-row-fields-render-inline.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/fs/tool-fs-search/src/search-core.ts`.
- **`shares-code-with`** — [0345. Workspace New Session reuse hijacked cwd-matching unaccounted blank sessions](0345-workspace-new-session-reuse-hijacked-cwd-matching-unaccounted-blank-sess.md): Shares source implementation: `packages/fs/tool-fs-search/src/search-core.ts`, `packages/fs/tool-fs/src/session-cwd.ts`.
- **`shares-code-with`** — [0348. `list_agents` uses `ready` for resumable children](0348-list-agents-uses-ready-for-resumable-children.md): Shares source implementation: `packages/api/gateway/src/index.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/fs/tool-fs/src/read.ts`.
- **`shares-code-with`** — [0130. Multiplex concurrent ACP sessions over one connection](0130-multiplex-concurrent-acp-sessions-over-one-connection.md): Shares source implementation: `packages/fs/tool-fs/src/session-cwd.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0237. opening a produced file from the web UI](0237-opening-a-produced-file-from-the-web-ui.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0320. The approval takeover shares the composer's text cap](0320-the-approval-takeover-shares-the-composer-s-text-cap.md): Shares source implementation: `scripts/coverage-exempt.spec.ts`, `scripts/publint-all.spec.ts`.
- **`shares-code-with`** — [0185. Workspace UI Complete Product Flow](0185-workspace-ui-complete-product-flow.md): Shares source implementation: `packages/fs/tool-fs/src/session-cwd.ts`, `packages/host/apiproxy/src/api-proxy.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0622-web-conversation-ui-polish-sweep.md`.
