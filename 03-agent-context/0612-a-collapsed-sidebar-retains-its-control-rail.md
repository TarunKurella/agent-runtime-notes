---
id: "dsh-note-0612"
title: "A collapsed sidebar retains its control rail"
status: "archived"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/bug-fix/2026-07-22-collapsed-sidebar-control-rail.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/configuration"
  - "domain/filesystem"
  - "domain/session-state"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
aliases:
  - "left"
  - "AppFrame"
  - "SidebarRoot"
  - "collapsed"
  - "grid-template-columns"
  - "--ds-ease-in-out"
  - "--ds-transition-duration-slow"
  - "prefers-reduced-motion"
  - "A collapsed sidebar retains its control rail"
  - "bug fix"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "recovery"
search_regex: "(?i)(left|AppFrame|SidebarRoot|collapsed|grid\\-template\\-columns|\\-\\-ds\\-ease\\-in\\-out|\\-\\-ds\\-transition\\-duration\\-slow|prefers\\-reduced\\-motion)"
---

# 0612. A collapsed sidebar retains its control rail — implementation context

## Open this when

The sidebar close action persisted a zero width preference, and the layout mapped that preference to a zero-width grid track. The only sidebar toggle and the settings entry both lived inside that clipped track, so closing the sidebar removed every visible recovery control. Reloading preserved the closed preference and reproduced the lockout.

## Source decision

The layout maps a closed sidebar (persisted width 0) to the fixed SIDEBAR_COLLAPSED width of 56px: a 24px icon column between the sidebar's 16px horizontal paddings. The sidebar track is fixed-width in the solver --- open or collapsed it never concedes to viewport pressure (only details shrinks, then auto-closes) --- and the rail retains its right border while the stored expanded width remains untouched.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/bug-fix/2026-07-22-collapsed-sidebar-control-rail.md](../02-notes/archived/bug-fix/2026-07-22-collapsed-sidebar-control-rail.md)
- Pinned source: [.agents/notes/archived/bug-fix/2026-07-22-collapsed-sidebar-control-rail.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/bug-fix/2026-07-22-collapsed-sidebar-control-rail.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `collapsed`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/ts-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/ts-types.ts) | runtime implementation | Defines `collapsed`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Defines `collapsed`, a construct named by the note. | `symbol-definition` |
| [`packages/settings/settings/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts) | package entry point | Defines `left`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-layout/src/client/AppFrame.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/AppFrame.tsx) | runtime implementation | Defines `AppFrame`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-attachment/src/AttachmentRail.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/AttachmentRail.tsx) | runtime implementation | Defines `left`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-sidebar/src/client/SidebarRoot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-sidebar/src/client/SidebarRoot.tsx) | runtime implementation | Defines `SidebarRoot`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/TrajectoryView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryView.tsx) | runtime implementation | Defines `collapsed`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/ChatView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx) | runtime implementation | Defines `left`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx) | runtime implementation | Defines `left`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/sessions/conversation-assembler.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation-assembler.ts) | runtime implementation | Defines `left`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `left` | `const` | [`packages/client/runtime/src/client/sessions/conversation-assembler.ts:103`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation-assembler.ts#L103) | `const left = additions[added]` |
| `left` | `const` | [`packages/client/ui-attachment/src/AttachmentRail.tsx:84`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/AttachmentRail.tsx#L84) | `const left = el.scrollLeft > 1` |
| `left` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L62) | `const left = Math.max(viewport.left, content.left)` |
| `AppFrame` | `function` | [`packages/client/ui-layout/src/client/AppFrame.tsx:87`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/AppFrame.tsx#L87) | `export function AppFrame({` |
| `SidebarRoot` | `function` | [`packages/client/ui-sidebar/src/client/SidebarRoot.tsx:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-sidebar/src/client/SidebarRoot.tsx#L44) | `export function SidebarRoot({` |
| `left` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx:676`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx#L676) | `const left = (span.start - model.start) / fullDuration` |
| `collapsed` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryView.tsx:402`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryView.tsx#L402) | `const collapsed = new Set(current)` |
| `collapsed` | `const` | [`packages/core/tools/src/index.ts:1381`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1381) | `const collapsed = visible !== undefined && this.collapses(name, agent, parent !== undefined)` |
| `collapsed` | `const` | [`packages/core/tools/src/py-types.ts:226`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L226) | `const collapsed = description` |
| `collapsed` | `const` | [`packages/core/tools/src/py-types.ts:242`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L242) | `const collapsed = describe({ description })` |
| `collapsed` | `const` | [`packages/core/tools/src/ts-types.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/ts-types.ts#L36) | `const collapsed = description.replace(/\s+/g, ' ').trim()` |
| `left` | `const` | [`packages/settings/settings/src/index.ts:152`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts#L152) | `const left = a as Record<string, unknown>` |

### Tests and executable evidence

- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — A test under the owning area exercises or imports `grid-template-columns`.
- [`apps/web/tests/code-mode-round.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/code-mode-round.e2e.ts) — A test under the owning area exercises or imports `grid-template-columns`.
- [`apps/web/tests/navigation-panes.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/navigation-panes.e2e.ts) — A test under the owning area exercises or imports `grid-template-columns`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `collapsed`.
- [`apps/web/tests/details-session-lifecycle.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/details-session-lifecycle.e2e.ts) — A test under the owning area exercises or imports `grid-template-columns`.
- [`packages/client/ui-layout/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `AppFrame`.
- [`packages/client/ui-layout/tests/app-frame.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/tests/app-frame.client.spec.tsx) — A test under the owning area exercises or imports `AppFrame`.
- [`packages/client/ui-sidebar/tests/sidebar-root.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-sidebar/tests/sidebar-root.client.spec.tsx) — A test under the owning area exercises or imports `SidebarRoot`.

## How to read the implementation

1. Start with [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/configuration`, `domain/filesystem`, `domain/session-state`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`
- Aliases: `left`, `AppFrame`, `SidebarRoot`, `collapsed`, `grid-template-columns`, `--ds-ease-in-out`, `--ds-transition-duration-slow`, `prefers-reduced-motion`, `A collapsed sidebar retains its control rail`, `bug fix`, `evidence`, `lifecycle`, `ownership`, `recovery`
- Regex: `(?i)(left|AppFrame|SidebarRoot|collapsed|grid\-template\-columns|\-\-ds\-ease\-in\-out|\-\-ds\-transition\-duration\-slow|prefers\-reduced\-motion)`

```bash
rg -n --pcre2 "(?i)(left|AppFrame|SidebarRoot|collapsed|grid\\-template\\-columns|\\-\\-ds\\-ease\\-in\\-out|\\-\\-ds\\-transition\\-duration\\-slow|prefers\\-reduced\\-motion)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0228. Code Mode language dispatch and the Python SDK renderer](0228-code-mode-language-dispatch-and-the-python-sdk-renderer.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/py-types.ts`.
- **`shares-code-with`** — [0189. Code Mode live dispatch lifecycle and native-contract parallelism](0189-code-mode-live-dispatch-lifecycle-and-native-contract-parallelism.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/py-types.ts`.
- **`shares-code-with`** — [0165. Typed tool returns in Code Mode](0165-typed-tool-returns-in-code-mode.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/py-types.ts`.
- **`shares-code-with`** — [0340. The conversation column reserves one scrollbar gutter for every view](0340-the-conversation-column-reserves-one-scrollbar-gutter-for-every-view.md): Shares source implementation: `packages/client/ui-attachment/src/AttachmentRail.tsx`, `packages/client/ui-conversation/src/client/chat/ChatView.tsx`.
- **`shares-code-with`** — [0603. /details command for transcript detail state](0603-details-command-for-transcript-detail-state.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/py-types.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/py-types.ts`.
- **`shares-code-with`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/py-types.ts`.
- **`shares-code-with`** — [0032. Tool output spill policy](0032-tool-output-spill-policy.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/ts-types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0612-a-collapsed-sidebar-retains-its-control-rail.md`.
