---
id: "dsh-note-0127"
title: "Trajectory assembly from registered Conversation Contexts"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-11-trajectory-conversation-context-assembly.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/context"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "ConversationNodeAssembler"
  - "views"
  - "rootCallId"
  - "TrajectorySearchIndex"
  - "eventNodes"
  - "target"
  - "user/message"
  - "next-step"
  - "Session.views"
  - "step/start"
  - "step/end"
  - "agent/inbox/spliced"
  - "be the loaded raw Event count,"
  - "the number of Trajectory Definitions,"
search_regex: "(?i)(ConversationNodeAssembler|views|rootCallId|TrajectorySearchIndex|eventNodes|target|user/message|next\\-step)"
---

# 0127. Trajectory assembly from registered Conversation Contexts — implementation context

## Open this when

Trajectory maintained an independent Session History source and folded the complete loaded Event window into Assistant, Tool, message, Request-header, and Compaction state. Chat already assembled the same Event families through registered Conversation Definitions. The two paths duplicated business correlation and pagination behavior, and a Trajectory structural update copied or rescanned work proportional to the raw Event count even when one business object changed. Reusing Chat's final Nodes would not solve the ownership problem.

## Source decision

Trajectory registers target-owned Conversation Definitions and a trajectory View Builder against the shared ConversationNodeAssembler. Session owns one contiguous Event window and publishes both Chat and Trajectory snapshots through Session.views; it does not run a second Trajectory history source or business fold. Each Definition belongs to one target. Chat and Trajectory may recognize the same durable Event family, but they keep separate State and final Node payloads.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-11-trajectory-conversation-context-assembly.md](../02-notes/implemented/architecture/2026-08-11-trajectory-conversation-context-assembly.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-11-trajectory-conversation-context-assembly.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-11-trajectory-conversation-context-assembly.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `target`, a construct named by the note. | `symbol-definition` |
| [`vendor/include/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts) | package entry point | Defines `target`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/src/edit.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/edit.ts) | runtime implementation | Defines `target`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `rootCallId`, a construct named by the note. | `symbol-definition` |
| [`vendor/loader/src/config/tree.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/tree.ts) | runtime implementation | Defines `target`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `views`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/apply.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts) | runtime implementation | Defines `views`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/trajectory-search-index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/trajectory-search-index.ts) | runtime implementation | Defines `TrajectorySearchIndex`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/sessions/conversation-assembler.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation-assembler.ts) | runtime implementation | Defines `ConversationNodeAssembler`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/conversation-nodes/tool.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/tool.ts) | runtime implementation | Defines `rootCallId`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/trajectory-tool-definition.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/trajectory-tool-definition.ts) | runtime implementation | Defines `rootCallId`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/trajectory-snapshot-builder.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/trajectory-snapshot-builder.ts) | runtime implementation | Defines `eventNodes`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ConversationNodeAssembler` | `class` | [`packages/client/runtime/src/client/sessions/conversation-assembler.ts:137`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation-assembler.ts#L137) | `export class ConversationNodeAssembler implements ConversationViewSnapshotStore {` |
| `views` | `const` | [`packages/client/ui-conversation/src/client/apply.ts:162`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts#L162) | `const views = {` |
| `rootCallId` | `const` | [`packages/client/ui-conversation/src/client/conversation-nodes/tool.ts:245`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/tool.ts#L245) | `const rootCallId: unknown = event.data.rootCallId` |
| `TrajectorySearchIndex` | `class` | [`packages/client/ui-trajectory/src/client/trajectory-search-index.ts:76`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/trajectory-search-index.ts#L76) | `export class TrajectorySearchIndex {` |
| `eventNodes` | `const` | [`packages/client/ui-trajectory/src/client/trajectory-snapshot-builder.ts:247`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/trajectory-snapshot-builder.ts#L247) | `const eventNodes = finalized` |
| `rootCallId` | `const` | [`packages/client/ui-trajectory/src/client/trajectory-tool-definition.ts:228`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/trajectory-tool-definition.ts#L228) | `const rootCallId: unknown = event.data.rootCallId` |
| `rootCallId` | `const` | [`packages/core/tools/src/index.ts:1368`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1368) | `const rootCallId = exec.rootCallId ?? callId` |
| `target` | `const` | [`packages/fs/tool-fs/src/edit.ts:117`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/edit.ts#L117) | `const target = await ctx.fs.resolve(input.filePath, sessionResolveOptions(exec, input.filePath, sandboxPolicy?.workspaceRoot))` |
| `views` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:3376`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L3376) | `const views: ConfigurableProviderView[] = directory.map(entry => ({` |
| `views` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:3464`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L3464) | `const views = jobViews(jobs.list(ctx.agents.get(session.id)))` |
| `views` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:3501`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L3501) | `const views = jobs === undefined ? [] : jobViews(jobs.list(ctx.agents.get(session.id)))` |
| `target` | `const` | [`vendor/hmr/src/index.ts:137`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L137) | `const target = await findWatchRoot(filename)` |
| `target` | `const` | [`vendor/include/src/index.ts:82`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts#L82) | `const target = entryMap.get(id)` |
| `target` | `const` | [`vendor/include/src/index.ts:110`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts#L110) | `const target = entryMap.get(id)` |
| `target` | `let` | [`vendor/loader/src/config/tree.ts:118`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/tree.ts#L118) | `let target = source` |

### Tests and executable evidence

- [`apps/web/tests/steering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/steering.e2e.ts) — A test under the owning area exercises or imports `next-step`.
- [`apps/web/tests/navigation-panes.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/navigation-panes.e2e.ts) — A test under the owning area exercises or imports `trajectory`.
- [`apps/web/tests/complex-history.perf.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/complex-history.perf.ts) — A test under the owning area exercises or imports `trajectory`.
- [`packages/core/agent/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/agent.spec.ts) — A test under the owning area exercises or imports `next-step`.
- [`apps/web/tests/live-interactions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/live-interactions.e2e.ts) — A test under the owning area exercises or imports `trajectory`.
- [`apps/web/tests/chat-scroll-contract.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/chat-scroll-contract.e2e.ts) — A test under the owning area exercises or imports `trajectory`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `next-step`.
- [`apps/web/tests/composer-tab-geometry.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/composer-tab-geometry.e2e.ts) — A test under the owning area exercises or imports `trajectory`.
- Source verification intent: Runtime tests pin target registration, exact-ID append, update-before-start replay, prepend identity, Reader window-gap repair, Location replay, and isolation between Chat and Trajectory snapshots. Trajectory Definition and Builder tests pin Assistant streaming and interruption, nested Tool calls and parallel interruption, Compaction and prompt inheritance, Steering classification and Step placement, Request marker order, stable contribution replacement, and prepend expansion.

## How to read the implementation

1. Start with [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/agent-loop`, `domain/build-release`, `domain/context`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `ConversationNodeAssembler`, `views`, `rootCallId`, `TrajectorySearchIndex`, `eventNodes`, `target`, `user/message`, `next-step`, `Session.views`, `step/start`, `step/end`, `agent/inbox/spliced`, `be the loaded raw Event count,`, `the number of Trajectory Definitions,`
- Regex: `(?i)(ConversationNodeAssembler|views|rootCallId|TrajectorySearchIndex|eventNodes|target|user/message|next\-step)`

```bash
rg -n --pcre2 "(?i)(ConversationNodeAssembler|views|rootCallId|TrajectorySearchIndex|eventNodes|target|user/message|next\\-step)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0113. Client Conversation business-node assembly and keyed Chat snapshots](0113-client-conversation-business-node-assembly-and-keyed-chat-snapshots.md): The source note links to this decision directly.
- **`source-link`** — [0194. Trajectory inspection ledger](0194-trajectory-inspection-ledger.md): The source note links to this decision directly.
- **`source-link`** — [0252. Web transcript marks context source, recall, and steering](0252-web-transcript-marks-context-source-recall-and-steering.md): The source note links to this decision directly.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `vendor/include/src/index.ts`.
- **`shares-code-with`** — [0189. Code Mode live dispatch lifecycle and native-contract parallelism](0189-code-mode-live-dispatch-lifecycle-and-native-contract-parallelism.md): Shares source implementation: `packages/core/tools/src/index.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0093. A session's agent is composed from a preset cordis.yml](0093-a-session-s-agent-is-composed-from-a-preset-cordis-yml.md): Shares source implementation: `packages/core/tools/src/index.ts`, `vendor/include/src/index.ts`.
- **`shares-code-with`** — [0561. Plan mode --- a logged per-agent session mode](0561-plan-mode-a-logged-per-agent-session-mode.md): Shares source implementation: `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0158. persistent PTY sessions](0158-persistent-pty-sessions.md): Shares source implementation: `packages/core/tools/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0127-trajectory-assembly-from-registered-conversation-contexts.md`.
