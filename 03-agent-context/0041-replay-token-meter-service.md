---
id: "dsh-note-0041"
title: "Replay token meter service"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-15-replay-token-meter-service.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "domain/agent-loop"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/streaming"
aliases:
  - "summarize"
  - "measure"
  - "totalTokens"
  - "modelPolicies"
  - "retainTokens"
  - "retainRatio"
  - "CompactionEngine"
  - "estimateMessage"
  - "TokenMeter"
  - "AgentOptions"
  - "dsh-compaction-basic"
  - "@deepseek-ai/dsh-token-meter"
  - "packages/llm/"
  - "ctx.tokenMeter"
search_regex: "(?i)(summarize|measure|totalTokens|modelPolicies|retainTokens|retainRatio|CompactionEngine|estimateMessage)"
---

# 0041. Replay token meter service — implementation context

## Open this when

Context pressure is useful outside compaction. A compaction backend, an overflow guard, or a future request-policy plugin can all need the same answer: how many tokens does the durable request consume? Keeping that fold inside dsh-compaction-basic duplicates replay logic, makes measurement unavailable without compaction, and encourages callers to reuse stale accounting. Provider usage is not a complete answer. It describes one successful call under one exact request envelope, while the current surface can grow, shrink, or be replaced afterward.

## Source decision

@deepseek-ai/dsh-token-meter is one concrete package under packages/llm/ and registers ctx.tokenMeter. It is not split into an interface and backend before a second implementation exists. TokenMeter itself exposes measure(session, requestHeader?) and estimateMessage(message); consumers call the singleton service directly. The service has no configuration. Estimation uses a fixed four-characters-per-token heuristic plus structural overhead. There are no model profiles, capacity settings, density settings, tokenizer backends, or language-specific strategies.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-15-replay-token-meter-service.md](../02-notes/implemented/architecture/2026-07-15-replay-token-meter-service.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-15-replay-token-meter-service.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-15-replay-token-meter-service.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/token-meter/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/llm`. Core file in the package named by the note: `packages/llm/token-meter`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/llm/token-meter/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/token-meter`. | `named-package-member` |
| [`packages/llm/token-meter/src/estimate.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/estimate.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/llm`. Core file in the package named by the note: `packages/llm/token-meter`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/llm/token-meter/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/token-meter`. | `named-package-member` |
| [`packages/compaction/compaction-basic/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/index.ts) | package entry point | Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-package-member` |
| [`packages/compaction/compaction-basic/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-package-member` |
| [`packages/compaction/compaction-basic/src/config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/config.ts) | runtime implementation | Core file in the package named by the note: `packages/compaction/compaction-basic`. Defines `modelPolicies`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/compaction/compaction-basic/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-package-member` |
| [`packages/llm/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/llm`. | `named-directory-member` |
| [`packages/llm`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/llm/token-meter`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/compaction/compaction-basic`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `summarize` | `function` | [`packages/bundle/headless/src/index.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts#L61) | `function summarize(events: readonly SessionEvent[], firstSeq: number): RunOutcome {` |
| `measure` | `const` | [`packages/client/ui-primitives/src/Toast.tsx:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Toast.tsx#L44) | `const measure = (): void => {` |
| `totalTokens` | `const` | [`packages/client/ui-subagent/src/client/SubagentCatalogAction.tsx:279`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-subagent/src/client/SubagentCatalogAction.tsx#L279) | `const totalTokens = tokenTotal(summary?.projectionValues?.tokenUsage)` |
| `modelPolicies` | `const` | [`packages/compaction/compaction-basic/src/config.ts:77`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/config.ts#L77) | `const modelPolicies = resolveModelPolicies(config.modelPolicies)` |
| `retainTokens` | `const` | [`packages/compaction/compaction-basic/src/config.ts:145`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/config.ts#L145) | `const retainTokens = policy.retainTokens === undefined` |
| `retainRatio` | `const` | [`packages/compaction/compaction-basic/src/config.ts:232`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/config.ts#L232) | `const retainRatio = config.retainRatio` |
| `retainTokens` | `const` | [`packages/compaction/compaction-basic/src/config.ts:233`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/config.ts#L233) | `const retainTokens = config.retainTokens` |
| `CompactionEngine` | `class` | [`packages/compaction/compaction/src/index.ts:96`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/index.ts#L96) | `export abstract class CompactionEngine extends Service {` |
| `estimateMessage` | `function` | [`packages/llm/token-meter/src/estimate.ts:56`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/estimate.ts#L56) | `export function estimateMessage(message: Message): number {` |
| `TokenMeter` | `class` | [`packages/llm/token-meter/src/index.ts:74`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/index.ts#L74) | `export class TokenMeter extends Service {` |
| `AgentOptions` | `interface` | [`packages/subagent/subagent/src/depth.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/depth.ts#L12) | `interface AgentOptions {` |

### Tests and executable evidence

- [`packages/llm/token-meter/tests/token-meter.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/tests/token-meter.spec.ts) — A test under the owning area exercises or imports `tokenMeter`. A test under the owning area exercises or imports `TokenMeter`.
- [`packages/compaction/compaction/tests/compaction.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/tests/compaction.spec.ts) — A test under the owning area exercises or imports `CompactionEngine`.
- [`packages/llm/token-meter/tests/token-usage-projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/tests/token-usage-projection.spec.ts) — A test under the owning area exercises or imports `tokenMeter`. A test under the owning area exercises or imports `TokenMeter`.
- [`packages/compaction/compaction-basic/tests/compaction-basic.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/compaction-basic.spec.ts) — A test under the owning area exercises or imports `dsh-compaction-basic`. A test under the owning area exercises or imports `tokenMeter`.
- [`packages/llm/token-meter/tests/context-breakdown-projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/tests/context-breakdown-projection.spec.ts) — A test under the owning area exercises or imports `tokenMeter`. A test under the owning area exercises or imports `TokenMeter`.
- [`packages/compaction/compaction-basic/tests/manual-compaction.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/manual-compaction.spec.ts) — A test under the owning area exercises or imports `dsh-compaction-basic`. A test under the owning area exercises or imports `tokenMeter`.
- [`packages/compaction/compaction-basic/tests/loader-composition.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/loader-composition.spec.ts) — A test under the owning area exercises or imports `dsh-compaction-basic`. A test under the owning area exercises or imports `TokenMeter`.
- [`packages/compaction/compaction-basic/tests/compaction-loop-repro.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/compaction-loop-repro.spec.ts) — A test under the owning area exercises or imports `dsh-compaction-basic`. A test under the owning area exercises or imports `TokenMeter`.
- Source verification intent: Unit tests cover fixed estimation, envelope invalidation and anchor replacement, replay boundaries, immutable snapshots, routed pressure, convergence, overflow generation proof, and rollback. A real Loader/Include fixture verifies the zero-config token-meter and compaction-basic load path in dependency order.

## How to read the implementation

1. Start with [`packages/llm/token-meter/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `domain/agent-loop`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/policy`, `mechanism/streaming`
- Aliases: `summarize`, `measure`, `totalTokens`, `modelPolicies`, `retainTokens`, `retainRatio`, `CompactionEngine`, `estimateMessage`, `TokenMeter`, `AgentOptions`, `dsh-compaction-basic`, `@deepseek-ai/dsh-token-meter`, `packages/llm/`, `ctx.tokenMeter`
- Regex: `(?i)(summarize|measure|totalTokens|modelPolicies|retainTokens|retainRatio|CompactionEngine|estimateMessage)`

```bash
rg -n --pcre2 "(?i)(summarize|measure|totalTokens|modelPolicies|retainTokens|retainRatio|CompactionEngine|estimateMessage)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0050. Routed model context and compaction policy](0050-routed-model-context-and-compaction-policy.md): The source note links to this decision directly.
- **`shares-code-with`** — [0033. After-call compaction pressure and context-overflow recovery](0033-after-call-compaction-pressure-and-context-overflow-recovery.md): Shares source implementation: `packages/compaction/compaction-basic/src/config.ts`, `packages/compaction/compaction-basic/src/index.ts`.
- **`shares-code-with`** — [0076. Projected token usage and context occupancy](0076-projected-token-usage-and-context-occupancy.md): Shares source implementation: `packages/llm/token-meter/src/index.ts`, `packages/llm/token-meter/src/invariant.ts`.
- **`shares-code-with`** — [0215. Queued manual compaction with one durable lock](0215-queued-manual-compaction-with-one-durable-lock.md): Shares source implementation: `packages/compaction/compaction-basic/src/index.ts`, `packages/compaction/compaction-basic/src/invariant.ts`.
- **`shares-code-with`** — [0343. the context meter could not see a compaction](0343-the-context-meter-could-not-see-a-compaction.md): Shares source implementation: `packages/compaction/compaction-basic/src/index.ts`, `packages/compaction/compaction-basic/src/invariant.ts`.
- **`shares-code-with`** — [0518. Recallable compaction --- index checkpoints, a state checkpoint, and in-session history recall](0518-recallable-compaction-index-checkpoints-a-state-checkpoint-and-in-sessio.md): Shares source implementation: `packages/compaction/compaction-basic/src/index.ts`, `packages/compaction/compaction-basic/src/types.ts`.
- **`shares-code-with`** — [0369. Full-session stats-strip figures through a sessionStats projection](0369-full-session-stats-strip-figures-through-a-sessionstats-projection.md): Shares source implementation: `packages/llm/token-meter/src/index.ts`, `packages/llm/token-meter/src/invariant.ts`.
- **`shares-code-with`** — [0304. The summarization call replays the conversation prefix for KV-cache reuse](0304-the-summarization-call-replays-the-conversation-prefix-for-kv-cache-reus.md): Shares source implementation: `packages/compaction/compaction-basic/src/index.ts`, `packages/compaction/compaction-basic/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0041-replay-token-meter-service.md`.
