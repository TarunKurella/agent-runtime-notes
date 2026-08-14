---
id: "dsh-note-0280"
title: "Parallel subagent delegations"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-09-parallel-subagent-delegations.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/filesystem"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "shared"
  - "subagent"
  - "maxParallelToolCalls"
  - "concurrencySafe"
  - "createdAt"
  - "isConcurrencySafe"
  - "run_in_background: true"
  - "dsh-workflow-worker-thread"
  - "ctx.subagents.start"
  - "dsh-tool-subagent"
  - "isConcurrencySafe: () => true"
  - "isConcurrencySafe:"
  - "sandbox/mode"
  - "approval/policy"
search_regex: "(?i)(shared|subagent|maxParallelToolCalls|concurrencySafe|createdAt|isConcurrencySafe|run_in_background:[- ]true|dsh\\-workflow\\-worker\\-thread)"
---

# 0280. Parallel subagent delegations — implementation context

## Open this when

A model that wants fan-out batches several subagent calls into one assistant message --- that batch is the parallel intent. The delegation tool declared no isConcurrencySafe classifier, so the fail-closed scheduler (parallel tool-call Agent Note) treated every foreground delegation as an exclusive barrier: nine cards in the GUI, one child running, eight queued behind it for its full runtime. The original conservative stance --- a unary classifier cannot prove that sibling delegations have disjoint workspace effects --- had stopped protecting anything.

## Source decision

dsh-tool-subagent declares isConcurrencySafe: () => true for every call form (foreground, one-shot background, continuable), so sibling delegations in one assistant step overlap under the loop's rolling pool up to maxParallelToolCalls, with results still committed in model order. The declaration satisfies the scheduler's safety contract structurally: a child works in its own session, a run never mutates the parent session (the start-time appends --- sandbox/mode, approval/policy, subagent/descriptor --- land only in the child's own log), and the tool returns its outputs to the loop for ordered commit.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-09-parallel-subagent-delegations.md](../02-notes/implemented/feature/2026-08-09-parallel-subagent-delegations.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-09-parallel-subagent-delegations.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-09-parallel-subagent-delegations.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/subagent/subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/tool-subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/test-support/llm-replay/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`packages/subagent/tool-subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/test-support/llm-replay/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`packages/workflow/workflow-worker-thread/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow-worker-thread/src/index.ts) | package entry point | Core file in the package named by the note: `packages/workflow/workflow-worker-thread`. | `named-package-member` |
| [`packages/workflow/workflow-worker-thread/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow-worker-thread/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/workflow/workflow-worker-thread`. | `named-package-member` |
| [`packages/workflow/workflow-worker-thread/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow-worker-thread/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/workflow/workflow-worker-thread`. | `named-package-member` |
| [`packages/subagent/subagent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/subagent/tool-subagent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `shared` | `const` | [`packages/client/runtime/src/client/sessions/manager.ts:540`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts#L540) | `const shared = opts.sessionId === undefined ? {} : { sessionId: opts.sessionId }` |
| `subagent` | `const` | [`packages/client/ui-subagent/src/client/index.ts:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-subagent/src/client/index.ts#L45) | `const subagent = owner.session?.subagent` |
| `maxParallelToolCalls` | `const` | [`packages/core/agent-loop/src/index.ts:134`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L134) | `const maxParallelToolCalls = value ?? DEFAULT_MAX_PARALLEL_TOOL_CALLS` |
| `concurrencySafe` | `const` | [`packages/core/tools/src/index.ts:1280`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1280) | `const concurrencySafe: unknown = tool.isConcurrencySafe(exec.arguments)` |
| `createdAt` | `const` | [`packages/goal/goal/src/fold.ts:160`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L160) | `const createdAt = nonNegativeInteger(value['createdAt'], 'createdAt')` |

### Tests and executable evidence

- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `concurrencySafe`.
- [`packages/core/agent-loop/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/settings.spec.ts) — A test under the owning area exercises or imports `maxParallelToolCalls`.
- [`packages/core/agent-loop/tests/tool-calls.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/tool-calls.spec.ts) — A test under the owning area exercises or imports `maxParallelToolCalls`.
- [`packages/subagent/tool-subagent/tests/tool-subagent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/tests/tool-subagent.spec.ts) — A test under the owning area exercises or imports `send_message`.
- [`packages/workflow/workflow-worker-thread/tests/integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow-worker-thread/tests/integration.spec.ts) — A test under the owning area exercises or imports `dsh-workflow-worker-thread`.
- [`packages/workflow/workflow-worker-thread/tests/built-worker.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow-worker-thread/tests/built-worker.e2e.ts) — A test under the owning area exercises or imports `dsh-workflow-worker-thread`.
- [`packages/workflow/workflow-worker-thread/tests/workflow-worker-thread.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow-worker-thread/tests/workflow-worker-thread.spec.ts) — A test under the owning area exercises or imports `dsh-workflow-worker-thread`.
- [`apps/web/tests/queue-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/queue-actions.e2e.ts) — Contains the exact code literal `dsh-llm-replay` named by the note.
- Source verification intent: Package tests pin the classifier for both call forms. A gate test drives the registry directly with two children that each block until both have started, proving the half the declaration depends on: the tool body and provider start path tolerate concurrent dispatch --- hidden serialization in that stack would deadlock instead of passing silently. A continuable gate holds two provider preparations at the same await, cancels one caller before publication, and proves that the cancelled child leaves no Agent or durable Session while its sibling reaches inbox acceptance and persists independently.

## How to read the implementation

1. Start with [`packages/subagent/subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/filesystem`, `domain/jobs-tasks`, `domain/llm`, `domain/multi-agent`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`
- Aliases: `shared`, `subagent`, `maxParallelToolCalls`, `concurrencySafe`, `createdAt`, `isConcurrencySafe`, `run_in_background: true`, `dsh-workflow-worker-thread`, `ctx.subagents.start`, `dsh-tool-subagent`, `isConcurrencySafe: () => true`, `isConcurrencySafe:`, `sandbox/mode`, `approval/policy`
- Regex: `(?i)(shared|subagent|maxParallelToolCalls|concurrencySafe|createdAt|isConcurrencySafe|run_in_background:[- ]true|dsh\-workflow\-worker\-thread)`

```bash
rg -n --pcre2 "(?i)(shared|subagent|maxParallelToolCalls|concurrencySafe|createdAt|isConcurrencySafe|run_in_background:[- ]true|dsh\\-workflow\\-worker\\-thread)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0151. Parallel tool-call execution by per-call safety](0151-parallel-tool-call-execution-by-per-call-safety.md): The source note links to this decision directly.
- **`shares-code-with`** — [0148. Background subagent tasks](0148-background-subagent-tasks.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/invariant.ts`.
- **`shares-code-with`** — [0462. Merge subagent control into the subagent service](0462-merge-subagent-control-into-the-subagent-service.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/invariant.ts`.
- **`shares-code-with`** — [0678. Record fork and mixed spawn+fork snapshot scenarios](0678-record-fork-and-mixed-spawn-fork-snapshot-scenarios.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/invariant.ts`.
- **`shares-code-with`** — [0153. Configure subagent persona, tool visibility, and depth](0153-configure-subagent-persona-tool-visibility-and-depth.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/types.ts`.
- **`shares-code-with`** — [0173. Durable subagent catalog and list_agents](0173-durable-subagent-catalog-and-list-agents.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/invariant.ts`.
- **`shares-code-with`** — [0049. Canonical tool output contract](0049-canonical-tool-output-contract.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/invariant.ts`.
- **`shares-code-with`** — [0106. Subagent list identity via the projection unit](0106-subagent-list-identity-via-the-projection-unit.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0280-parallel-subagent-delegations.md`.
