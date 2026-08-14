---
id: "dsh-note-0050"
title: "Routed model context and compaction policy"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-20-routed-model-context-and-compaction-policy.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
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
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "defaultContextWindow"
  - "thresholdRatio"
  - "modelPolicies"
  - "retainTokens"
  - "ResolvedCompactSpec"
  - "context"
  - "LlmModelContext"
  - "contextWindow"
  - "LlmAdapter.resolveModel"
  - "LlmRuntime.resolveModelInfo"
  - "listModels"
  - "dsh-token-meter"
  - "0.8"
  - "retainRatio: 0.16"
search_regex: "(?i)(defaultContextWindow|thresholdRatio|modelPolicies|retainTokens|ResolvedCompactSpec|context|LlmModelContext|contextWindow)"
---

# 0050. Routed model context and compaction policy — implementation context

## Open this when

Compaction cannot safely apply one global context window when a process routes requests to models with different capacities. The same model id can also exist under multiple providers, and an adapter may accept dynamic ids absent from its advisory catalog. A wrong capacity either compacts too late and triggers avoidable overflow or compacts too early and discards useful context. Neither obvious configuration owner is sufficient. Compact-basic is optional and does not know which models an adapter accepts.

## Source decision

LlmAdapter.resolveModel(provider, model, signal?) returns aggregate metadata for one exact route, with optional LlmModelContext under its context field. LlmRuntime.resolveModelInfo() selects the registered route owner, validates a positive integer contextWindow, and returns detached metadata. The query is independent of listModels(): an unlisted dynamic model may have capacity metadata, and an absent context means only that the adapter cannot describe capacity. The hand-rolled DeepSeek adapter accepts optional contextWindow on each configured model plus an adapter-wide defaultContextWindow.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-20-routed-model-context-and-compaction-policy.md](../02-notes/implemented/architecture/2026-07-20-routed-model-context-and-compaction-policy.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-20-routed-model-context-and-compaction-policy.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-20-routed-model-context-and-compaction-policy.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/token-meter/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/token-meter`. | `named-package-member` |
| [`packages/llm/token-meter/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/token-meter`. | `named-package-member` |
| [`packages/llm/token-meter/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/token-meter`. | `named-package-member` |
| [`packages/llm/token-meter/src/usage-projection.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/usage-projection.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/token-meter`. Defines `contextWindow`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/token-meter`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Defines `LlmModelContext`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `context`, a construct named by the note. | `symbol-definition` |
| [`packages/compaction/compaction-basic/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/types.ts) | public types and contract | Defines `ResolvedCompactSpec`, a construct named by the note. | `symbol-definition` |
| [`packages/compaction/compaction-basic/src/config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/config.ts) | runtime implementation | Defines `modelPolicies`, a construct named by the note. Defines `thresholdRatio`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-settings-models/src/client/ProviderEditor.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ProviderEditor.tsx) | provider/backend adapter | Defines `defaultContextWindow`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/token-meter/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/README.md) | package contract and examples | Core file in the package named by the note: `packages/llm/token-meter`. | `named-package-member` |
| [`packages/llm/token-meter/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/package.json) | composition and configuration | Core file in the package named by the note: `packages/llm/token-meter`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `defaultContextWindow` | `const` | [`packages/client/ui-settings-models/src/client/ProviderEditor.tsx:342`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ProviderEditor.tsx#L342) | `const defaultContextWindow = getPath(fallback, ['defaultContextWindow'])` |
| `thresholdRatio` | `const` | [`packages/compaction/compaction-basic/src/config.ts:74`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/config.ts#L74) | `const thresholdRatio = config.thresholdRatio ?? DEFAULT_THRESHOLD_RATIO` |
| `modelPolicies` | `const` | [`packages/compaction/compaction-basic/src/config.ts:77`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/config.ts#L77) | `const modelPolicies = resolveModelPolicies(config.modelPolicies)` |
| `retainTokens` | `const` | [`packages/compaction/compaction-basic/src/config.ts:145`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/config.ts#L145) | `const retainTokens = policy.retainTokens === undefined` |
| `ResolvedCompactSpec` | `type` | [`packages/compaction/compaction-basic/src/types.ts:72`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/types.ts#L72) | `export type ResolvedCompactSpec = Omit<ResolvedTargetPolicy, 'retainRatio' \| 'retainTokens'> & {` |
| `context` | `const` | [`packages/llm/llm/src/index.ts:648`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L648) | `const context = resolved.context` |
| `LlmModelContext` | `interface` | [`packages/llm/llm/src/types.ts:247`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L247) | `export interface LlmModelContext {` |
| `contextWindow` | `const` | [`packages/llm/token-meter/src/usage-projection.ts:172`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/usage-projection.ts#L172) | `const contextWindow = event.data.contextWindow` |

### Tests and executable evidence

- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `LlmModelContext`.
- [`packages/llm/token-meter/tests/token-meter.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/tests/token-meter.spec.ts) — A test under the owning area exercises or imports `dsh-token-meter`.
- [`packages/llm/token-meter/tests/token-usage-projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/tests/token-usage-projection.spec.ts) — A test under the owning area exercises or imports `dsh-token-meter`.
- [`packages/client/ui-settings-models/tests/components.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/components.client.spec.tsx) — A test under the owning area exercises or imports `defaultContextWindow`.
- [`packages/llm/token-meter/tests/context-breakdown-projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/tests/context-breakdown-projection.spec.ts) — A test under the owning area exercises or imports `dsh-token-meter`.
- [`packages/compaction/compaction-basic/tests/compaction-basic.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/compaction-basic.spec.ts) — A test under the owning area exercises or imports `modelPolicies`. A test under the owning area exercises or imports `thresholdRatio`.
- [`packages/compaction/compaction-basic/tests/loader-composition.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/loader-composition.spec.ts) — A test under the owning area exercises or imports `modelPolicies`. A test under the owning area exercises or imports `thresholdRatio`.
- [`packages/compaction/compaction-basic/tests/compaction-loop-repro.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/compaction-loop-repro.spec.ts) — A test under the owning area exercises or imports `thresholdRatio`. A test under the owning area exercises or imports `retainTokens`.
- Source verification intent: Service tests cover detached context metadata, invalid adapter output, catalog independence, and default absence. Adapter tests cover DeepSeek exact/default/unlisted resolution, invalid capacities, and pi-ai exact descriptor resolution. Compact tests cover ratio scaling, exact provider/model overrides, load-time rejection of invalid merged ratios, runtime absolute-budget validation, same-model-id provider switches, target-specific warning suppression, and capacity-independent overflow recovery. Loader fixtures reject the removed token-meter capacity setting, and examples configure capacity on adapters.

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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/discovery-routing`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`
- Aliases: `defaultContextWindow`, `thresholdRatio`, `modelPolicies`, `retainTokens`, `ResolvedCompactSpec`, `context`, `LlmModelContext`, `contextWindow`, `LlmAdapter.resolveModel`, `LlmRuntime.resolveModelInfo`, `listModels`, `dsh-token-meter`, `0.8`, `retainRatio: 0.16`
- Regex: `(?i)(defaultContextWindow|thresholdRatio|modelPolicies|retainTokens|ResolvedCompactSpec|context|LlmModelContext|contextWindow)`

```bash
rg -n --pcre2 "(?i)(defaultContextWindow|thresholdRatio|modelPolicies|retainTokens|ResolvedCompactSpec|context|LlmModelContext|contextWindow)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0041. Replay token meter service](0041-replay-token-meter-service.md): The source note links to this decision directly.
- **`shares-code-with`** — [0076. Projected token usage and context occupancy](0076-projected-token-usage-and-context-occupancy.md): Shares source implementation: `packages/llm/token-meter/src/index.ts`, `packages/llm/token-meter/src/invariant.ts`.
- **`shares-code-with`** — [0369. Full-session stats-strip figures through a sessionStats projection](0369-full-session-stats-strip-figures-through-a-sessionstats-projection.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0033. After-call compaction pressure and context-overflow recovery](0033-after-call-compaction-pressure-and-context-overflow-recovery.md): Shares source implementation: `packages/compaction/compaction-basic/src/config.ts`, `packages/compaction/compaction-basic/src/types.ts`.
- **`shares-code-with`** — [0079. Adapter-owned max-token defaults](0079-adapter-owned-max-token-defaults.md): Shares source implementation: `packages/client/ui-settings-models/src/client/ProviderEditor.tsx`, `packages/compaction/compaction-basic/src/config.ts`.
- **`shares-code-with`** — [0037. Provider-routed LLM adapters and a generic pi-ai backend](0037-provider-routed-llm-adapters-and-a-generic-pi-ai-backend.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0050-routed-model-context-and-compaction-policy.md`.
