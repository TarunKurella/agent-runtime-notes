---
id: "dsh-note-0342"
title: "Load sessions from the pre-react-loop format"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-04-load-pre-react-loop-sessions.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/performance"
  - "concern/recovery"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
aliases:
  - "parent"
  - "user"
  - "legacy"
  - "disposed"
  - "inspect"
  - "hook"
  - "aborted"
  - "SESSION_FORMAT_VERSION"
  - "PersistenceCoordinator"
  - "steering/message"
  - "turn/start.trigger"
  - "agent/inbox/*"
  - "user/message"
  - "readFrom"
search_regex: "(?i)(parent|user|legacy|disposed|inspect|hook|aborted|SESSION_FORMAT_VERSION)"
---

# 0342. Load sessions from the pre-react-loop format — implementation context

## Open this when

The react-loop simplification changed durable events while retaining SESSION_FORMAT_VERSION 0. Stored sessions from the change's base contain steering/message and turn/start.trigger; their terminal reasons also use coarse aborted, separate disposed, and two older error payloads. Current surface and turn invariants cannot replay those records directly. The new durable inbox is not part of this compatibility problem. The base emitted process-local inbox notifications but no agent/inbox/ session events, so replaying old history as pending work would resurrect already claimed or discarded prompts.

## Source decision

PersistenceCoordinator recognizes the exact pre-react-loop shapes after backend decoding and projects them into the current read view. It removes the obsolete turn/start.trigger, converts steering/message to the same identified user/message, maps old failure facts into the current structured error, folds disposed into an aborted turn with the disposed cause, and represents coarse aborted records with the persistence-only { kind: 'legacy' } cause because their caller is unavailable. The coordinator applies the projection to load, inspect, adoption, HMR prefix comparison, and readFrom.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-04-load-pre-react-loop-sessions.md](../02-notes/implemented/bug-fix/2026-08-04-load-pre-react-loop-sessions.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-04-load-pre-react-loop-sessions.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-04-load-pre-react-loop-sessions.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/cli/src/args.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts) | runtime implementation | Defines `parent`, a construct named by the note. | `symbol-definition` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `parent`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `disposed`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `parent`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Defines `parent`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/fs-local/src/fsio.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts) | runtime implementation | Defines `parent`, a construct named by the note. | `symbol-definition` |
| [`vendor/loader/src/config/entry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/entry.ts) | runtime implementation | Defines `legacy`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Defines `SESSION_FORMAT_VERSION`, a construct named by the note. | `symbol-definition` |
| [`packages/util/timeout/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/index.ts) | package entry point | Defines `disposed`, a construct named by the note. | `symbol-definition` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Defines `user`, a construct named by the note. | `symbol-definition` |
| [`packages/plan/plan-mode/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/index.ts) | package entry point | Defines `disposed`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-pi-ai/src/config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/config.ts) | runtime implementation | Defines `legacy`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `parent` | `const` | [`apps/cli/src/args.ts:149`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts#L149) | `const parent = program.opts<BootOptions & { profile?: string }>()` |
| `user` | `const` | [`packages/boot/app-boot/src/index.ts:185`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L185) | `const user = home === resolve(cwd) ? undefined : readEnvLayer(binName, home, warn)` |
| `legacy` | `const` | [`packages/client/runtime/src/client/sessions/session.ts:736`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/session.ts#L736) | `const legacy = chat.legacy` |
| `disposed` | `let` | [`packages/client/runtime/src/client/slots.ts:376`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/slots.ts#L376) | `let disposed = false` |
| `inspect` | `const` | [`packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx:151`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx#L151) | `const inspect = useStore(s => s.inspect ?? null)` |
| `user` | `const` | [`packages/client/ui-settings-plugins/src/client/card-form.ts:344`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/client/card-form.ts#L344) | `const user = this.userLayer()` |
| `hook` | `let` | [`packages/client/web-react/src/session-provider.tsx:59`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/session-provider.tsx#L59) | `let hook = hookCache.get(source)` |
| `hook` | `let` | [`packages/client/web-react/src/session-provider.tsx:99`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/session-provider.tsx#L99) | `let hook = projectionHookCache.get(info)` |
| `aborted` | `const` | [`packages/core/agent-loop/src/index.ts:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L98) | `const aborted = Promise.withResolvers<never>()` |
| `aborted` | `let` | [`packages/core/agent-loop/src/tool-calls.ts:138`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/tool-calls.ts#L138) | `let aborted: boolean = signal.aborted` |
| `parent` | `const` | [`packages/core/agent/src/index.ts:677`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L677) | `const parent = fiber.parent.fiber` |
| `SESSION_FORMAT_VERSION` | `const` | [`packages/core/session/src/types.ts:56`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L56) | `export const SESSION_FORMAT_VERSION = 0` |
| `parent` | `const` | [`packages/core/tools/src/index.ts:1371`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1371) | `const parent = exec.parent` |
| `inspect` | `const` | [`packages/extensions/cordis-client-runner/src/client/index.ts:189`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-client-runner/src/client/index.ts#L189) | `const inspect = new ClientCordisInspectRegistry({` |
| `parent` | `const` | [`packages/fs/fs-local/src/fsio.ts:187`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L187) | `const parent = dirname(ancestor)` |
| `hook` | `const` | [`packages/hooks/hooks-claude-code/src/config.ts:95`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-claude-code/src/config.ts#L95) | `const hook = asObject(rawHook)` |

### Tests and executable evidence

- [`packages/e2b/e2b/tests/composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/tests/composition.e2e.ts) — A test under the owning area exercises or imports `readFrom`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `SESSION_FORMAT_VERSION`.
- [`packages/shell/pwsh-local/tests/executor.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/tests/executor.spec.ts) — A test under the owning area exercises or imports `readFrom`.
- [`packages/shell/bash-sandbox/tests/sandbox.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/tests/sandbox.spec.ts) — A test under the owning area exercises or imports `readFrom`.
- [`packages/feedback/message-feedback/tests/helpers.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/message-feedback/tests/helpers.ts) — A test under the owning area exercises or imports `readFrom`.
- [`packages/host/apiproxy/tests/api-proxy-cold.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-cold.spec.ts) — A test under the owning area exercises or imports `readFrom`.
- [`packages/e2b/subprocess-e2b/tests/subprocess.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/tests/subprocess.spec.ts) — A test under the owning area exercises or imports `readFrom`.
- [`packages/subprocess/subprocess/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/tests/service.spec.ts) — A test under the owning area exercises or imports `readFrom`.

## How to read the implementation

1. Start with [`apps/cli/src/args.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/human-control`, `concern/lifecycle`, `concern/performance`, `concern/recovery`, `domain/agent-loop`, `domain/build-release`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/projection`
- Aliases: `parent`, `user`, `legacy`, `disposed`, `inspect`, `hook`, `aborted`, `SESSION_FORMAT_VERSION`, `PersistenceCoordinator`, `steering/message`, `turn/start.trigger`, `agent/inbox/*`, `user/message`, `readFrom`
- Regex: `(?i)(parent|user|legacy|disposed|inspect|hook|aborted|SESSION_FORMAT_VERSION)`

```bash
rg -n --pcre2 "(?i)(parent|user|legacy|disposed|inspect|hook|aborted|SESSION_FORMAT_VERSION)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0009. Session persistence as an abstract service over the existing `SessionEvent`](0009-session-persistence-as-an-abstract-service-over-the-existing-sessioneven.md): The source note links to this decision directly.
- **`source-link`** — [0311. Load sessions persisted before message identity](0311-load-sessions-persisted-before-message-identity.md): The source note links to this decision directly.
- **`shares-code-with`** — [0611. Keep the Code Mode result card complete](0611-keep-the-code-mode-result-card-complete.md): Shares source implementation: `apps/cli/src/args.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): Shares source implementation: `packages/util/timeout/src/index.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0142. Dynamic workflows --- a script-driven multi-agent orchestration seam](0142-dynamic-workflows-a-script-driven-multi-agent-orchestration-seam.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0554. dsh-tui chat channel module split](0554-dsh-tui-chat-channel-module-split.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/llm/llm/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0342-load-sessions-from-the-pre-react-loop-format.md`.
