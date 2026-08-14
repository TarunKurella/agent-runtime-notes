---
id: "dsh-note-0203"
title: "SDK max output tokens"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-28-sdk-max-output-tokens.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/generation"
  - "mechanism/policy"
aliases:
  - "maxTokens"
  - "LlmCallConfig"
  - "AgentOptions"
  - "initialize"
  - "GenerateOptions.maxTokens"
  - "compaction-basic.maxTokens"
  - "max_tokens"
  - "AgentOptions.maxTokens"
  - "SubagentStartRequest.agentOptions.maxTokens"
  - "dsh-tool-subagent"
  - "subagent-dsh-sdk"
  - "maxTokensAsSuccess"
  - "session/prompt"
  - "max-tokens"
search_regex: "(?i)(maxTokens|LlmCallConfig|AgentOptions|initialize|GenerateOptions\\.maxTokens|compaction\\-basic\\.maxTokens|max_tokens|AgentOptions\\.maxTokens)"
---

# 0203. SDK max output tokens — implementation context

## Open this when

The Python and TypeScript SDKs could select a provider and model but could not bound conversation-model output. The runtime therefore omitted GenerateOptions.maxTokens, leaving provider defaults in control even when an evaluation host required a fixed output budget. compaction-basic.maxTokens could not fill this role because it limits only compaction-summary calls.

## Source decision

The high-level SDKs expose one optional process-wide output cap: Python names it max_tokens, TypeScript names it maxTokens, and the shared initialize wire payload carries maxTokens. The JSON-RPC server rejects values that are not positive safe integers and stores the accepted cap with its provider/model route. Each SDK-created root Agent receives the cap through AgentOptions.maxTokens.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-28-sdk-max-output-tokens.md](../02-notes/implemented/feature/2026-07-28-sdk-max-output-tokens.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-28-sdk-max-output-tokens.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-28-sdk-max-output-tokens.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/subagent/tool-subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/subagent/subagent-dsh-sdk/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-dsh-sdk/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/subagent-dsh-sdk`. | `named-package-member` |
| [`packages/subagent/tool-subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/subagent/subagent-dsh-sdk/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-dsh-sdk/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/subagent-dsh-sdk`. | `named-package-member` |
| [`packages/subagent/tool-subagent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/subagent/subagent-dsh-sdk`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-dsh-sdk) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/llm/src/call-config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/call-config.ts) | runtime implementation | Defines `LlmCallConfig`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent-loop/src/agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts) | runtime implementation | Defines `maxTokens`, a construct named by the note. | `symbol-definition` |
| [`packages/subagent/subagent/src/depth.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/depth.ts) | runtime implementation | Defines `AgentOptions`, a construct named by the note. | `symbol-definition` |
| [`python/sdk/src/deepseek_harness/client.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/src/deepseek_harness/client.py) | runtime implementation | Defines `initialize`, a construct named by the note. Contains the exact code literal `session/prompt` named by the note. | `exact-code-occurrence, symbol-definition` |
| [`packages/subagent/tool-subagent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/README.md) | package contract and examples | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/subagent/subagent-dsh-sdk/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-dsh-sdk/README.md) | package contract and examples | Core file in the package named by the note: `packages/subagent/subagent-dsh-sdk`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `maxTokens` | `const` | [`packages/core/agent-loop/src/agent.ts:427`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L427) | `const maxTokens = this.options.maxTokens` |
| `LlmCallConfig` | `interface` | [`packages/llm/llm/src/call-config.ts:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/call-config.ts#L23) | `export interface LlmCallConfig {` |
| `AgentOptions` | `interface` | [`packages/subagent/subagent/src/depth.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/depth.ts#L12) | `interface AgentOptions {` |
| `initialize` | `def` | [`python/sdk/src/deepseek_harness/client.py:117`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/src/deepseek_harness/client.py#L117) | `def initialize(` |

### Tests and executable evidence

- [`python/sdk/tests/test_client.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_client.py) — A test under the owning area exercises or imports `initialize`. Contains the exact code literal `session/prompt` named by the note.
- [`python/sdk/tests/test_bundled_runtime.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_bundled_runtime.py) — A test under the owning area exercises or imports `initialize`.
- [`packages/llm/llm/tests/call-config.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/call-config.spec.ts) — A test under the owning area exercises or imports `LlmCallConfig`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `maxTokens`.
- [`packages/core/agent-loop/tests/request-reconstruction.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/request-reconstruction.spec.ts) — A test under the owning area exercises or imports `maxTokens`.
- [`packages/subagent/subagent-dsh-sdk/tests/subagent-dsh-sdk.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-dsh-sdk/tests/subagent-dsh-sdk.spec.ts) — A test under the owning area exercises or imports `subagent-dsh-sdk`.
- [`packages/subagent/subagent-dsh-sdk/tests/loader-composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-dsh-sdk/tests/loader-composition.e2e.ts) — A test under the owning area exercises or imports `subagent-dsh-sdk`.

## How to read the implementation

1. Start with [`packages/subagent/tool-subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/generation`, `mechanism/policy`
- Aliases: `maxTokens`, `LlmCallConfig`, `AgentOptions`, `initialize`, `GenerateOptions.maxTokens`, `compaction-basic.maxTokens`, `max_tokens`, `AgentOptions.maxTokens`, `SubagentStartRequest.agentOptions.maxTokens`, `dsh-tool-subagent`, `subagent-dsh-sdk`, `maxTokensAsSuccess`, `session/prompt`, `max-tokens`
- Regex: `(?i)(maxTokens|LlmCallConfig|AgentOptions|initialize|GenerateOptions\.maxTokens|compaction\-basic\.maxTokens|max_tokens|AgentOptions\.maxTokens)`

```bash
rg -n --pcre2 "(?i)(maxTokens|LlmCallConfig|AgentOptions|initialize|GenerateOptions\\.maxTokens|compaction\\-basic\\.maxTokens|max_tokens|AgentOptions\\.maxTokens)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0153. Configure subagent persona, tool visibility, and depth](0153-configure-subagent-persona-tool-visibility-and-depth.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.
- **`shares-code-with`** — [0290. Background job completion wakes an idle owner](0290-background-job-completion-wakes-an-idle-owner.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/subagent/tool-subagent/src/index.ts`.
- **`shares-code-with`** — [0148. Background subagent tasks](0148-background-subagent-tasks.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.
- **`shares-code-with`** — [0120. Product subagent providers live in the shared profile host](0120-product-subagent-providers-live-in-the-shared-profile-host.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.
- **`shares-code-with`** — [0462. Merge subagent control into the subagent service](0462-merge-subagent-control-into-the-subagent-service.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.
- **`shares-code-with`** — [0249. Claude Code and Codex subagent backends](0249-claude-code-and-codex-subagent-backends.md): Shares source implementation: `packages/subagent/tool-subagent/src/index.ts`, `packages/subagent/tool-subagent/src/invariant.ts`.
- **`shares-code-with`** — [0289. Continuable delegation is background-first](0289-continuable-delegation-is-background-first.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.
- **`shares-code-with`** — [0026. Subagent provider-lifecycle events --- `subagent/provider-added` / `subagent/provider-removed`](0026-subagent-provider-lifecycle-events-subagent-provider-added-subagent-prov.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0203-sdk-max-output-tokens.md`.
