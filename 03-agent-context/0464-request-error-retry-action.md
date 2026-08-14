---
id: "dsh-note-0464"
title: "Request-error retry action"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-07-27-request-error-retry-action.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/context"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "ReactLoopAgent"
  - "inject"
  - "RequestErrorAction"
  - "retry"
  - "next"
  - "agent/request-error"
  - "Agent.retry"
  - "Request-error retry action"
  - "simplification"
  - "boundary"
  - "cancellation timeout"
  - "evidence"
  - "ownership"
  - "recovery"
search_regex: "(?i)(ReactLoopAgent|inject|RequestErrorAction|retry|next|agent/request\\-error|Agent\\.retry|Request\\-error[- ]retry[- ]action)"
---

# 0464. Request-error retry action — implementation context

## Open this when

Model-request recovery was decided inside agent/request-error but communicated through Agent.retry(). That public command was valid during one narrow waterfall window and while idle, rejected other running states, and required ReactLoopAgent to retain a mutable retry window beside the waterfall result. The recovery plugins were the only production callers, so the wider live-agent capability exposed states and behavior unrelated to their policy decision.

## Source decision

agent/request-error returns RequestErrorAction, whose handling action is { kind: 'retry' }; the default undefined keeps the failed turn terminal. A listener that does not own the failure calls next(). A listener that owns it performs any awaited repair and returns the retry action without delegating. The loop reads the action after the waterfall settles, closes the failed turn, and opens one retry turn from durable history. It rechecks the turn signal when consuming the action, so cancellation or disposal during recovery prevents the retry even if a listener returns it afterward.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-07-27-request-error-retry-action.md](../02-notes/implemented/simplification/2026-07-27-request-error-retry-action.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-07-27-request-error-retry-action.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-07-27-request-error-retry-action.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/runtime-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/runtime-types.ts) | runtime implementation | Core file in the package named by the note: `packages/core/agent`. Defines `RequestErrorAction`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts) | package entry point | Defines `next`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-retry/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/index.ts) | package entry point | Defines `retry`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent-loop/src/agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts) | runtime implementation | Defines `ReactLoopAgent`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `agent/request-error` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Contains the exact code literal `agent/request-error` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ReactLoopAgent` | `class` | [`packages/core/agent-loop/src/agent.ts:64`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L64) | `export class ReactLoopAgent implements Agent {` |
| `inject` | `const` | [`packages/core/agent/src/invariant.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts#L12) | `export const inject = ['invariants']` |
| `RequestErrorAction` | `type` | [`packages/core/agent/src/runtime-types.ts:58`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/runtime-types.ts#L58) | `export type RequestErrorAction = { kind: 'retry' } \| undefined` |
| `retry` | `const` | [`packages/llm/llm-retry/src/index.ts:191`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/index.ts#L191) | `const retry = previousRetry + 1` |
| `next` | `const` | [`vendor/loader/src/index.ts:181`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts#L181) | `const next = fiber.parent.fiber` |

### Tests and executable evidence

- [`packages/core/agent/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/agent.spec.ts) — A test under the owning area exercises or imports `steer`.
- [`packages/core/agent-loop/tests/contract-regressions.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/contract-regressions.spec.ts) — A test under the owning area exercises or imports `ReactLoopAgent`.

## How to read the implementation

1. Start with [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/agent-loop`, `domain/context`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `ReactLoopAgent`, `inject`, `RequestErrorAction`, `retry`, `next`, `agent/request-error`, `Agent.retry`, `Request-error retry action`, `simplification`, `boundary`, `cancellation timeout`, `evidence`, `ownership`, `recovery`
- Regex: `(?i)(ReactLoopAgent|inject|RequestErrorAction|retry|next|agent/request\-error|Agent\.retry|Request\-error[- ]retry[- ]action)`

```bash
rg -n --pcre2 "(?i)(ReactLoopAgent|inject|RequestErrorAction|retry|next|agent/request\\-error|Agent\\.retry|Request\\-error[- ]retry[- ]action)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0653. Stop mirroring the token stream as an agent event](0653-stop-mirroring-the-token-stream-as-an-agent-event.md): Shares source implementation: `packages/core/agent`, `packages/core/agent-loop/src/agent.ts`.
- **`shares-code-with`** — [0469. Keep agent routing private](0469-keep-agent-routing-private.md): Shares source implementation: `packages/core/agent`, `packages/core/agent-loop/src/agent.ts`.
- **`shares-code-with`** — [0104. Agent-scoped events dispatch a single payload object](0104-agent-scoped-events-dispatch-a-single-payload-object.md): Shares source implementation: `packages/core/agent`, `packages/core/agent-loop/src/agent.ts`.
- **`shares-code-with`** — [0042. Explicit turn cancellation capability](0042-explicit-turn-cancellation-capability.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0449. Stop mirroring durable boundaries as agent events](0449-stop-mirroring-durable-boundaries-as-agent-events.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/README.md`.
- **`shares-code-with`** — [0334. Reject human interaction from runtime-owned subagents](0334-reject-human-interaction-from-runtime-owned-subagents.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/README.md`.
- **`shares-code-with`** — [0363. Bounded background job admission](0363-bounded-background-job-admission.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0011. Agent lifecycle and ownership contracts](0011-agent-lifecycle-and-ownership-contracts.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0464-request-error-retry-action.md`.
