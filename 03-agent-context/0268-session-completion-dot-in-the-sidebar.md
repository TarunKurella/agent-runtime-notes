---
id: "dsh-note-0268"
title: "Session completion dot in the sidebar"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-06-session-completed-done-dot.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/performance"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
aliases:
  - "completed"
  - "done"
  - "SessionListEntry"
  - "SessionManager"
  - "SessionSummary"
  - "select"
  - "StateDot"
  - "selectSubagent"
  - "SessionSummary.completed"
  - "Session completion dot in the sidebar"
  - "feature"
  - "discovery routing"
  - "evidence"
  - "human control"
search_regex: "(?i)(completed|done|SessionListEntry|SessionManager|SessionSummary|select|StateDot|selectSubagent)"
---

# 0268. Session completion dot in the sidebar — implementation context

## Open this when

A session the operator delegated work to and then left (switched to another conversation) gives no signal when it finishes. Its running indicator stops, but the row then looks identical to any idle session, so the operator must poll the list or discover the finished work late. The pending-interaction amber dot covers sessions that need input, not sessions whose work is simply done.

## Source decision

SessionManager owns a client-side completion-reminder set, a sibling of the pending-interaction bit: a running→idle edge of a session that is not the selected one arms its reminder; select()/selectSubagent() consume it; starting a new run disarms it and its completion re-arms it; removal prunes it. The bit rides SessionListEntry → SessionSummary (optional, absent = no reminder) into the workspace browser, whose session and search rows render the existing StateDot done state --- running keeps the ongoing spinner, an idle session without a reminder shows nothing --- and whose hover card labels the reminder 已完成 /.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-06-session-completed-done-dot.md](../02-notes/implemented/feature/2026-08-06-session-completed-done-dot.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-06-session-completed-done-dot.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-06-session-completed-done-dot.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `completed`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/src/process-shutdown.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/process-shutdown.ts) | runtime implementation | Defines `completed`, a construct named by the note. | `symbol-definition` |
| [`packages/e2b/fs-e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts) | package entry point | Defines `completed`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent-loop/src/agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts) | runtime implementation | Defines `done`, a construct named by the note. | `symbol-definition` |
| [`packages/lsp/lsp-stdio/src/connection.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/connection.ts) | runtime implementation | Defines `done`, a construct named by the note. | `symbol-definition` |
| [`packages/e2b/subprocess-e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/index.ts) | package entry point | Defines `done`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/analyzer.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts) | runtime implementation | Defines `completed`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api/sessions.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/sessions.ts) | runtime implementation | Defines `SessionSummary`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/http-bridge.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/http-bridge.ts) | runtime implementation | Defines `done`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/StateDot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/StateDot.tsx) | runtime implementation | Defines `StateDot`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `done`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/sessions/manager.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts) | runtime implementation | Defines `SessionManager`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `completed` | `let` | [`apps/cli/src/process-shutdown.ts:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/process-shutdown.ts#L30) | `let completed = false` |
| `done` | `const` | [`packages/client/connection/src/client/fixture.ts:2150`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L2150) | `const done = pieces.slice(0, i).join('')` |
| `done` | `const` | [`packages/client/connection/src/http-bridge.ts:88`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/http-bridge.ts#L88) | `const done = (): void => {` |
| `SessionListEntry` | `interface` | [`packages/client/runtime/src/client/sessions/lineage.ts:17`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/lineage.ts#L17) | `export interface SessionListEntry {` |
| `SessionManager` | `class` | [`packages/client/runtime/src/client/sessions/manager.ts:106`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts#L106) | `export class SessionManager {` |
| `SessionSummary` | `interface` | [`packages/client/runtime/src/client/sessions/service.ts:42`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/service.ts#L42) | `export interface SessionSummary {` |
| `select` | `const` | [`packages/client/ui-directory-picker-browse/src/client/DirectoryBrowser.tsx:496`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-directory-picker-browse/src/client/DirectoryBrowser.tsx#L496) | `const select = useCallback((entry: DirectoryEntry) => {` |
| `select` | `const` | [`packages/client/ui-permission-presets/src/client/index.ts:118`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-permission-presets/src/client/index.ts#L118) | `const select = (preset: string): Promise<void> => controller.select(preset)` |
| `StateDot` | `function` | [`packages/client/ui-primitives/src/StateDot.tsx:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/StateDot.tsx#L24) | `export function StateDot({ state, size = 10, className }: {` |
| `done` | `const` | [`packages/core/agent-loop/src/agent.ts:144`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L144) | `const done = Promise.withResolvers<void>()` |
| `completed` | `let` | [`packages/e2b/fs-e2b/src/index.ts:258`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L258) | `let completed = false` |
| `completed` | `let` | [`packages/e2b/fs-e2b/src/index.ts:305`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L305) | `let completed = false` |
| `done` | `const` | [`packages/e2b/subprocess-e2b/src/index.ts:173`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/index.ts#L173) | `const done = Promise.withResolvers<void>()` |
| `SessionSummary` | `interface` | [`packages/host/apiproxy/src/api/sessions.ts:177`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/sessions.ts#L177) | `export interface SessionSummary {` |
| `completed` | `let` | [`packages/llm/llm/src/index.ts:872`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L872) | `let completed = false` |
| `done` | `const` | [`packages/lsp/lsp-stdio/src/connection.ts:285`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/connection.ts#L285) | `const done = (error?: Error \| null): void => {` |

### Tests and executable evidence

- [`packages/client/runtime/tests/manager.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/manager.client.spec.ts) — A test under the owning area exercises or imports `SessionManager`. A test under the owning area exercises or imports `selectSubagent`.
- [`packages/client/runtime/tests/lineage.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/lineage.client.spec.ts) — A test under the owning area exercises or imports `SessionSummary`.
- [`packages/client/runtime/tests/queue-store.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/queue-store.client.spec.ts) — A test under the owning area exercises or imports `SessionManager`.
- [`packages/client/runtime/tests/projection-store.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/projection-store.client.spec.ts) — A test under the owning area exercises or imports `SessionManager`.
- [`packages/client/runtime/tests/subagent-lineage.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/subagent-lineage.client.spec.ts) — A test under the owning area exercises or imports `SessionSummary`.
- [`packages/client/ui-primitives/tests/state-dot.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/state-dot.client.spec.tsx) — A test under the owning area exercises or imports `StateDot`.
- [`packages/client/ui-primitives/tests/terminal-block.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/terminal-block.client.spec.tsx) — A test under the owning area exercises or imports `StateDot`.

## How to read the implementation

1. Start with [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) because it has the strongest evidence link to the note.
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

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/performance`, `concern/simplification`, `domain/build-release`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`
- Aliases: `completed`, `done`, `SessionListEntry`, `SessionManager`, `SessionSummary`, `select`, `StateDot`, `selectSubagent`, `SessionSummary.completed`, `Session completion dot in the sidebar`, `feature`, `discovery routing`, `evidence`, `human control`
- Regex: `(?i)(completed|done|SessionListEntry|SessionManager|SessionSummary|select|StateDot|selectSubagent)`

```bash
rg -n --pcre2 "(?i)(completed|done|SessionListEntry|SessionManager|SessionSummary|select|StateDot|selectSubagent)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0364. Owned-run finish reason reporting](0364-owned-run-finish-reason-reporting.md): Shares source implementation: `apps/cli/src/process-shutdown.ts`, `packages/e2b/fs-e2b/src/index.ts`.
- **`shares-code-with`** — [0651. Drop unconsumed assembled LLM convenience surfaces](0651-drop-unconsumed-assembled-llm-convenience-surfaces.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/e2b/fs-e2b/src/index.ts`.
- **`shares-code-with`** — [0554. dsh-tui chat channel module split](0554-dsh-tui-chat-channel-module-split.md): Shares source implementation: `packages/e2b/fs-e2b/src/index.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0616. TUI presents a reason for every turn-end kind](0616-tui-presents-a-reason-for-every-turn-end-kind.md): Shares source implementation: `apps/cli/src/process-shutdown.ts`, `packages/e2b/fs-e2b/src/index.ts`.
- **`shares-code-with`** — [0072. Portable consumers over filesystem and subprocess execution worlds](0072-portable-consumers-over-filesystem-and-subprocess-execution-worlds.md): Shares source implementation: `packages/e2b/fs-e2b/src/index.ts`, `packages/e2b/subprocess-e2b/src/index.ts`.
- **`shares-code-with`** — [0302. Render error cause chains at every diagnostic boundary](0302-render-error-cause-chains-at-every-diagnostic-boundary.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0587. TUI prompt themes compose mutable plugin values](0587-tui-prompt-themes-compose-mutable-plugin-values.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0126. Repository naming contract and pre-release rename ledger](0126-repository-naming-contract-and-pre-release-rename-ledger.md): Shares source implementation: `packages/llm/llm/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0268-session-completion-dot-in-the-sidebar.md`.
