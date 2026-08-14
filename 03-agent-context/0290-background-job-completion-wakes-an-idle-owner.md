---
id: "dsh-note-0290"
title: "Background job completion wakes an idle owner"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-11-background-job-completion-wakes-an-idle-owner.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/streaming"
aliases:
  - "target"
  - "inject"
  - "wakeup"
  - "JobStart"
  - "settle"
  - "continuable"
  - "kill"
  - "tool-jobs"
  - "agent.inject"
  - "job_output"
  - "Agent.send"
  - "wakeDriver"
  - "tool-subagent"
  - "maxConsecutiveWakes"
search_regex: "(?i)(target|inject|wakeup|JobStart|settle|continuable|kill|tool\\-jobs)"
---

# 0290. Background job completion wakes an idle owner — implementation context

## Open this when

tool-jobs promised the model "You are notified in-session when a task finishes --- do not busy-poll or sleep on one." The promise held only while the model was still working. Completion delivered through agent.inject(), which appends to the next-step inbox without reserving a driver, so a task settling after its turn closed left the notice parked until something unrelated woke the agent. The common shape is exactly the one that breaks: the model starts a long command, tells the user it started it, ends its turn, and the command finishes into an inbox nobody will claim.

## Source decision

An unreported completion picks its lane from what the owner is doing. A busy owner is injected, unchanged. An idle owner is woken with followup(). This adopts the delivery rule the continuation manager already ships for subagent settlement, where "steering rather than injecting is deliberate … This is a correctness rule, not a deployment preference." The two paths do not overlap: tool-subagent registers a Task only for a one-shot background child and returns continuable before reaching that code, so a child is delivered by exactly one of the two mechanisms.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-11-background-job-completion-wakes-an-idle-owner.md](../02-notes/implemented/feature/2026-08-11-background-job-completion-wakes-an-idle-owner.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-11-background-job-completion-wakes-an-idle-owner.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-11-background-job-completion-wakes-an-idle-owner.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/subsystems/jobs.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/jobs.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/jobs/tool-jobs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/jobs/tool-jobs`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/core/agent-loop/src/agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts) | runtime implementation | Core file in the package named by the note: `packages/core/agent-loop`. Defines `target`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-bash`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/jobs/tool-jobs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/jobs/tool-jobs`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent-loop/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent-loop`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/tool-bash/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/tool-bash`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/tool-subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/tool-subagent`. Defines `continuable`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/tool-subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/subagent/tool-subagent-report/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-report/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/tool-subagent-report`. | `named-package-member` |
| [`packages/subagent/tool-subagent-report/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-report/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/tool-subagent-report`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `target` | `let` | [`packages/core/agent-loop/src/agent.ts:261`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L261) | `let target: InboxTarget = 'next-turn'` |
| `inject` | `const` | [`packages/core/agent-loop/src/invariant.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts#L16) | `export const inject = ['invariants']` |
| `wakeup` | `const` | [`packages/core/tools/src/code-mode.ts:381`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L381) | `const wakeup = (): void => {` |
| `JobStart` | `interface` | [`packages/jobs/jobs/src/types.ts:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/types.ts#L46) | `export interface JobStart {` |
| `inject` | `const` | [`packages/jobs/tool-jobs/src/index.ts:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L22) | `export const inject = ['tools', 'jobs', 'systemPrompt']` |
| `inject` | `const` | [`packages/jobs/tool-jobs/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `settle` | `const` | [`packages/sdk/client/src/dispose.ts:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/dispose.ts#L45) | `const settle = (complete: () => void): void => {` |
| `inject` | `const` | [`packages/shell/tool-bash/src/index.ts:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts#L31) | `export const inject = ['tools', 'shell', 'systemPrompt', 'shellEnv']` |
| `inject` | `const` | [`packages/shell/tool-bash/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `continuable` | `const` | [`packages/subagent/tool-subagent/src/index.ts:276`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts#L276) | `const continuable = (config.backgroundMode ?? 'one-shot') === 'continuable'` |
| `kill` | `const` | [`packages/subprocess/subprocess-local/src/spawn.ts:432`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts#L432) | `const kill = (sig: NodeJS.Signals): void => {` |

### Tests and executable evidence

- [`packages/jobs/jobs/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/tests/service.spec.ts) — A test under the owning area exercises or imports `JobStart`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `wakeup`. A test under the owning area exercises or imports `followup`.
- [`packages/shell/tool-bash/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/tests/tools.spec.ts) — A test under the owning area exercises or imports `job_output`.
- [`packages/core/agent-loop/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/agent.spec.ts) — A test under the owning area exercises or imports `followup`. A test under the owning area exercises or imports `steer`.
- [`packages/core/agent-loop/tests/resume.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/resume.spec.ts) — A test under the owning area exercises or imports `followup`. A test under the owning area exercises or imports `tool-bash`.
- [`packages/core/agent-loop/tests/cancel.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/cancel.spec.ts) — A test under the owning area exercises or imports `steer`.
- [`packages/jobs/tool-jobs/tests/tool-jobs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/tests/tool-jobs.spec.ts) — A test under the owning area exercises or imports `tool-jobs`. A test under the owning area exercises or imports `job_output`.
- [`packages/core/agent-loop/tests/tool-calls.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/tool-calls.spec.ts) — A test under the owning area exercises or imports `followup`.

## How to read the implementation

1. Start with [`docs/subsystems/jobs.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/jobs.md) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/agent-loop`, `domain/build-release`, `domain/extensions`, `domain/jobs-tasks`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/streaming`
- Aliases: `target`, `inject`, `wakeup`, `JobStart`, `settle`, `continuable`, `kill`, `tool-jobs`, `agent.inject`, `job_output`, `Agent.send`, `wakeDriver`, `tool-subagent`, `maxConsecutiveWakes`
- Regex: `(?i)(target|inject|wakeup|JobStart|settle|continuable|kill|tool\-jobs)`

```bash
rg -n --pcre2 "(?i)(target|inject|wakeup|JobStart|settle|continuable|kill|tool\\-jobs)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0015. The background job runtime (`ctx.jobs`) and generic task control tools](0015-the-background-job-runtime-ctx-jobs-and-generic-task-control-tools.md): The source note links to this decision directly.
- **`source-link`** — [0053. Unify agent delivery on send(target × wakeup) and coalesce injected context into user/message](0053-unify-agent-delivery-on-send-target-wakeup-and-coalesce-injected-context.md): The source note links to this decision directly.
- **`source-link`** — [0265. Settlement delivery belongs to the continuation manager](0265-settlement-delivery-belongs-to-the-continuation-manager.md): The source note links to this decision directly.
- **`shares-code-with`** — [0157. Harness-level goal-based execution](0157-harness-level-goal-based-execution.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/core/agent-loop/src/index.ts`.
- **`shares-code-with`** — [0203. SDK max output tokens](0203-sdk-max-output-tokens.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/subagent/tool-subagent/src/index.ts`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/core/agent-loop/src/index.ts`.
- **`shares-code-with`** — [0249. Claude Code and Codex subagent backends](0249-claude-code-and-codex-subagent-backends.md): Shares source implementation: `packages/subagent/tool-subagent/src/index.ts`, `packages/subagent/tool-subagent/src/invariant.ts`.
- **`shares-code-with`** — [0011. Agent lifecycle and ownership contracts](0011-agent-lifecycle-and-ownership-contracts.md): Shares source implementation: `packages/shell/tool-bash/src/index.ts`, `packages/shell/tool-bash/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0290-background-job-completion-wakes-an-idle-owner.md`.
