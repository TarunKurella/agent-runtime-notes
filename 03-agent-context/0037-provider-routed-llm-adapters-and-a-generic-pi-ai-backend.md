---
id: "dsh-note-0037"
title: "Provider-routed LLM adapters and a generic pi-ai backend"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-14-provider-routed-llm-adapters.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "finish"
  - "apiKey"
  - "streamIdleTimeoutMs"
  - "PiAiAdapter"
  - "model"
  - "baseUrl"
  - "BlockAssembler"
  - "LlmCallConfig"
  - "LlmRuntime"
  - "retryPolicy"
  - "provider"
  - "stream"
  - "AssistantMessage"
  - "maxRetries"
search_regex: "(?i)(finish|apiKey|streamIdleTimeoutMs|PiAiAdapter|model|baseUrl|BlockAssembler|LlmCallConfig)"
---

# 0037. Provider-routed LLM adapters and a generic pi-ai backend — implementation context

## Open this when

dsh-llm registered adapters by exact model name. A plugin supplied a model list at Cordis startup, LlmRuntime stored one adapter per listed string, and GenerateOptions.model selected the adapter and the provider model at once. This worked while both shipping adapters targeted the same two DeepSeek models, but it conflated two independent decisions: which upstream provider owns a request, and which model that provider should run. The conflation prevents a provider gateway from serving an open-ended model catalog.

## Source decision

GenerateOptions and LlmCallConfig carry provider: string beside model: string; AgentOptions carries the corresponding optional creation field. A loop request is valid only after both values are non-empty, and both values are part of the logged request header. agent/request may return a replacement pair on any step, so a session can switch providers and models without changing the Cordis plugin lifecycle. LlmRuntime registers and resolves adapters by provider.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-14-provider-routed-llm-adapters.md](../02-notes/implemented/architecture/2026-07-14-provider-routed-llm-adapters.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-14-provider-routed-llm-adapters.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-14-provider-routed-llm-adapters.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. Defines `LlmRuntime`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. Defines `GenerateOptions`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/message.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/message.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm`. Defines `AssistantMessage`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/call-config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/call-config.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm`. Defines `LlmCallConfig`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-pi-ai/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-pi-ai`. | `named-package-member` |
| [`packages/llm/llm-retry/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-retry`. | `named-package-member` |
| [`packages/llm/llm-retry/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm-retry`. | `named-package-member` |
| [`packages/llm/llm/src/retry-policy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/retry-policy.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm`. Defines `maxRetries`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-pi-ai/src/config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/config.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm-pi-ai`. Defines `streamIdleTimeoutMs`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-pi-ai/src/adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts) | provider/backend adapter | Core file in the package named by the note: `packages/llm/llm-pi-ai`. Defines `model`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-pi-ai/src/catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm-pi-ai`. Defines `baseUrl`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `finish` | `const` | [`packages/core/tools/src/ts-types.ts:115`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/ts-types.ts#L115) | `const finish = (document: TypeDocument): void => {` |
| `apiKey` | `const` | [`packages/llm/llm-deepseek/src/adapter.ts:221`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/adapter.ts#L221) | `const apiKey = await this.config.resolveApiKey(connection)` |
| `streamIdleTimeoutMs` | `const` | [`packages/llm/llm-deepseek/src/index.ts:175`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts#L175) | `const streamIdleTimeoutMs = config.streamIdleTimeoutMs ?? DEFAULT_STREAM_IDLE_TIMEOUT_MS` |
| `PiAiAdapter` | `class` | [`packages/llm/llm-pi-ai/src/adapter.ts:186`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts#L186) | `export class PiAiAdapter extends LlmAdapter {` |
| `model` | `const` | [`packages/llm/llm-pi-ai/src/adapter.ts:287`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts#L287) | `const model = this.modelOf(snapshot, options.provider, options.model)` |
| `apiKey` | `const` | [`packages/llm/llm-pi-ai/src/adapter.ts:292`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts#L292) | `const apiKey = await this.config.resolveApiKey(options.provider, profile)` |
| `streamIdleTimeoutMs` | `const` | [`packages/llm/llm-pi-ai/src/adapter.ts:298`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts#L298) | `const streamIdleTimeoutMs = profile.streamIdleTimeoutMs` |
| `baseUrl` | `const` | [`packages/llm/llm-pi-ai/src/catalog.ts:502`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts#L502) | `const baseUrl = request.baseURL ?? base?.baseUrl ?? providerBaseUrl` |
| `streamIdleTimeoutMs` | `const` | [`packages/llm/llm-pi-ai/src/config.ts:318`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/config.ts#L318) | `const streamIdleTimeoutMs = source.streamIdleTimeoutMs ?? DEFAULT_STREAM_IDLE_TIMEOUT_MS` |
| `apiKey` | `const` | [`packages/llm/llm-pi-ai/src/discovery.ts:241`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/discovery.ts#L241) | `const apiKey = supplied === undefined ? undefined : usableProbeKey(supplied)` |
| `baseUrl` | `const` | [`packages/llm/llm-pi-ai/src/provider.ts:147`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/provider.ts#L147) | `const baseUrl = spec.baseURL ?? base.baseUrl` |
| `BlockAssembler` | `class` | [`packages/llm/llm/src/assembler.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/assembler.ts#L36) | `export class BlockAssembler {` |
| `LlmCallConfig` | `interface` | [`packages/llm/llm/src/call-config.ts:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/call-config.ts#L23) | `export interface LlmCallConfig {` |
| `LlmRuntime` | `class` | [`packages/llm/llm/src/index.ts:284`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L284) | `export class LlmRuntime extends Service {` |
| `retryPolicy` | `const` | [`packages/llm/llm/src/index.ts:387`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L387) | `const retryPolicy = adapter.providerRetryPolicy(provider)` |
| `provider` | `const` | [`packages/llm/llm/src/index.ts:632`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L632) | `const provider = registration.provider.id` |

### Tests and executable evidence

- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `GenerateOptions`. A test under the owning area exercises or imports `registerAdapter`.
- [`packages/llm/llm/tests/topology.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/topology.spec.ts) — A test under the owning area exercises or imports `GenerateOptions`. A test under the owning area exercises or imports `registerAdapter`.
- [`packages/llm/llm/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/invariant.spec.ts) — A test under the owning area exercises or imports `GenerateOptions`. A test under the owning area exercises or imports `registerAdapter`.
- [`packages/llm/llm/tests/assembler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/assembler.spec.ts) — A test under the owning area exercises or imports `BlockAssembler`.
- [`packages/llm/llm-pi-ai/tests/assemble.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/assemble.ts) — A test under the owning area exercises or imports `BlockAssembler`.
- [`packages/llm/llm/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/properties.spec.ts) — A test under the owning area exercises or imports `BlockAssembler`.
- [`packages/llm/llm-retry/tests/retry.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/tests/retry.spec.ts) — A test under the owning area exercises or imports `LlmRuntime`. A test under the owning area exercises or imports `GenerateOptions`.
- [`packages/llm/llm/tests/call-config.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/call-config.spec.ts) — A test under the owning area exercises or imports `GenerateOptions`. A test under the owning area exercises or imports `LlmCallConfig`.
- Source verification intent: Unit coverage exercises registry conflicts, request reconstruction, session validation, profile resolution, single-attempt option forwarding, native API selection including OpenAI Responses, conversion, replay validation, error mapping, caller cancellation, idle-timeout transport termination, content rewrites, and same-instance versus different-instance replay dispatch.

## How to read the implementation

1. Start with [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `finish`, `apiKey`, `streamIdleTimeoutMs`, `PiAiAdapter`, `model`, `baseUrl`, `BlockAssembler`, `LlmCallConfig`, `LlmRuntime`, `retryPolicy`, `provider`, `stream`, `AssistantMessage`, `maxRetries`
- Regex: `(?i)(finish|apiKey|streamIdleTimeoutMs|PiAiAdapter|model|baseUrl|BlockAssembler|LlmCallConfig)`

```bash
rg -n --pcre2 "(?i)(finish|apiKey|streamIdleTimeoutMs|PiAiAdapter|model|baseUrl|BlockAssembler|LlmCallConfig)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0025. Every LLM request is reconstructable from the session log](0025-every-llm-request-is-reconstructable-from-the-session-log.md): The source note links to this decision directly.
- **`source-link`** — [0039. Advisory LLM catalogs and per-session ACP model selection](0039-advisory-llm-catalogs-and-per-session-acp-model-selection.md): The source note links to this decision directly.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/types.ts`.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/types.ts`.
- **`shares-code-with`** — [0181. Per-provider request retry policies](0181-per-provider-request-retry-policies.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/types.ts`.
- **`shares-code-with`** — [0307. Empty model completions are retryable EMPTY_RESPONSE failures](0307-empty-model-completions-are-retryable-empty-response-failures.md): Shares source implementation: `packages/llm/llm-pi-ai/src/index.ts`, `packages/llm/llm-retry/src/index.ts`.
- **`shares-code-with`** — [0077. request-level LLM configuration and the credential seam](0077-request-level-llm-configuration-and-the-credential-seam.md): Shares source implementation: `packages/llm/llm-pi-ai/src/config.ts`, `packages/llm/llm-pi-ai/src/index.ts`.
- **`shares-code-with`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0037-provider-routed-llm-adapters-and-a-generic-pi-ai-backend.md`.
