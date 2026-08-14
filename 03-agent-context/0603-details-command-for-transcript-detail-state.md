---
id: "dsh-note-0603"
title: "/details command for transcript detail state"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-30-tui-details-command.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/lifecycle"
  - "concern/performance"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/registry"
aliases:
  - "hidden"
  - "expanded"
  - "collapsed"
  - "settle"
  - "renderEvent"
  - "dsh-tui"
  - "/details"
  - "DetailsDialog"
  - "detailsDialogWidth"
  - "/model"
  - "setToolsVisibility"
  - "setReasoning"
  - "StreamingAssistantComponent"
  - "assistant/message"
search_regex: "(?i)(hidden|expanded|collapsed|settle|renderEvent|dsh\\-tui|/details|DetailsDialog)"
---

# 0603. /details command for transcript detail state — implementation context

## Open this when

The TUI's transcript detail state --- tool-card visibility (collapsed/expanded/hidden, per the consolidated TUI presentation) and reasoning-block display --- was reachable only through the Ctrl+O cycle and the Ctrl+R toggle. A user who wants a specific mode must cycle through the others, cannot set both dimensions in one action, and has no way to query the current state; a terminal that swallows those control keys has no fallback at all.

## Source decision

A combined invocation applies reasoning before visibility because setReasoning rebuilds the transcript from session events, which drops non-durable notice components; applying it last would erase the just-appended visibility notice. The reasoning rebuild exposed a replay defect that this change fixes in renderEvent: the live path cleared a settled StreamingAssistantComponent before a later assistant/message of the same step (so the second message got a fresh component), but rebuildTranscript replay reused the settled component and settle() overwrote its content, silently dropping the earlier message's text.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-30-tui-details-command.md](../02-notes/archived/feature/2026-07-30-tui-details-command.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-30-tui-details-command.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-30-tui-details-command.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `collapsed`, a construct named by the note. | `symbol-definition` |
| [`scripts/gen-persistence-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-persistence-catalog.ts) | repository automation | Defines `renderEvent`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/client/src/dispose.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/dispose.ts) | runtime implementation | Defines `settle`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/ts-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/ts-types.ts) | runtime implementation | Defines `collapsed`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Defines `collapsed`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts) | package entry point | Defines `settle`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/code-mode.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts) | runtime implementation | Defines `settle`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `settle`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-workspace/src/client/tree.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/tree.ts) | runtime implementation | Defines `expanded`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/DiffBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DiffBlock.tsx) | runtime implementation | Defines `hidden`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/ReadBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ReadBlock.tsx) | runtime implementation | Defines `hidden`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts) | runtime implementation | Defines `renderEvent`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `hidden` | `const` | [`packages/client/ui-conversation/src/client/chat/ContextBody.tsx:94`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ContextBody.tsx#L94) | `const hidden = formRendered ? ['kind', 'form'] : ['kind']` |
| `expanded` | `const` | [`packages/client/ui-conversation/src/client/queue/QueueDock.tsx:49`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/queue/QueueDock.tsx#L49) | `const expanded = !collapsed \|\| interactionActive` |
| `hidden` | `const` | [`packages/client/ui-deliverables/src/client/ProducedFiles.tsx:114`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-deliverables/src/client/ProducedFiles.tsx#L114) | `const hidden = paths.length - shown.length` |
| `hidden` | `const` | [`packages/client/ui-primitives/src/DiffBlock.tsx:159`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DiffBlock.tsx#L159) | `const hidden = rows.length - maxLines` |
| `hidden` | `const` | [`packages/client/ui-primitives/src/ReadBlock.tsx:107`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ReadBlock.tsx#L107) | `const hidden = lines.length - maxLines` |
| `hidden` | `const` | [`packages/client/ui-primitives/src/head-tail-cap.ts:29`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/head-tail-cap.ts#L29) | `const hidden = total - maxLines` |
| `collapsed` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryView.tsx:402`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryView.tsx#L402) | `const collapsed = new Set(current)` |
| `expanded` | `const` | [`packages/client/ui-workspace/src/client/tree.ts:259`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/tree.ts#L259) | `const expanded = expandedGroups.has(g.key)` |
| `settle` | `const` | [`packages/core/tools/src/code-mode.ts:487`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L487) | `const settle = (result: ToolExecutionResult): void => {` |
| `collapsed` | `const` | [`packages/core/tools/src/index.ts:1381`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1381) | `const collapsed = visible !== undefined && this.collapses(name, agent, parent !== undefined)` |
| `collapsed` | `const` | [`packages/core/tools/src/py-types.ts:226`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L226) | `const collapsed = description` |
| `collapsed` | `const` | [`packages/core/tools/src/py-types.ts:242`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L242) | `const collapsed = describe({ description })` |
| `collapsed` | `const` | [`packages/core/tools/src/ts-types.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/ts-types.ts#L36) | `const collapsed = description.replace(/\s+/g, ' ').trim()` |
| `settle` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1460`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1460) | `const settle = (outcome: ApprovalOutcome): void => {` |
| `settle` | `const` | [`packages/sdk/client/src/dispose.ts:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/dispose.ts#L45) | `const settle = (complete: () => void): void => {` |
| `settle` | `const` | [`packages/subprocess/subprocess-local/src/spawn.ts:472`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts#L472) | `const settle = (exitCode: number \| null, signal: NodeJS.Signals \| null): void => {` |

### Tests and executable evidence

- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `dsh-tui`. Contains the exact code literal `dsh-tui` named by the note.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `collapsed`.
- [`packages/client/runtime/tests/conversation.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/conversation.client.spec.ts) — A test under the owning area exercises or imports `Assistant`.
- [`packages/client/ui-deliverables/tests/produced-files.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-deliverables/tests/produced-files.client.spec.tsx) — A test under the owning area exercises or imports `Assistant`.
- [`packages/client/ui-conversation/tests/conversation-node-definitions.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/conversation-node-definitions.client.spec.ts) — A test under the owning area exercises or imports `Assistant`.

## How to read the implementation

1. Start with [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/lifecycle`, `concern/performance`, `concern/recovery`, `domain/build-release`, `domain/configuration`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/registry`
- Aliases: `hidden`, `expanded`, `collapsed`, `settle`, `renderEvent`, `dsh-tui`, `/details`, `DetailsDialog`, `detailsDialogWidth`, `/model`, `setToolsVisibility`, `setReasoning`, `StreamingAssistantComponent`, `assistant/message`
- Regex: `(?i)(hidden|expanded|collapsed|settle|renderEvent|dsh\-tui|/details|DetailsDialog)`

```bash
rg -n --pcre2 "(?i)(hidden|expanded|collapsed|settle|renderEvent|dsh\\-tui|/details|DetailsDialog)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0555. Consolidated TUI presentation and navigation](0555-consolidated-tui-presentation-and-navigation.md): The source note links to this decision directly.
- **`shares-code-with`** — [0165. Typed tool returns in Code Mode](0165-typed-tool-returns-in-code-mode.md): Shares source implementation: `packages/core/tools/src/code-mode.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0228. Code Mode language dispatch and the Python SDK renderer](0228-code-mode-language-dispatch-and-the-python-sdk-renderer.md): Shares source implementation: `packages/core/tools/src/code-mode.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0189. Code Mode live dispatch lifecycle and native-contract parallelism](0189-code-mode-live-dispatch-lifecycle-and-native-contract-parallelism.md): Shares source implementation: `packages/core/tools/src/code-mode.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0138. dsh-hooks-claude-code + dsh-hooks-codex --- the Claude Code / Codex hook bridges](0138-dsh-hooks-claude-code-dsh-hooks-codex-the-claude-code-codex-hook-bridges.md): Shares source implementation: `packages/core/tools/src/code-mode.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0142. Dynamic workflows --- a script-driven multi-agent orchestration seam](0142-dynamic-workflows-a-script-driven-multi-agent-orchestration-seam.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/py-types.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/py-types.ts`.
- **`shares-code-with`** — [0159. Fresh-agent Ralph workflow tool](0159-fresh-agent-ralph-workflow-tool.md): Shares source implementation: `packages/core/tools/src/code-mode.ts`, `packages/core/tools/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0603-details-command-for-transcript-detail-state.md`.
