---
id: "dsh-note-0033"
title: "After-call compaction pressure and context-overflow recovery"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-10-after-call-compaction-pressure-and-overflow-recovery.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/agent-loop"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/streaming"
aliases:
  - "summarize"
  - "pressure"
  - "modelPolicies"
  - "maxOverflowRetries"
  - "model"
  - "CompactionResult"
  - "aborted"
  - "GenerateOptions"
  - "agent/pre-step"
  - "agent/request"
  - "dsh-compaction-basic"
  - "ctx.tokenMeter"
  - "tokenMeter"
  - "AgentOptions.model"
search_regex: "(?i)(summarize|pressure|modelPolicies|maxOverflowRetries|model|CompactionResult|aborted|GenerateOptions)"
---

# 0033. After-call compaction pressure and context-overflow recovery — implementation context

## Open this when

agent/pre-step runs before final request routing and before assistant output, tool results, buffered context, and steering exist. Even with the assembled prompt and session prefix, its pressure view is provisional because agent/request can still change routing or call configuration and tool schemas are not frozen with those inputs. Adding fields cannot make pre-call state describe a completed call and couples the generic extension point to compaction. Successful calls are not the only pressure signal.

## Source decision

agent/pre-step receives the exclusive claimed message batch plus { turn, step, signal } and returns the final reject/enter decision. It carries no compaction-only prompt or prefix fields. Compact-basic wraps agent/pre-step before each proposed request. At a continuation boundary the preceding assistant output, every dispatched or synthetic tool result, post-tool context, and steering are already durable, so pressure policy sees the complete successful-call state without splitting an assistant tool call from its result.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-10-after-call-compaction-pressure-and-overflow-recovery.md](../02-notes/implemented/architecture/2026-07-10-after-call-compaction-pressure-and-overflow-recovery.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-10-after-call-compaction-pressure-and-overflow-recovery.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-10-after-call-compaction-pressure-and-overflow-recovery.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/compaction/compaction-basic/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/index.ts) | package entry point | Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-package-member` |
| [`packages/compaction/compaction-basic/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-package-member` |
| [`packages/compaction/compaction-basic/src/config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/config.ts) | runtime implementation | Core file in the package named by the note: `packages/compaction/compaction-basic`. Defines `model`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/compaction/compaction-basic/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-package-member` |
| [`packages/compaction/compaction-basic/src/summarizer.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/summarizer.ts) | runtime implementation | Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-package-member` |
| [`packages/compaction/compaction-basic`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Defines `GenerateOptions`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Defines `aborted`, a construct named by the note. | `symbol-definition` |
| [`packages/bundle/headless/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts) | package entry point | Defines `summarize`, a construct named by the note. | `symbol-definition` |
| [`packages/compaction/compaction/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/types.ts) | public types and contract | Defines `CompactionResult`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/ContextMeter.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ContextMeter.tsx) | runtime implementation | Defines `pressure`, a construct named by the note. | `symbol-definition` |
| [`packages/compaction/compaction-basic/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/README.md) | package contract and examples | Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `summarize` | `function` | [`packages/bundle/headless/src/index.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts#L61) | `function summarize(events: readonly SessionEvent[], firstSeq: number): RunOutcome {` |
| `pressure` | `const` | [`packages/client/ui-conversation/src/client/skeleton/ContextMeter.tsx:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ContextMeter.tsx#L41) | `const pressure = useProjection('contextPressure')` |
| `modelPolicies` | `const` | [`packages/compaction/compaction-basic/src/config.ts:77`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/config.ts#L77) | `const modelPolicies = resolveModelPolicies(config.modelPolicies)` |
| `maxOverflowRetries` | `const` | [`packages/compaction/compaction-basic/src/config.ts:236`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/config.ts#L236) | `const maxOverflowRetries = config.maxOverflowRetries` |
| `model` | `const` | [`packages/compaction/compaction-basic/src/config.ts:260`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/config.ts#L260) | `const model = config.summarizationModel` |
| `CompactionResult` | `interface` | [`packages/compaction/compaction/src/types.ts:93`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/types.ts#L93) | `export interface CompactionResult {` |
| `aborted` | `const` | [`packages/core/agent-loop/src/index.ts:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L98) | `const aborted = Promise.withResolvers<never>()` |
| `GenerateOptions` | `interface` | [`packages/llm/llm/src/types.ts:320`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L320) | `export interface GenerateOptions {` |

### Tests and executable evidence

- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `GenerateOptions`.
- [`packages/llm/llm/tests/topology.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/topology.spec.ts) — A test under the owning area exercises or imports `GenerateOptions`.
- [`packages/llm/llm/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/invariant.spec.ts) — A test under the owning area exercises or imports `GenerateOptions`.
- [`packages/llm/llm/tests/call-config.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/call-config.spec.ts) — A test under the owning area exercises or imports `GenerateOptions`.
- [`packages/compaction/compaction/tests/compaction.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/tests/compaction.spec.ts) — A test under the owning area exercises or imports `CompactionResult`.
- [`packages/compaction/compaction-basic/tests/compaction-basic.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/compaction-basic.spec.ts) — A test under the owning area exercises or imports `dsh-compaction-basic`. A test under the owning area exercises or imports `tokenMeter`.
- [`packages/client/ui-conversation/tests/context-meter.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/context-meter.client.spec.tsx) — A test under the owning area exercises or imports `pressure`.
- [`packages/compaction/compaction-basic/tests/manual-compaction.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/manual-compaction.spec.ts) — A test under the owning area exercises or imports `dsh-compaction-basic`. A test under the owning area exercises or imports `tokenMeter`.
- Source verification intent: Unit tests cover the final-adapter normalization boundary, closed-turn retry numbering and reset, cancellation and disposal, step-boundary ordering, routed-envelope pressure, pressure-gated pruning, pruning-only relief, pruned-input summarization, balanced overflow reduction, durable prune progress before later failure, generation proof, caps, delegation, and auxiliary-call routing. Real-loop tests cover thrown and in-band overflow through pruning or summary compaction to a reconstructed retry request.

## How to read the implementation

1. Start with [`packages/compaction/compaction-basic/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/agent-loop`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/generation`, `mechanism/policy`, `mechanism/streaming`
- Aliases: `summarize`, `pressure`, `modelPolicies`, `maxOverflowRetries`, `model`, `CompactionResult`, `aborted`, `GenerateOptions`, `agent/pre-step`, `agent/request`, `dsh-compaction-basic`, `ctx.tokenMeter`, `tokenMeter`, `AgentOptions.model`
- Regex: `(?i)(summarize|pressure|modelPolicies|maxOverflowRetries|model|CompactionResult|aborted|GenerateOptions)`

```bash
rg -n --pcre2 "(?i)(summarize|pressure|modelPolicies|maxOverflowRetries|model|CompactionResult|aborted|GenerateOptions)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0078. Terminal LLM stream failures](0078-terminal-llm-stream-failures.md): The source note links to this decision directly.
- **`source-link`** — [0088. Claim inbox input before one pre-step decision](0088-claim-inbox-input-before-one-pre-step-decision.md): The source note links to this decision directly.
- **`source-link`** — [0464. Request-error retry action](0464-request-error-retry-action.md): The source note links to this decision directly.
- **`shares-code-with`** — [0540. Fold the single compaction backend into its service package](0540-fold-the-single-compaction-backend-into-its-service-package.md): Shares source implementation: `packages/bundle/headless/src/index.ts`, `packages/compaction/compaction-basic`.
- **`shares-code-with`** — [0343. the context meter could not see a compaction](0343-the-context-meter-could-not-see-a-compaction.md): Shares source implementation: `packages/compaction/compaction-basic`, `packages/compaction/compaction-basic/src/index.ts`.
- **`shares-code-with`** — [0215. Queued manual compaction with one durable lock](0215-queued-manual-compaction-with-one-durable-lock.md): Shares source implementation: `packages/compaction/compaction-basic/src/index.ts`, `packages/compaction/compaction-basic/src/invariant.ts`.
- **`shares-code-with`** — [0041. Replay token meter service](0041-replay-token-meter-service.md): Shares source implementation: `packages/compaction/compaction-basic/src/config.ts`, `packages/compaction/compaction-basic/src/index.ts`.
- **`shares-code-with`** — [0518. Recallable compaction --- index checkpoints, a state checkpoint, and in-session history recall](0518-recallable-compaction-index-checkpoints-a-state-checkpoint-and-in-sessio.md): Shares source implementation: `packages/compaction/compaction-basic/src/index.ts`, `packages/compaction/compaction-basic/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0033-after-call-compaction-pressure-and-context-overflow-recovery.md`.
