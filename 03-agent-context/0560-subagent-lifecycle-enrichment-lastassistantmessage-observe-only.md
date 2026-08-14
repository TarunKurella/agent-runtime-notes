---
id: "dsh-note-0560"
title: "Subagent lifecycle enrichment --- lastAssistantMessage (observe-only)"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-06-30-subagent-observe-enrich.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/shell-terminal"
  - "domain/storage"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "stopReason"
  - "provider"
  - "resume"
  - "output"
  - "SubagentRunEndInfo"
  - "SubagentResult"
  - "emit"
  - "subagent/start"
  - "subagent/end"
  - "lastAssistantMessage"
  - "SubagentResult.output"
  - "stopReason: 'error"
  - "SubagentService.start"
  - "ctx.agents.get"
search_regex: "(?i)(stopReason|provider|resume|output|SubagentRunEndInfo|SubagentResult|emit|subagent/start)"
---

# 0560. Subagent lifecycle enrichment --- lastAssistantMessage (observe-only) — implementation context

## Open this when

The hooks subsystem (interception seams Agent Note) lets a plugin observe and gate the agent at lifecycle points. Claude Code and Codex both expose SubagentStart / SubagentStop hooks, and CC's carry the subagent's final message. The harness already emits subagent/start and subagent/end lifecycle events (the subagent capability-seam), but their payloads were minimal (provider, id, and on end stopReason) --- not enough for a hooks bridge to report WHAT a subagent produced without separately reaching for the live run. This Agent Note enriches the end payload.

## Source decision

Add lastAssistantMessage --- the child's final output --- to SubagentRunEndInfo. On the settle path it is the readonly typed SubagentResult.output, so an observer sees what the child produced without holding the run. On an infrastructure rejection where no SubagentResult exists, it is absent and the event reports stopReason: 'error'. Providers and listeners are trusted same-process collaborators and honor the borrowed immutable payload contract. Both events stay plain emits.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-06-30-subagent-observe-enrich.md](../02-notes/archived/feature/2026-06-30-subagent-observe-enrich.md)
- Pinned source: [.agents/notes/archived/feature/2026-06-30-subagent-observe-enrich.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-06-30-subagent-observe-enrich.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `provider`, a construct named by the note. | `symbol-definition` |
| [`packages/web/web/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web/src/index.ts) | package entry point | Defines `provider`, a construct named by the note. | `symbol-definition` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `stopReason`, a construct named by the note. | `symbol-definition` |
| [`vendor/cosmokit/src/string.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cosmokit/src/string.ts) | runtime implementation | Defines `output`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `output`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts) | package entry point | Defines `output`, a construct named by the note. | `symbol-definition` |
| [`packages/api/gateway/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts) | package entry point | Defines `provider`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) | package entry point | Defines `output`, a construct named by the note. | `symbol-definition` |
| [`packages/subagent/subagent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts) | public types and contract | Defines `SubagentRunEndInfo`, a construct named by the note. Defines `SubagentResult`, a construct named by the note. | `symbol-definition` |
| [`packages/api/remotes/src/agent-lookup.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/src/agent-lookup.ts) | runtime implementation | Defines `resume`, a construct named by the note. | `symbol-definition` |
| [`packages/hooks/hook-protocol/src/codec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/codec.ts) | runtime implementation | Defines `stopReason`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/llm-mock-server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-mock-server/src/index.ts) | package entry point | Defines `emit`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `stopReason` | `const` | [`packages/acp/acp/src/index.ts:297`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L297) | `const stopReason = await new Promise<StopReason>((resolve, reject) => {` |
| `provider` | `const` | [`packages/api/gateway/src/index.ts:322`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts#L322) | `const provider = this.ctx.typert.contexts.getHost(marker.invocation.context)` |
| `resume` | `let` | [`packages/api/remotes/src/agent-lookup.ts:143`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/src/agent-lookup.ts#L143) | `let resume = resumes.get(sessionId)` |
| `output` | `const` | [`packages/core/tools/src/index.ts:1039`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1039) | `const output = (definition as Partial<ToolDefinition>).output` |
| `output` | `const` | [`packages/core/tools/src/index.ts:1243`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1243) | `const output = snapshotJsonValue(definition.output.schema)` |
| `stopReason` | `const` | [`packages/hooks/hook-protocol/src/codec.ts:100`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/codec.ts#L100) | `const stopReason = str(parsed, 'stopReason')` |
| `provider` | `const` | [`packages/llm/llm/src/index.ts:632`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L632) | `const provider = registration.provider.id` |
| `output` | `const` | [`packages/sdk/server/src/index.ts:55`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts#L55) | `const output = config.output ?? process.stdout` |
| `output` | `const` | [`packages/shell/tool-bash/src/index.ts:160`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts#L160) | `const output = (stream: ShellRunResult['stdout']) => ({` |
| `stopReason` | `const` | [`packages/subagent/subagent-in-process-driver/src/index.ts:225`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-in-process-driver/src/index.ts#L225) | `const stopReason: SubagentStopReason = cancelled && recorded !== 'completed' ? 'aborted' : recorded` |
| `SubagentRunEndInfo` | `interface` | [`packages/subagent/subagent/src/types.ts:56`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts#L56) | `export interface SubagentRunEndInfo {` |
| `SubagentResult` | `interface` | [`packages/subagent/subagent/src/types.ts:219`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts#L219) | `export interface SubagentResult {` |
| `emit` | `function` | [`packages/test-support/llm-mock-server/src/index.ts:291`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-mock-server/src/index.ts#L291) | `function emit(options: ResolvedOptions, event: MockLlmServerEvent): void {` |
| `provider` | `const` | [`packages/web/web/src/index.ts:141`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web/src/index.ts#L141) | `const provider = resolveProvider({` |
| `provider` | `const` | [`packages/web/web/src/index.ts:158`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web/src/index.ts#L158) | `const provider = resolveProvider({` |
| `provider` | `const` | [`packages/web/web/src/index.ts:175`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web/src/index.ts#L175) | `const provider = providers.get(configuredId)` |

### Tests and executable evidence

- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `TODO`.
- [`scripts/translation-prompt.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-prompt.spec.ts) — A test under the owning area exercises or imports `FIXME`.
- [`packages/acp/acp/tests/edges.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/edges.spec.ts) — A test under the owning area exercises or imports `stopReason`.
- [`packages/acp/acp/tests/turns.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/turns.spec.ts) — A test under the owning area exercises or imports `stopReason`.
- [`packages/acp/acp/tests/bridge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/bridge.spec.ts) — A test under the owning area exercises or imports `stopReason`.
- [`packages/acp/acp/tests/dispose.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/dispose.spec.ts) — A test under the owning area exercises or imports `stopReason`.
- [`packages/sdk/server/tests/server.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/tests/server.spec.ts) — A test under the owning area exercises or imports `lastAssistantMessage`.
- [`packages/sdk/client/tests/fake-runtime.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/tests/fake-runtime.ts) — A test under the owning area exercises or imports `lastAssistantMessage`.

## How to read the implementation

1. Start with [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
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

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/shell-terminal`, `domain/storage`, `lifecycle/archived`, `mechanism/policy`, `mechanism/registry`
- Aliases: `stopReason`, `provider`, `resume`, `output`, `SubagentRunEndInfo`, `SubagentResult`, `emit`, `subagent/start`, `subagent/end`, `lastAssistantMessage`, `SubagentResult.output`, `stopReason: 'error`, `SubagentService.start`, `ctx.agents.get`
- Regex: `(?i)(stopReason|provider|resume|output|SubagentRunEndInfo|SubagentResult|emit|subagent/start)`

```bash
rg -n --pcre2 "(?i)(stopReason|provider|resume|output|SubagentRunEndInfo|SubagentResult|emit|subagent/start)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0226. Web tool-row unified expand and trajectory Inspect](0226-web-tool-row-unified-expand-and-trajectory-inspect.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/sdk/server/src/index.ts`.
- **`shares-code-with`** — [0018. Web capability seam - stable tools over multiple providers](0018-web-capability-seam-stable-tools-over-multiple-providers.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0058. Separate context injection from turn execution](0058-separate-context-injection-from-turn-execution.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0576. TUI footer shows the session cache hit rate](0576-tui-footer-shows-the-session-cache-hit-rate.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/sdk/server/src/index.ts`.
- **`shares-code-with`** — [0093. A session's agent is composed from a preset cordis.yml](0093-a-session-s-agent-is-composed-from-a-preset-cordis-yml.md): Shares source implementation: `packages/api/gateway/src/index.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0200. Continuable subagents](0200-continuable-subagents.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0144. The approval seam --- one-shot permission decisions over a waterfall of answerers](0144-the-approval-seam-one-shot-permission-decisions-over-a-waterfall-of-answ.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/llm/llm/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0560-subagent-lifecycle-enrichment-lastassistantmessage-observe-only.md`.
