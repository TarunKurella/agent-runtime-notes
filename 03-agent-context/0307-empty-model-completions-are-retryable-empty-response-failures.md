---
id: "dsh-note-0307"
title: "Empty model completions are retryable EMPTY_RESPONSE failures"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-24-empty-model-response-is-retryable.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/lifecycle"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/agent-loop"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/streaming"
aliases:
  - "mapStopReason"
  - "BlockAssembler"
  - "CONTEXT_WINDOW_EXCEEDED_CODE"
  - "QUOTA_EXCEEDED_CODE"
  - "EMPTY_RESPONSE_CODE"
  - "completed"
  - "retryableCodes"
  - "finishError"
  - "assistant/message"
  - "dsh-llm"
  - "EMPTY_RESPONSE"
  - "dsh-llm-pi-ai"
  - "dsh-llm-deepseek"
  - "[DONE]"
search_regex: "(?i)(mapStopReason|BlockAssembler|CONTEXT_WINDOW_EXCEEDED_CODE|QUOTA_EXCEEDED_CODE|EMPTY_RESPONSE_CODE|completed|retryableCodes|finishError)"
---

# 0307. Empty model completions are retryable EMPTY_RESPONSE failures — implementation context

## Open this when

Providers occasionally return a degenerate completion: a well-formed stream that carries a terminal stop finish and zero content blocks --- no text, no reasoning, no tool calls. If an adapter maps this shape to a successful {kind: 'stop'} finish, the loop logs an empty assistant/message and ends the turn as completed. Retry never runs, no failure reaches the caller, and a driver such as goal-round-driver consumes a round without progress.

## Source decision

An adapter classifies a completed empty response as a provider-boundary failure, and retry policy treats it as transient: dsh-llm exports the canonical code EMPTY_RESPONSE_CODE ('EMPTY_RESPONSE') beside CONTEXT_WINDOW_EXCEEDED_CODE/QUOTA_EXCEEDED_CODE. dsh-llm-pi-ai (mapStopReason): a terminal stop whose assistant message has no content blocks becomes a finish {kind: 'error'} with that code. Context-overflow detection still wins where it applies (it is checked first and is the more actionable classification).

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-24-empty-model-response-is-retryable.md](../02-notes/implemented/bug-fix/2026-07-24-empty-model-response-is-retryable.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-24-empty-model-response-is-retryable.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-24-empty-model-response-is-retryable.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`examples/acp-agent/retry.cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/retry.cordis.yml) | composition and configuration | The source note names this file directly. | `named-file` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. Defines `completed`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/error.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm`. Defines `EMPTY_RESPONSE_CODE`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/assembler.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/assembler.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm`. Defines `BlockAssembler`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-pi-ai/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-pi-ai`. | `named-package-member` |
| [`packages/llm/llm-retry/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-retry`. | `named-package-member` |
| [`packages/llm/llm-retry/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm-retry`. | `named-package-member` |
| [`packages/llm/llm/src/retry-policy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/retry-policy.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm`. Defines `retryableCodes`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-pi-ai/src/stream.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/stream.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm-pi-ai`. Defines `mapStopReason`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `mapStopReason` | `function` | [`packages/llm/llm-pi-ai/src/stream.ts:73`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/stream.ts#L73) | `export function mapStopReason(message: AssistantMessage, contextWindow?: number): FinishReason {` |
| `BlockAssembler` | `class` | [`packages/llm/llm/src/assembler.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/assembler.ts#L36) | `export class BlockAssembler {` |
| `CONTEXT_WINDOW_EXCEEDED_CODE` | `const` | [`packages/llm/llm/src/error.ts:25`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts#L25) | `export const CONTEXT_WINDOW_EXCEEDED_CODE = 'CONTEXT_WINDOW_EXCEEDED'` |
| `QUOTA_EXCEEDED_CODE` | `const` | [`packages/llm/llm/src/error.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts#L28) | `export const QUOTA_EXCEEDED_CODE = 'QUOTA'` |
| `EMPTY_RESPONSE_CODE` | `const` | [`packages/llm/llm/src/error.ts:39`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts#L39) | `export const EMPTY_RESPONSE_CODE = 'EMPTY_RESPONSE'` |
| `completed` | `let` | [`packages/llm/llm/src/index.ts:872`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L872) | `let completed = false` |
| `retryableCodes` | `const` | [`packages/llm/llm/src/retry-policy.ts:162`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/retry-policy.ts#L162) | `const retryableCodes = config.retryableCodes ?? [...DEFAULT_RETRYABLE_CODES]` |
| `finishError` | `function` | [`packages/session/session-title-llm/src/index.ts:201`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title-llm/src/index.ts#L201) | `function finishError(finish: FinishReason): Error \| undefined {` |

### Tests and executable evidence

- [`packages/llm/llm/tests/assembler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/assembler.spec.ts) — A test under the owning area exercises or imports `tool-calls`. A test under the owning area exercises or imports `BlockAssembler`.
- [`packages/llm/llm-pi-ai/tests/assemble.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/assemble.ts) — A test under the owning area exercises or imports `BlockAssembler`.
- [`packages/llm/llm/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/properties.spec.ts) — A test under the owning area exercises or imports `tool-calls`. A test under the owning area exercises or imports `BlockAssembler`.
- [`packages/llm/llm-retry/tests/retry.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/tests/retry.spec.ts) — A test under the owning area exercises or imports `EMPTY_RESPONSE_CODE`. A test under the owning area exercises or imports `EMPTY_RESPONSE`.
- [`packages/llm/llm/tests/retry-policy.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/retry-policy.spec.ts) — A test under the owning area exercises or imports `EMPTY_RESPONSE`. A test under the owning area exercises or imports `retryableCodes`.
- [`packages/llm/llm-pi-ai/tests/adapter.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/adapter.e2e.ts) — A test under the owning area exercises or imports `dsh-llm-pi-ai`. A test under the owning area exercises or imports `dsh-llm-deepseek`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `max-tokens`.
- [`packages/llm/llm-deepseek/tests/assemble.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/tests/assemble.ts) — A test under the owning area exercises or imports `BlockAssembler`.

## How to read the implementation

1. Start with [`examples/acp-agent/retry.cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/retry.cordis.yml) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/lifecycle`, `concern/recovery`, `concern/simplification`, `domain/agent-loop`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/shell-terminal`, `domain/storage`, `domain/tools`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/streaming`
- Aliases: `mapStopReason`, `BlockAssembler`, `CONTEXT_WINDOW_EXCEEDED_CODE`, `QUOTA_EXCEEDED_CODE`, `EMPTY_RESPONSE_CODE`, `completed`, `retryableCodes`, `finishError`, `assistant/message`, `dsh-llm`, `EMPTY_RESPONSE`, `dsh-llm-pi-ai`, `dsh-llm-deepseek`, `[DONE]`
- Regex: `(?i)(mapStopReason|BlockAssembler|CONTEXT_WINDOW_EXCEEDED_CODE|QUOTA_EXCEEDED_CODE|EMPTY_RESPONSE_CODE|completed|retryableCodes|finishError)`

```bash
rg -n --pcre2 "(?i)(mapStopReason|BlockAssembler|CONTEXT_WINDOW_EXCEEDED_CODE|QUOTA_EXCEEDED_CODE|EMPTY_RESPONSE_CODE|completed|retryableCodes|finishError)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0037. Provider-routed LLM adapters and a generic pi-ai backend](0037-provider-routed-llm-adapters-and-a-generic-pi-ai-backend.md): Shares source implementation: `packages/llm/llm-pi-ai/src/index.ts`, `packages/llm/llm-retry/src/index.ts`.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/types.ts`.
- **`shares-code-with`** — [0181. Per-provider request retry policies](0181-per-provider-request-retry-policies.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/types.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/types.ts`.
- **`shares-code-with`** — [0494. Property-based testing for protocol-shaped code](0494-property-based-testing-for-protocol-shaped-code.md): Shares source implementation: `packages/llm/llm/src/assembler.ts`, `packages/llm/llm/src/error.ts`.
- **`shares-code-with`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares source implementation: `packages/llm/llm/src/error.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0077. request-level LLM configuration and the credential seam](0077-request-level-llm-configuration-and-the-credential-seam.md): Shares source implementation: `packages/llm/llm-pi-ai/src/index.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0008. Two LLM adapters as a design-verification twin](0008-two-llm-adapters-as-a-design-verification-twin.md): Shares source implementation: `packages/llm/llm-pi-ai/src/index.ts`, `packages/llm/llm/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0307-empty-model-completions-are-retryable-empty-response-failures.md`.
