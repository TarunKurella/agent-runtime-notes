---
id: "dsh-note-0181"
title: "Per-provider request retry policies"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-24-provider-retry-policies.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/performance"
  - "concern/recovery"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "retryPolicy"
  - "llm"
  - "initialDelayMs"
  - "maxDelayMs"
  - "jitterRatio"
  - "maxRetries"
  - "retryableCodes"
  - "agent/request"
  - "ctx.llm"
  - "@deepseek-ai/dsh-llm-retry"
  - "request/header"
  - "[1 - jitterRatio, 1 + jitterRatio]"
  - "Retry-After"
  - "llm/retry"
search_regex: "(?i)(retryPolicy|initialDelayMs|maxDelayMs|jitterRatio|maxRetries|retryableCodes|agent/request|ctx\\.llm)"
---

# 0181. Per-provider request retry policies — implementation context

## Open this when

One process may route model requests to providers with different reliability and cost constraints. A single transient classifier and finite retry budget cannot express a deployment that wants bounded recovery for most providers but requires one provider to keep retrying every model-request failure until the request succeeds or the caller cancels it. Provider policy must follow the request that actually failed, including a route selected by agent/request, rather than the agent's initial options.

## Source decision

Each concrete adapter accepts an optional retryPolicy inside its provider configuration. The adapter validates and resolves the policy, and ctx.llm captures it when that exact provider route registers. When a call enters its final adapter boundary, ctx.llm binds the serving registration's immutable policy to that call; the agent loop passes it to closed-step recovery even if the route is disposed or replaced while the request is in flight. @deepseek-ai/dsh-llm-retry combines that call-local policy with the failed step's durable provider identity.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-24-provider-retry-policies.md](../02-notes/implemented/feature/2026-07-24-provider-retry-policies.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-24-provider-retry-policies.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-24-provider-retry-policies.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. Defines `retryPolicy`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. Defines `llm`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-retry/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-retry`. | `named-package-member` |
| [`packages/llm/llm-retry/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm-retry`. | `named-package-member` |
| [`packages/llm/llm/src/retry-policy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/retry-policy.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm`. Defines `retryableCodes`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-retry/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm-retry`. | `named-package-member` |
| [`packages/llm/llm`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/llm-retry`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/llm/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/README.md) | package contract and examples | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/package.json) | composition and configuration | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm-retry/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/README.md) | package contract and examples | Core file in the package named by the note: `packages/llm/llm-retry`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `retryPolicy` | `const` | [`packages/llm/llm/src/index.ts:387`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L387) | `const retryPolicy = adapter.providerRetryPolicy(provider)` |
| `llm` | `const` | [`packages/llm/llm/src/invariant.ts:92`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts#L92) | `const llm = ctx.get('llm')` |
| `initialDelayMs` | `const` | [`packages/llm/llm/src/retry-policy.ts:119`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/retry-policy.ts#L119) | `const initialDelayMs = config?.initialDelayMs ?? DEFAULT_INITIAL_DELAY_MS` |
| `maxDelayMs` | `const` | [`packages/llm/llm/src/retry-policy.ts:120`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/retry-policy.ts#L120) | `const maxDelayMs = config?.maxDelayMs ?? DEFAULT_MAX_DELAY_MS` |
| `jitterRatio` | `const` | [`packages/llm/llm/src/retry-policy.ts:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/retry-policy.ts#L121) | `const jitterRatio = config?.jitterRatio ?? DEFAULT_JITTER_RATIO` |
| `maxRetries` | `const` | [`packages/llm/llm/src/retry-policy.ts:161`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/retry-policy.ts#L161) | `const maxRetries = config.maxRetries ?? DEFAULT_MAX_RETRIES` |
| `retryableCodes` | `const` | [`packages/llm/llm/src/retry-policy.ts:162`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/retry-policy.ts#L162) | `const retryableCodes = config.retryableCodes ?? [...DEFAULT_RETRYABLE_CODES]` |

### Tests and executable evidence

- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `retryPolicy`. A test under the owning area exercises or imports `maxRetries`.
- [`packages/llm/llm-retry/tests/retry.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/tests/retry.spec.ts) — A test under the owning area exercises or imports `retryPolicy`. A test under the owning area exercises or imports `retryableCodes`.
- [`packages/llm/llm/tests/retry-policy.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/retry-policy.spec.ts) — A test under the owning area exercises or imports `retryPolicy`. A test under the owning area exercises or imports `retryableCodes`.
- [`packages/llm/llm-retry/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/tests/invariant.spec.ts) — A test under the owning area exercises or imports `maxRetries`. A test under the owning area exercises or imports `dsh-llm-retry`.
- [`packages/llm/llm-retry/tests/persistence.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/tests/persistence.spec.ts) — A test under the owning area exercises or imports `dsh-llm-retry`.
- [`packages/llm/llm-retry/tests/transport-recovery.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/tests/transport-recovery.spec.ts) — A test under the owning area exercises or imports `retryPolicy`. A test under the owning area exercises or imports `maxRetries`.
- [`packages/llm/llm-retry/tests/loader-composition.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/tests/loader-composition.spec.ts) — A test under the owning area exercises or imports `retryPolicy`. A test under the owning area exercises or imports `retryableCodes`.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — Contains the exact code literal `llm/retry` named by the note.
- Source verification intent: Adapter tests validate nested policies at provider load, prove registration captures configured and default policies, and retain the serving policy across in-flight route replacement. Unit tests select policies from the failed request's serving registration, separate provider and changed-policy histories, exercise always mode beyond the normal budget, pin jitter and delay caps, prove downstream recovery ordering, prove cancellation and disposal drain delegated recovery before reaching quiescence, and prove both abort active backoff waits.

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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/performance`, `concern/recovery`, `domain/agent-loop`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `retryPolicy`, `llm`, `initialDelayMs`, `maxDelayMs`, `jitterRatio`, `maxRetries`, `retryableCodes`, `agent/request`, `ctx.llm`, `@deepseek-ai/dsh-llm-retry`, `request/header`, `[1 - jitterRatio, 1 + jitterRatio]`, `Retry-After`, `llm/retry`
- Regex: `(?i)(retryPolicy|initialDelayMs|maxDelayMs|jitterRatio|maxRetries|retryableCodes|agent/request|ctx\.llm)`

```bash
rg -n --pcre2 "(?i)(retryPolicy|initialDelayMs|maxDelayMs|jitterRatio|maxRetries|retryableCodes|agent/request|ctx\\.llm)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): The source note links to this decision directly.
- **`shares-code-with`** — [0037. Provider-routed LLM adapters and a generic pi-ai backend](0037-provider-routed-llm-adapters-and-a-generic-pi-ai-backend.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/types.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/types.ts`.
- **`shares-code-with`** — [0307. Empty model completions are retryable EMPTY_RESPONSE failures](0307-empty-model-completions-are-retryable-empty-response-failures.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/types.ts`.
- **`shares-code-with`** — [0056. Adapter-owned reasoning effort capabilities](0056-adapter-owned-reasoning-effort-capabilities.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0669. TUI titles come from the session-title service](0669-tui-titles-come-from-the-session-title-service.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0567. Dedicated full-screen TUI front door](0567-dedicated-full-screen-tui-front-door.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0623. TUI model-context resolution defers on the adapter-registration race](0623-tui-model-context-resolution-defers-on-the-adapter-registration-race.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0181-per-provider-request-retry-policies.md`.
