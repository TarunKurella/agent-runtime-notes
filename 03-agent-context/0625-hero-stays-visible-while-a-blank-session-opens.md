---
id: "dsh-note-0625"
title: "Hero stays visible while a blank session opens"
status: "archived"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/bug-fix/2026-07-31-hero-visible-while-blank-session-opens.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "domain/filesystem"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/archived"
  - "mechanism/policy"
aliases:
  - "openState"
  - "ConversationRoot"
  - "summaryBlank"
  - "settling"
  - "hero"
  - "blank"
  - "ConversationSession"
  - "select"
  - "cold"
  - "history"
  - "loading"
  - "WorkspacesService.startInitialSelection"
  - "startInitialSelection"
  - "summaryBlank !== true"
search_regex: "(?i)(openState|ConversationRoot|summaryBlank|settling|hero|blank|ConversationSession|select)"
---

# 0625. Hero stays visible while a blank session opens — implementation context

## Open this when

The conversation root has a settling phase for a session that is still opening while its composer reads blank: the hero-versus-docked outcome is unknowable until history arrives, so the composer seat is hidden (visibility:hidden) rather than flashing the centered hero and snapping to the docked bar. Startup auto-selection turned that guard into the defect it was meant to prevent.

## Source decision

ConversationRoot reads the session list summary's blank flag alongside the conversation snapshot and exempts summary-proven blank sessions from settling: settling additionally requires summaryBlank !== true, and hero accepts a blank composer whenever the summary proves the session blank, in every open state rather than only loading. A session the list already reports as blank can only land on the hero, so hiding buys nothing and costs the visible flash; the same proof holds before the open starts (cold) and after one fails (error), where the previous conditions fell through to the active phase and rendered.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/bug-fix/2026-07-31-hero-visible-while-blank-session-opens.md](../02-notes/archived/bug-fix/2026-07-31-hero-visible-while-blank-session-opens.md)
- Pinned source: [.agents/notes/archived/bug-fix/2026-07-31-hero-visible-while-blank-session-opens.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/bug-fix/2026-07-31-hero-visible-while-blank-session-opens.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/typert/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts) | package entry point | Defines `loading`, a construct named by the note. | `symbol-definition` |
| [`packages/skill/tool-skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts) | package entry point | Defines `history`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `blank`, a construct named by the note. Defines `cold`, a construct named by the note. | `symbol-definition` |
| [`packages/mcp/mcp-client/src/connection.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/connection.ts) | runtime implementation | Defines `settling`, a construct named by the note. | `symbol-definition` |
| [`packages/subagent/subagent/src/continuation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/continuation.ts) | runtime implementation | Defines `settling`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-permission-presets/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-permission-presets/src/client/index.ts) | package entry point | Defines `select`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/ChatView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx) | runtime implementation | Defines `openState`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx) | runtime implementation | Defines `settling`, a construct named by the note. Defines `openState`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-directory-picker-browse/src/client/DirectoryBrowser.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-directory-picker-browse/src/client/DirectoryBrowser.tsx) | runtime implementation | Defines `select`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx) | runtime implementation | Defines `blank`, a construct named by the note. Defines `ConversationSession`, a construct named by the note. | `symbol-definition` |
| [`tsconfig.host.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.host.json) | composition and configuration | Contains the exact code literal `apps/web/tests/startup-auto-selection.e2e.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `openState` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:157`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L157) | `const openState = useSession(s => s.openState)` |
| `ConversationRoot` | `function` | [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx#L15) | `export function ConversationRoot({` |
| `openState` | `const` | [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx:19`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx#L19) | `const openState = useSession(s => s.openState)` |
| `summaryBlank` | `const` | [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx:25`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx#L25) | `const summaryBlank = useSessions(s => sessionId === undefined ? undefined : s.byId[sessionId]?.blank)` |
| `settling` | `const` | [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx:77`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx#L77) | `const settling = sessionId !== undefined && composerPhase === 'blank' && openState === 'loading'` |
| `hero` | `const` | [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx:79`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx#L79) | `const hero = sessionId === undefined` |
| `blank` | `const` | [`packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx#L71) | `const blank = useSession(s => s.blank)` |
| `ConversationSession` | `function` | [`packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx:138`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx#L138) | `export function ConversationSession({` |
| `blank` | `const` | [`packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx:147`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx#L147) | `const blank = useSession(s => s.blank)` |
| `select` | `const` | [`packages/client/ui-directory-picker-browse/src/client/DirectoryBrowser.tsx:496`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-directory-picker-browse/src/client/DirectoryBrowser.tsx#L496) | `const select = useCallback((entry: DirectoryEntry) => {` |
| `select` | `const` | [`packages/client/ui-permission-presets/src/client/index.ts:118`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-permission-presets/src/client/index.ts#L118) | `const select = (preset: string): Promise<void> => controller.select(preset)` |
| `blank` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:513`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L513) | `const blank = state.blank && event.type !== 'turn/start'` |
| `cold` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1740`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1740) | `const cold = (await persistence.list(signal))` |
| `settling` | `let` | [`packages/mcp/mcp-client/src/connection.ts:308`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/connection.ts#L308) | `let settling = connectGeneration(true)` |
| `history` | `const` | [`packages/skill/tool-skill/src/index.ts:229`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts#L229) | `const history = catalogHistory(agent)` |
| `settling` | `const` | [`packages/subagent/subagent/src/continuation.ts:1244`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/continuation.ts#L1244) | `const settling = await this.locks.run<SettlementAttempt>(activation.childId, () => {` |

### Tests and executable evidence

- [`apps/web/tests/startup-auto-selection.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/startup-auto-selection.e2e.ts) — The source note names this file directly. A test under the owning area exercises or imports `startInitialSelection`.
- [`packages/client/runtime/tests/notifier.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/notifier.client.spec.ts) — A test under the owning area exercises or imports `notifyNow`.
- [`packages/client/ui-conversation/tests/skeleton.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/skeleton.client.spec.tsx) — A test under the owning area exercises or imports `settling`. A test under the owning area exercises or imports `openState`.
- [`packages/client/runtime/tests/workspaces-service.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/workspaces-service.client.spec.ts) — A test under the owning area exercises or imports `startInitialSelection`.
- [`packages/client/ui-conversation/tests/chat-view.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-view.client.spec.tsx) — A test under the owning area exercises or imports `openState`.
- [`packages/client/ui-conversation/tests/input-bar.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-bar.client.spec.tsx) — A test under the owning area exercises or imports `openState`. A test under the owning area exercises or imports `hero`.
- [`packages/client/ui-conversation/tests/queue-dock.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/queue-dock.client.spec.tsx) — A test under the owning area exercises or imports `openState`.
- [`packages/client/ui-conversation/tests/chat-stats.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-stats.client.spec.tsx) — A test under the owning area exercises or imports `openState`.

## How to read the implementation

1. Start with [`packages/typert/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `domain/filesystem`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/archived`, `mechanism/policy`
- Aliases: `openState`, `ConversationRoot`, `summaryBlank`, `settling`, `hero`, `blank`, `ConversationSession`, `select`, `cold`, `history`, `loading`, `WorkspacesService.startInitialSelection`, `startInitialSelection`, `summaryBlank !== true`
- Regex: `(?i)(openState|ConversationRoot|summaryBlank|settling|hero|blank|ConversationSession|select)`

```bash
rg -n --pcre2 "(?i)(openState|ConversationRoot|summaryBlank|settling|hero|blank|ConversationSession|select)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0348. `list_agents` uses `ready` for resumable children](0348-list-agents-uses-ready-for-resumable-children.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/mcp/mcp-client/src/connection.ts`.
- **`shares-code-with`** — [0275. The no-Workspace composer opens the existing picker](0275-the-no-workspace-composer-opens-the-existing-picker.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0318. Fixed header, sticky composer inside the transcript scrollport](0318-fixed-header-sticky-composer-inside-the-transcript-scrollport.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/ChatView.tsx`, `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`.
- **`shares-code-with`** — [0288. Web session-log export as a host-streamed ZIP download](0288-web-session-log-export-as-a-host-streamed-zip-download.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/typert/loader/src/index.ts`.
- **`shares-code-with`** — [0239. Goal command input projection](0239-goal-command-input-projection.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0204. Independent model and user skill invocation policy](0204-independent-model-and-user-skill-invocation-policy.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/skill/tool-skill/src/index.ts`.
- **`shares-code-with`** — [0194. Trajectory inspection ledger](0194-trajectory-inspection-ledger.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0085. the end-seed log boundary](0085-the-end-seed-log-boundary.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/skill/tool-skill/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0625-hero-stays-visible-while-a-blank-session-opens.md`.
