---
id: "dsh-note-0348"
title: "`list_agents` uses `ready` for resumable children"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-06-list-agents-residency-vocabulary.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/projection"
aliases:
  - "parameters"
  - "idle"
  - "running"
  - "activity"
  - "ready"
  - "complete"
  - "list_agents"
  - "send_message"
  - "SubagentListEntry.activity"
  - "<id> [running] --- <label>"
  - "<id> [idle] --- <label>"
  - "<id> [ready] --- <label>"
  - "subagent-list-agents"
  - "`list_agents` uses `ready` for resumable children"
search_regex: "(?i)(parameters|idle|running|activity|ready|complete|list_agents|send_message)"
---

# 0348. `list_agents` uses `ready` for resumable children — implementation context

## Open this when

The word is especially misleading alongside manager-owned settlement delivery. Completion reaches the parent as a notice; listing exists to recall durable conversations, not to poll for that notice.

## Source decision

running means the resident Agent has an active driver. idle means the Agent is resident between turns and may be waiting on agents it started. ready means only the durable conversation remains. send_message starts the next turn on the same conversation; the status is resumable rather than terminal and does not mean a result is waiting to be collected. The tool description states those distinctions and directs the model away from polling: it says the parent is told when a child finishes and that listing is for recalling which children it started.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-06-list-agents-residency-vocabulary.md](../02-notes/implemented/bug-fix/2026-08-06-list-agents-residency-vocabulary.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-06-list-agents-residency-vocabulary.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-06-list-agents-residency-vocabulary.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `ready`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts) | runtime implementation | Defines `parameters`, a construct named by the note. | `symbol-definition` |
| [`packages/api/gateway/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts) | package entry point | Defines `parameters`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts) | package entry point | Defines `parameters`, a construct named by the note. | `symbol-definition` |
| [`packages/jobs/tool-jobs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts) | package entry point | Defines `complete`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/bash-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts) | package entry point | Defines `running`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/pwsh-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/index.ts) | package entry point | Defines `running`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `ready`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/emitter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/emitter.ts) | runtime implementation | Defines `parameters`, a construct named by the note. | `symbol-definition` |
| [`packages/mcp/mcp-client/src/connection.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/connection.ts) | runtime implementation | Defines `ready`, a construct named by the note. | `symbol-definition` |
| [`packages/skill/skill-filesystem/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts) | package entry point | Defines `ready`, a construct named by the note. Defines `complete`, a construct named by the note. | `symbol-definition` |
| [`packages/terminal/tool-terminal/src/render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/tool-terminal/src/render.ts) | runtime implementation | Defines `complete`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `parameters` | `const` | [`packages/api/gateway/src/index.ts:285`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts#L285) | `const parameters: InvocationParameterDescriptor[] = []` |
| `idle` | `const` | [`packages/client/connection/src/client/connection.ts:166`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/connection.ts#L166) | `const idle = new AbortController()` |
| `running` | `const` | [`packages/client/connection/src/client/fixture.ts:1792`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1792) | `const running = summaryOf(id)?.running === true` |
| `activity` | `const` | [`packages/client/runtime/src/client/sessions/manager.ts:930`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts#L930) | `const activity = running ? 'running' as const : 'inactive' as const` |
| `activity` | `const` | [`packages/client/runtime/src/client/sessions/manager.ts:978`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts#L978) | `const activity = activityRows.get(entry.id)` |
| `running` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L156) | `const running = useSession(s => s.running)` |
| `activity` | `const` | [`packages/client/ui-subagent/src/client/SubagentCatalogAction.tsx:275`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-subagent/src/client/SubagentCatalogAction.tsx#L275) | `const activity = entry.activity === 'running' ? t('activity.running') : t('activity.inactive')` |
| `parameters` | `const` | [`packages/core/tools/src/schema.ts:566`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L566) | `const parameters = parameterSchemaSpecToJsonSchema(options.parameters)` |
| `activity` | `const` | [`packages/extensions/ui-cordis/src/client/CordisPanel.tsx:138`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/CordisPanel.tsx#L138) | `const activity = activeRuns.get(listed.pluginId)` |
| `running` | `const` | [`packages/extensions/ui-cordis/src/client/CordisPanel.tsx:154`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/CordisPanel.tsx#L154) | `const running = all.filter(view => visiblePanelStatus(` |
| `activity` | `const` | [`packages/extensions/ui-cordis/src/client/CordisRunRow.tsx:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/CordisRunRow.tsx#L61) | `const activity = card.pluginId === null ? undefined : activeRuns.get(card.pluginId)` |
| `ready` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:3657`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L3657) | `const ready: SessionLogExportReady = {` |
| `complete` | `const` | [`packages/jobs/tool-jobs/src/index.ts:129`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L129) | `const complete = \`${content}${suffix}\`` |
| `complete` | `const` | [`packages/jobs/tool-jobs/src/index.ts:150`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L150) | `const complete = \`${prefix}${detail}. Read its output with job_output.\`` |
| `ready` | `const` | [`packages/mcp/mcp-client/src/connection.ts:313`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/connection.ts#L313) | `const ready: Promise<ConnectionOutcome> = settling.then(() => {` |
| `running` | `const` | [`packages/shell/bash-local/src/index.ts:257`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts#L257) | `const running = this.ctx.subprocess.spawn(this.spawnSpec(spec, argv, this.config.maxOutputBytes, spec.signal))` |

### Tests and executable evidence

- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — A test under the owning area exercises or imports `inactive`.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `list_agents`. A test under the owning area exercises or imports `send_message`.
- [`examples/acp-agent/tests/acp.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/acp.snapshot.ts) — A test under the owning area exercises or imports `list_agents`. A test under the owning area exercises or imports `send_message`.
- [`apps/web/tests/shipped-composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/shipped-composition.e2e.ts) — A test under the owning area exercises or imports `list_agents`. A test under the owning area exercises or imports `send_message`.
- [`apps/web/tests/subagent-conversation.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/subagent-conversation.e2e.ts) — A test under the owning area exercises or imports `inactive`.
- [`packages/core/agent-loop/tests/resume.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/resume.spec.ts) — A test under the owning area exercises or imports `inactive`.
- [`packages/boot/app-boot/tests/user-patches.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/user-patches.spec.ts) — A test under the owning area exercises or imports `inactive`.
- [`packages/core/tools/tests/gen-tool-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/gen-tool-catalog.spec.ts) — A test under the owning area exercises or imports `list_agents`. A test under the owning area exercises or imports `send_message`.

## How to read the implementation

1. Start with [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/concurrency`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/projection`
- Aliases: `parameters`, `idle`, `running`, `activity`, `ready`, `complete`, `list_agents`, `send_message`, `SubagentListEntry.activity`, `<id> [running] --- <label>`, `<id> [idle] --- <label>`, `<id> [ready] --- <label>`, `subagent-list-agents`, ``list_agents` uses `ready` for resumable children`
- Regex: `(?i)(parameters|idle|running|activity|ready|complete|list_agents|send_message)`

```bash
rg -n --pcre2 "(?i)(parameters|idle|running|activity|ready|complete|list_agents|send_message)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0173. Durable subagent catalog and list_agents](0173-durable-subagent-catalog-and-list-agents.md): The source note links to this decision directly.
- **`source-link`** — [0265. Settlement delivery belongs to the continuation manager](0265-settlement-delivery-belongs-to-the-continuation-manager.md): The source note links to this decision directly.
- **`shares-code-with`** — [0270. Steer the whole Web queue with an empty-draft Cmd/Ctrl+Enter](0270-steer-the-whole-web-queue-with-an-empty-draft-cmd-ctrl-enter.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/shell/bash-local/src/index.ts`.
- **`shares-code-with`** — [0353. Latch wake-ups that land in the cancel-convergence window](0353-latch-wake-ups-that-land-in-the-cancel-convergence-window.md): Shares source implementation: `packages/mcp/mcp-client/src/connection.ts`, `packages/shell/bash-local/src/index.ts`.
- **`shares-code-with`** — [0622. Web conversation UI polish sweep](0622-web-conversation-ui-polish-sweep.md): Shares source implementation: `packages/api/gateway/src/index.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0591. Code Mode sub-calls in the trajectory and waterfall views](0591-code-mode-sub-calls-in-the-trajectory-and-waterfall-views.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/shell/bash-local/src/index.ts`.
- **`shares-code-with`** — [0625. Hero stays visible while a blank session opens](0625-hero-stays-visible-while-a-blank-session-opens.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/mcp/mcp-client/src/connection.ts`.
- **`shares-code-with`** — [0546. Extract example apps into packages](0546-extract-example-apps-into-packages.md): Shares source implementation: `packages/shell/bash-local/src/index.ts`, `vendor/hmr/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0348-list-agents-uses-ready-for-resumable-children.md`.
