---
id: "dsh-note-0576"
title: "TUI footer shows the session cache hit rate"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-21-tui-footer-cache-hit-rate.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/ownership"
  - "concern/performance"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "round"
  - "addUsage"
  - "output"
  - "input"
  - "cacheRead"
  - "TokenUsage"
  - "is the uncached input reported by the model."
  - "TokenTotals"
  - "cacheWrite"
  - "cacheReadTokens"
  - "cacheWriteTokens"
  - "cacheHitRate"
  - "round(cacheRead / (input + cacheRead + cacheWrite) * 100)"
  - "FooterComponent"
search_regex: "(?i)(round|addUsage|output|input|cacheRead|TokenUsage|is[- ]the[- ]uncached[- ]input[- ]reported[- ]by[- ]the[- ]model\\.|TokenTotals)"
---

# 0576. TUI footer shows the session cache hit rate — implementation context

## Open this when

The footer summed the session's token usage as ↑ ↓, where ↑ is the uncached input reported by the model. TokenUsage counts are disjoint: billed prompt tokens are inputTokens (uncached) plus cacheReadTokens and cacheWriteTokens. With only the uncached number visible, a user could not tell how much of each turn's prompt the provider cache served --- the signal that most directly reflects whether the reused request prefix is paying off. On a long session dominated by cache reads the ↑ figure stays small and hides that the prompt is large but cheap.

## Source decision

The footer appends cache % after ↑ ↓, where the rate is the share of billed prompt tokens served from the provider cache. TokenTotals accumulates the four disjoint buckets (input, output, cacheRead, cacheWrite). addUsage folds one call's TokenUsage into the totals, treating a missing cacheReadTokens/cacheWriteTokens as zero. cacheHitRate(totals) is round(cacheRead / (input + cacheRead + cacheWrite) 100), and undefined before any input is billed. FooterComponent omits the whole cache N% segment while the rate is undefined, so an empty session shows no meaningless zero.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-21-tui-footer-cache-hit-rate.md](../02-notes/archived/feature/2026-07-21-tui-footer-cache-hit-rate.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-21-tui-footer-cache-hit-rate.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-21-tui-footer-cache-hit-rate.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/cosmokit/src/string.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cosmokit/src/string.ts) | runtime implementation | Defines `output`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Defines `TokenUsage`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/src/edit.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/edit.ts) | runtime implementation | Defines `input`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/src/read.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts) | runtime implementation | Defines `input`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts) | package entry point | Defines `input`, a construct named by the note. Defines `output`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/src/write.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/write.ts) | runtime implementation | Defines `input`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `output`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) | package entry point | Defines `output`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-deepseek/src/translate.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/translate.ts) | runtime implementation | Defines `cacheRead`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal-round-driver/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal-round-driver/src/index.ts) | package entry point | Defines `round`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/loader-smoke/src/agent-turn.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/loader-smoke/src/agent-turn.ts) | runtime implementation | Defines `addUsage`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/TrajectoryView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryView.tsx) | runtime implementation | Defines `addUsage`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `round` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx:820`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx#L820) | `const round = properties.round` |
| `addUsage` | `function` | [`packages/client/ui-trajectory/src/client/TrajectoryView.tsx:96`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryView.tsx#L96) | `function addUsage(` |
| `addUsage` | `function` | [`packages/client/ui-trajectory/src/client/trajectory-assistant-definition.ts:91`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/trajectory-assistant-definition.ts#L91) | `function addUsage(current: UsageValue \| undefined, next: UsageValue): UsageValue {` |
| `output` | `const` | [`packages/core/tools/src/index.ts:1039`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1039) | `const output = (definition as Partial<ToolDefinition>).output` |
| `output` | `const` | [`packages/core/tools/src/index.ts:1243`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1243) | `const output = snapshotJsonValue(definition.output.schema)` |
| `input` | `const` | [`packages/fs/tool-fs/src/edit.ts:113`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/edit.ts#L113) | `const input = parseEditArgs(args)` |
| `input` | `const` | [`packages/fs/tool-fs/src/read.ts:107`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts#L107) | `const input = parseReadArgs(args, caps.limit)` |
| `input` | `const` | [`packages/fs/tool-fs/src/read.ts:137`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts#L137) | `const input = parseReadArgs(args, caps.limit)` |
| `input` | `const` | [`packages/fs/tool-fs/src/write.ts:103`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/write.ts#L103) | `const input = parseWriteArgs(args)` |
| `round` | `const` | [`packages/goal/goal-round-driver/src/index.ts:174`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal-round-driver/src/index.ts#L174) | `const round = goal.roundsStarted + 1` |
| `cacheRead` | `const` | [`packages/llm/llm-deepseek/src/translate.ts:54`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/translate.ts#L54) | `const cacheRead = usage.prompt_tokens_details?.cached_tokens ?? usage.prompt_cache_hit_tokens` |
| `TokenUsage` | `interface` | [`packages/llm/llm/src/types.ts:135`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L135) | `export interface TokenUsage {` |
| `input` | `const` | [`packages/sdk/server/src/index.ts:53`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts#L53) | `const input = config.input ?? process.stdin` |
| `output` | `const` | [`packages/sdk/server/src/index.ts:55`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts#L55) | `const output = config.output ?? process.stdout` |
| `output` | `const` | [`packages/shell/tool-bash/src/index.ts:160`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts#L160) | `const output = (stream: ShellRunResult['stdout']) => ({` |
| `addUsage` | `function` | [`packages/test-support/loader-smoke/src/agent-turn.ts:25`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/loader-smoke/src/agent-turn.ts#L25) | `function addUsage(total: TokenUsage \| undefined, step: TokenUsage): TokenUsage {` |

### Tests and executable evidence

- [`vitest.e2e.config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vitest.e2e.config.ts) — A test under the owning area exercises or imports `and`.
- [`scripts/translation-brief.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-brief.spec.ts) — A test under the owning area exercises or imports `and`.
- [`scripts/gen-client-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-client-catalog.spec.ts) — A test under the owning area exercises or imports `and`.
- [`scripts/translation-pairing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing.spec.ts) — A test under the owning area exercises or imports `and`.
- [`scripts/verify-cordis-config.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-cordis-config.spec.ts) — A test under the owning area exercises or imports `and`.
- [`apps/web/tests/chat-scroll-fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/chat-scroll-fixture.ts) — A test under the owning area exercises or imports `inputTokens`.
- [`apps/web/tests/complex-history.perf.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/complex-history.perf.ts) — A test under the owning area exercises or imports `cacheReadTokens`. A test under the owning area exercises or imports `inputTokens`.
- [`apps/web/tests/skill-user-invoke.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/skill-user-invoke.e2e.ts) — A test under the owning area exercises or imports `inputTokens`.
- Source verification intent: packages/ui/tui/tests/tui.spec.ts drives the footer through the real createTuiChat: an empty session renders ↑0 ↓0 with no cache segment (the hidden path), a cold turn (inputTokens only) renders cache 0%, and a live warm turn carrying cacheReadTokens and cacheWriteTokens updates it to cache 60% while no longer showing cache 0%. The examples/tui-agent snapshot suite replays green against the recorded expected output.

## How to read the implementation

1. Start with [`vendor/cosmokit/src/string.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cosmokit/src/string.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/ownership`, `concern/performance`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `round`, `addUsage`, `output`, `input`, `cacheRead`, `TokenUsage`, `is the uncached input reported by the model.`, `TokenTotals`, `cacheWrite`, `cacheReadTokens`, `cacheWriteTokens`, `cacheHitRate`, `round(cacheRead / (input + cacheRead + cacheWrite) * 100)`, `FooterComponent`
- Regex: `(?i)(round|addUsage|output|input|cacheRead|TokenUsage|is[- ]the[- ]uncached[- ]input[- ]reported[- ]by[- ]the[- ]model\.|TokenTotals)`

```bash
rg -n --pcre2 "(?i)(round|addUsage|output|input|cacheRead|TokenUsage|is[- ]the[- ]uncached[- ]input[- ]reported[- ]by[- ]the[- ]model\\.|TokenTotals)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0019. Make `dsh-fs-observation-policy` an event-gate plugin, not a method interface](0019-make-dsh-fs-observation-policy-an-event-gate-plugin-not-a-method-interfa.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/fs/tool-fs/src/edit.ts`.
- **`shares-code-with`** — [0226. Web tool-row unified expand and trajectory Inspect](0226-web-tool-row-unified-expand-and-trajectory-inspect.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/sdk/server/src/index.ts`.
- **`shares-code-with`** — [0560. Subagent lifecycle enrichment --- lastAssistantMessage (observe-only)](0560-subagent-lifecycle-enrichment-lastassistantmessage-observe-only.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/sdk/server/src/index.ts`.
- **`shares-code-with`** — [0130. Multiplex concurrent ACP sessions over one connection](0130-multiplex-concurrent-acp-sessions-over-one-connection.md): Shares source implementation: `packages/fs/tool-fs/src/edit.ts`, `packages/fs/tool-fs/src/write.ts`.
- **`shares-code-with`** — [0528. Prune dead public API and result fields](0528-prune-dead-public-api-and-result-fields.md): Shares source implementation: `packages/fs/tool-fs/src/edit.ts`, `packages/fs/tool-fs/src/read.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/fs/tool-fs/src/read.ts`.
- **`shares-code-with`** — [0018. Web capability seam - stable tools over multiple providers](0018-web-capability-seam-stable-tools-over-multiple-providers.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/llm/llm/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0576-tui-footer-shows-the-session-cache-hit-rate.md`.
