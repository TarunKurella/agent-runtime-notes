---
id: "dsh-note-0079"
title: "Adapter-owned max-token defaults"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-30-adapter-owned-max-token-defaults.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/streaming"
aliases:
  - "reasoningEffort"
  - "maxTokens"
  - "defaultMaxTokens"
  - "LlmRuntime"
  - "GenerateOptions.maxTokens"
  - "request/header"
  - "LlmResolvedModelInfo.defaultMaxTokens"
  - "LlmCallConfig.maxTokens"
  - "agent/request"
  - "LlmRuntime.stream"
  - "max_tokens"
  - "AgentOptions.maxTokens"
  - "max_tokens: 256000"
  - "llm-deepseek.config.maxTokens"
search_regex: "(?i)(reasoningEffort|maxTokens|defaultMaxTokens|LlmRuntime|GenerateOptions\\.maxTokens|request/header|LlmResolvedModelInfo\\.defaultMaxTokens|LlmCallConfig\\.maxTokens)"
---

# 0079. Adapter-owned max-token defaults — implementation context

## Open this when

An LLM adapter could serialize an explicit GenerateOptions.maxTokens, but its Cordis configuration could not establish a reconstructable conversation default. Applying a fallback only inside provider serialization would make the wire request differ from the durable request/header; putting every provider's default in Agent Loop would instead transfer deployment and model policy into the provider-neutral driver.

## Source decision

LlmResolvedModelInfo.defaultMaxTokens carries an optional adapter-configured per-request output cap for one exact provider/model route. LlmRuntime validates it as a positive safe integer and materializes it into LlmCallConfig.maxTokens only when the caller omitted a value. A prepared call identifies materialized maxTokens and reasoningEffort fields as adapter defaults; explicit request or Agent options remain unmarked and therefore win without clamping.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-30-adapter-owned-max-token-defaults.md](../02-notes/implemented/architecture/2026-07-30-adapter-owned-max-token-defaults.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-30-adapter-owned-max-token-defaults.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-30-adapter-owned-max-token-defaults.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `defaultMaxTokens`, a construct named by the note. Defines `LlmRuntime`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Defines `reasoningEffort`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent-loop/src/agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts) | runtime implementation | Defines `maxTokens`, a construct named by the note. Defines `reasoningEffort`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-pi-ai/src/catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts) | runtime implementation | Defines `maxTokens`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-pi-ai/src/discovery.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/discovery.ts) | runtime implementation | Defines `maxTokens`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-deepseek/src/serialize.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/serialize.ts) | runtime implementation | Defines `reasoningEffort`, a construct named by the note. | `symbol-definition` |
| [`packages/compaction/compaction-basic/src/config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/config.ts) | runtime implementation | Defines `maxTokens`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-model-selection/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/client/index.ts) | package entry point | Defines `reasoningEffort`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-settings-models/src/client/ProviderEditor.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ProviderEditor.tsx) | provider/backend adapter | Defines `defaultMaxTokens`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-settings-models/src/client/DeepSeekModelsEditor.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/DeepSeekModelsEditor.tsx) | runtime implementation | Defines `maxTokens`, a construct named by the note. | `symbol-definition` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `agent/request` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `agent/request` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `reasoningEffort` | `const` | [`packages/client/ui-model-selection/src/client/index.ts:83`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/client/index.ts#L83) | `const reasoningEffort = sameRoute` |
| `maxTokens` | `const` | [`packages/client/ui-settings-models/src/client/DeepSeekModelsEditor.tsx:116`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/DeepSeekModelsEditor.tsx#L116) | `const maxTokens = model['maxTokens']` |
| `defaultMaxTokens` | `const` | [`packages/client/ui-settings-models/src/client/ProviderEditor.tsx:343`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ProviderEditor.tsx#L343) | `const defaultMaxTokens = getPath(fallback, ['maxTokens'])` |
| `maxTokens` | `const` | [`packages/compaction/compaction-basic/src/config.ts:234`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/config.ts#L234) | `const maxTokens = config.maxTokens` |
| `reasoningEffort` | `const` | [`packages/core/agent-loop/src/agent.ts:422`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L422) | `const reasoningEffort = persistedConfig?.provider === route.provider` |
| `maxTokens` | `const` | [`packages/core/agent-loop/src/agent.ts:427`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L427) | `const maxTokens = this.options.maxTokens` |
| `reasoningEffort` | `const` | [`packages/core/session/src/index.ts:266`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L266) | `const reasoningEffort = configRecord['reasoningEffort']` |
| `reasoningEffort` | `function` | [`packages/llm/llm-deepseek/src/serialize.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/serialize.ts#L26) | `function reasoningEffort(effort: NonNullable<GenerateOptions['reasoningEffort']>): 'off' \| 'high' \| 'max' {` |
| `maxTokens` | `const` | [`packages/llm/llm-pi-ai/src/catalog.ts:514`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts#L514) | `const maxTokens = entry.maxTokens ?? base?.maxTokens ?? request.defaultMaxTokens` |
| `maxTokens` | `const` | [`packages/llm/llm-pi-ai/src/discovery.ts:153`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/discovery.ts#L153) | `const maxTokens = capacity(entry?.max_output_tokens, entry?.max_tokens)` |
| `LlmRuntime` | `class` | [`packages/llm/llm/src/index.ts:284`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L284) | `export class LlmRuntime extends Service {` |
| `defaultMaxTokens` | `const` | [`packages/llm/llm/src/index.ts:658`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L658) | `const defaultMaxTokens = resolved.defaultMaxTokens` |

### Tests and executable evidence

- [`python/sdk/tests/test_client.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_client.py) — A test under the owning area exercises or imports `max_tokens`.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `max_tokens`.
- [`examples/acp-agent/tests/acp.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/acp.e2e.ts) — A test under the owning area exercises or imports `max_tokens`.
- [`packages/acp/acp/tests/codec.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/codec.spec.ts) — A test under the owning area exercises or imports `max_tokens`.
- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `defaultMaxTokens`.
- [`packages/sdk/server/tests/server.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/tests/server.spec.ts) — A test under the owning area exercises or imports `max_tokens`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `maxTokens`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `reasoningEffort`.

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
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/storage`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/streaming`
- Aliases: `reasoningEffort`, `maxTokens`, `defaultMaxTokens`, `LlmRuntime`, `GenerateOptions.maxTokens`, `request/header`, `LlmResolvedModelInfo.defaultMaxTokens`, `LlmCallConfig.maxTokens`, `agent/request`, `LlmRuntime.stream`, `max_tokens`, `AgentOptions.maxTokens`, `max_tokens: 256000`, `llm-deepseek.config.maxTokens`
- Regex: `(?i)(reasoningEffort|maxTokens|defaultMaxTokens|LlmRuntime|GenerateOptions\.maxTokens|request/header|LlmResolvedModelInfo\.defaultMaxTokens|LlmCallConfig\.maxTokens)`

```bash
rg -n --pcre2 "(?i)(reasoningEffort|maxTokens|defaultMaxTokens|LlmRuntime|GenerateOptions\\.maxTokens|request/header|LlmResolvedModelInfo\\.defaultMaxTokens|LlmCallConfig\\.maxTokens)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0025. Every LLM request is reconstructable from the session log](0025-every-llm-request-is-reconstructable-from-the-session-log.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0050. Routed model context and compaction policy](0050-routed-model-context-and-compaction-policy.md): Shares source implementation: `packages/client/ui-settings-models/src/client/ProviderEditor.tsx`, `packages/compaction/compaction-basic/src/config.ts`.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0326. The browser conversation is a log-ordered human transcript](0326-the-browser-conversation-is-a-log-ordered-human-transcript.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0078. Terminal LLM stream failures](0078-terminal-llm-stream-failures.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0128. A pi-ai model declares its own input modalities, and undeclared means text](0128-a-pi-ai-model-declares-its-own-input-modalities-and-undeclared-means-tex.md): Shares source implementation: `packages/llm/llm-pi-ai/src/catalog.ts`, `packages/llm/llm-pi-ai/src/discovery.ts`.
- **`shares-code-with`** — [0522. Architectural conformance --- dependency rules and the adapter kit](0522-architectural-conformance-dependency-rules-and-the-adapter-kit.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/core/session/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0079-adapter-owned-max-token-defaults.md`.
