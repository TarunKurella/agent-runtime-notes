---
id: "dsh-note-0064"
title: "The job registry is a capability seam (`dsh-jobs` / `dsh-jobs-local`)"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-26-job-registry-seam.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "core"
  - "list"
  - "TASK_WAIT_TIMEOUT"
  - "LocalJobRegistry"
  - "JobId"
  - "JobRegistry"
  - "JobKindMap"
  - "JobOutcome"
  - "JobStart"
  - "JobHooks"
  - "JobSnapshot"
  - "JobRead"
  - "JobDoneListener"
  - "jobs"
search_regex: "(?i)(core|list|TASK_WAIT_TIMEOUT|LocalJobRegistry|JobId|JobRegistry|JobKindMap|JobOutcome)"
---

# 0064. The job registry is a capability seam (`dsh-jobs` / `dsh-jobs-local`) — implementation context

## Open this when

The background-job runtime shipped JobRegistry as one concrete package: @deepseek-ai/dsh-jobs owned both the ctx.jobs contract every producer and controller programs against and the process-local provider (the in-memory store, settlement bookkeeping, owner-cleanup effects, teardown). That bundling recouples the two rates of change the repository's capability-seam rule separates: swapping the registry's storage or lifecycle backend would churn the same package whose types and ctx.jobs API producers (dsh-tool-bash, dsh-tool-terminal, dsh-tool-subagent), the controller (dsh-tool-jobs), and JobKindMap extenders.

## Source decision

jobs/ is now a three-package capability family in the bash-trio shape: @deepseek-ai/dsh-jobs (Service Definition) --- the abstract JobRegistry extends Service owning ctx.jobs, the nine-method contract (start, list, get, read, kill, wait, onJobDone, onJobsChanged, attachController), all vocabulary types (JobId, JobKindMap, JobStart, JobHooks, JobOutcome, JobSnapshot, JobRead, JobDoneListener), and the snapshot invariant companion.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-26-job-registry-seam.md](../02-notes/implemented/architecture/2026-07-26-job-registry-seam.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-26-job-registry-seam.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-26-job-registry-seam.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/jobs/jobs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/jobs/jobs`. Defines `JobRegistry`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/jobs/jobs/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/jobs/jobs`. Defines `JobKindMap`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/jobs/jobs/src/brand.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/brand.ts) | runtime implementation | Core file in the package named by the note: `packages/jobs/jobs`. Defines `JobId`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/util/timeout/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/index.ts) | package entry point | Core file in the package named by the note: `packages/util/timeout`. | `named-package-member` |
| [`packages/jobs/jobs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/jobs/jobs`. | `named-package-member` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/jobs/tool-jobs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/jobs/tool-jobs`. Defines `jobs`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/jobs/jobs-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/jobs/jobs-local`. Defines `LocalJobRegistry`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-bash`. Defines `jobs`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/util/timeout/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/util/timeout`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `core` | `const` | [`packages/client/ui-conversation/src/client/input/facade.ts:497`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/facade.ts#L497) | `const core = this.core.state` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `TASK_WAIT_TIMEOUT` | `const` | [`packages/jobs/jobs-local/src/index.ts:25`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs-local/src/index.ts#L25) | `export const TASK_WAIT_TIMEOUT = 'TASK_WAIT_TIMEOUT'` |
| `LocalJobRegistry` | `class` | [`packages/jobs/jobs-local/src/index.ts:91`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs-local/src/index.ts#L91) | `export class LocalJobRegistry extends JobRegistry {` |
| `JobId` | `type` | [`packages/jobs/jobs/src/brand.ts:19`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/brand.ts#L19) | `export type JobId = Branded<'JobId'>` |
| `JobId` | `function` | [`packages/jobs/jobs/src/brand.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/brand.ts#L26) | `export function JobId(id: string): JobId {` |
| `JobRegistry` | `class` | [`packages/jobs/jobs/src/index.ts:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/index.ts#L62) | `export abstract class JobRegistry extends Service {` |
| `JobKindMap` | `interface` | [`packages/jobs/jobs/src/types.ts:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/types.ts#L23) | `export interface JobKindMap {` |
| `JobOutcome` | `interface` | [`packages/jobs/jobs/src/types.ts:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/types.ts#L32) | `export interface JobOutcome {` |
| `JobStart` | `interface` | [`packages/jobs/jobs/src/types.ts:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/types.ts#L46) | `export interface JobStart {` |
| `JobHooks` | `interface` | [`packages/jobs/jobs/src/types.ts:72`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/types.ts#L72) | `export interface JobHooks {` |
| `JobSnapshot` | `interface` | [`packages/jobs/jobs/src/types.ts:97`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/types.ts#L97) | `export interface JobSnapshot {` |
| `JobRead` | `interface` | [`packages/jobs/jobs/src/types.ts:131`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/types.ts#L131) | `export interface JobRead {` |
| `JobDoneListener` | `type` | [`packages/jobs/jobs/src/types.ts:146`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/types.ts#L146) | `export type JobDoneListener = (` |
| `jobs` | `const` | [`packages/jobs/tool-jobs/src/index.ts:356`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L356) | `const jobs = ctx.jobs.list(exec.agent)` |
| `stopping` | `let` | [`packages/schedule/schedule/src/index.ts:42`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/index.ts#L42) | `let stopping = false` |

### Tests and executable evidence

- [`packages/jobs/jobs/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/tests/service.spec.ts) — A test under the owning area exercises or imports `dsh-jobs`. A test under the owning area exercises or imports `dsh-jobs-local`.
- [`packages/jobs/jobs/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/tests/invariant.spec.ts) — A test under the owning area exercises or imports `dsh-jobs`. A test under the owning area exercises or imports `JobRegistry`.
- [`packages/jobs/jobs-local/tests/jobs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs-local/tests/jobs.spec.ts) — A test under the owning area exercises or imports `dsh-jobs`. A test under the owning area exercises or imports `dsh-jobs-local`.
- [`packages/shell/tool-bash/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/tests/tools.spec.ts) — A test under the owning area exercises or imports `dsh-jobs-local`. A test under the owning area exercises or imports `LocalJobRegistry`.
- [`packages/jobs/tool-jobs/tests/tool-jobs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/tests/tool-jobs.spec.ts) — A test under the owning area exercises or imports `dsh-jobs`. A test under the owning area exercises or imports `dsh-jobs-local`.
- [`packages/shell/tool-bash/tests/integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/tests/integration.spec.ts) — A test under the owning area exercises or imports `dsh-jobs-local`. A test under the owning area exercises or imports `dsh-tool-jobs`.
- [`packages/terminal/tool-terminal/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/tool-terminal/tests/tools.spec.ts) — A test under the owning area exercises or imports `dsh-jobs-local`. A test under the owning area exercises or imports `dsh-tool-terminal`.
- [`packages/terminal/tool-terminal/tests/render.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/tool-terminal/tests/render.spec.ts) — A test under the owning area exercises or imports `dsh-tool-terminal`.

## How to read the implementation

1. Start with [`packages/jobs/jobs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `core`, `list`, `TASK_WAIT_TIMEOUT`, `LocalJobRegistry`, `JobId`, `JobRegistry`, `JobKindMap`, `JobOutcome`, `JobStart`, `JobHooks`, `JobSnapshot`, `JobRead`, `JobDoneListener`, `jobs`
- Regex: `(?i)(core|list|TASK_WAIT_TIMEOUT|LocalJobRegistry|JobId|JobRegistry|JobKindMap|JobOutcome)`

```bash
rg -n --pcre2 "(?i)(core|list|TASK_WAIT_TIMEOUT|LocalJobRegistry|JobId|JobRegistry|JobKindMap|JobOutcome)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0007. Capability seams --- Service Definition / Service Provider / Consumer roles](0007-capability-seams-service-definition-service-provider-consumer-roles.md): The source note links to this decision directly.
- **`source-link`** — [0015. The background job runtime (`ctx.jobs`) and generic task control tools](0015-the-background-job-runtime-ctx-jobs-and-generic-task-control-tools.md): The source note links to this decision directly.
- **`shares-code-with`** — [0134. Subagent capability seam](0134-subagent-capability-seam.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0158. persistent PTY sessions](0158-persistent-pty-sessions.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0042. Explicit turn cancellation capability](0042-explicit-turn-cancellation-capability.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0142. Dynamic workflows --- a script-driven multi-agent orchestration seam](0142-dynamic-workflows-a-script-driven-multi-agent-orchestration-seam.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0363. Bounded background job admission](0363-bounded-background-job-admission.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0064-the-job-registry-is-a-capability-seam-dsh-jobs-dsh-jobs-local.md`.
