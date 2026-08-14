---
id: "dsh-note-0538"
title: "Collapse workflows to the exercised foreground core"
status: "rejected"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/rejected/simplification/2026-07-12-collapse-workflow-to-foreground-core.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/rejected"
aliases:
  - "detail"
  - "cancel"
  - "args"
  - "dispose"
  - "signal"
  - "phase"
  - "whenToUse"
  - "log"
  - "model"
  - "phases"
  - "WorkflowError"
  - "isFatalWorkflowError"
  - "WorkflowRun"
  - "WorkflowRunInfo"
search_regex: "(?i)(detail|cancel|args|dispose|signal|phase|whenToUse|model)"
---

# 0538. Collapse workflows to the exercised foreground core — implementation context

## Open this when

The workflow capability executes foreground JavaScript that composes subagents, but it also carries an unconsumed progress-observation system. No production listener subscribes to any of the six workflow/ events; listeners exist only in workflow tests. Nevertheless the seam defines run/phase/agent outcome payloads, the worker sends phase/log/agent lifecycle protocol messages, the host forwards them through a liveAgents pairing ledger, and the engine maintains run ids solely to correlate those notifications.

## Source decision

Keep the exercised core: agent(prompt, { schema, model }), parallel, pipeline, args, concurrency/agent caps, cancellation, bounded disposal, structured results, worker isolation, and foreground tool collection. Remove all workflow/ events and their event-only info/outcome types; remove phase(), log(), agent label/phase, phase declarations, whenToUse, and their worker messages/host observers; collapse workflow metadata to the name the tool actually uses; remove event-only run ids/meta snapshots and the synthesized agent-end ledger.

## Decision status

Rejected design. Do not implement it as written. Reuse its failure analysis, constraints, and reversal condition.

- Raw note: [rejected/simplification/2026-07-12-collapse-workflow-to-foreground-core.md](../02-notes/rejected/simplification/2026-07-12-collapse-workflow-to-foreground-core.md)
- Pinned source: [.agents/notes/rejected/simplification/2026-07-12-collapse-workflow-to-foreground-core.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/rejected/simplification/2026-07-12-collapse-workflow-to-foreground-core.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. Defines `args`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/dispatch.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/dispatch.ts) | runtime implementation | Core file in the package named by the note: `packages/core/agent`. Defines `args`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/cordis/src/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts) | runtime implementation | Defines `label`, a construct named by the note. | `symbol-definition` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `detail`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `phase`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `signal`, a construct named by the note. | `symbol-definition` |
| [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) | package entry point | Defines `whenToUse`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts) | package entry point | Defines `model`, a construct named by the note. | `symbol-definition` |
| [`scripts/build-exe-for-python-sdk.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/build-exe-for-python-sdk.ts) | repository automation | Defines `pipeline`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `detail` | `const` | [`packages/acp/acp/src/index.ts:314`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L314) | `const detail = error instanceof Error ? error.message : String(error)` |
| `cancel` | `const` | [`packages/client/ui-primitives/src/pointer-grace.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/pointer-grace.ts#L36) | `const cancel = useCallback(() => {` |
| `args` | `const` | [`packages/core/agent/src/dispatch.ts:125`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/dispatch.ts#L125) | `const args: unknown[] = [carrier, name, fused(payload)]` |
| `dispose` | `const` | [`packages/core/agent/src/index.ts:373`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L373) | `const dispose = this.ctx.effect(() => {` |
| `dispose` | `const` | [`packages/core/agent/src/index.ts:451`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L451) | `const dispose = this.ctx.effect(function* (this: AgentRegistry) {` |
| `args` | `const` | [`packages/core/agent/src/index.ts:529`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L529) | `const args: unknown[] = [entry.carrier, 'agent/disposed', { agent: entry.agent }]` |
| `args` | `const` | [`packages/core/agent/src/index.ts:561`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L561) | `const args: unknown[] = [entry.carrier, 'agent/created', { agent: entry.agent }]` |
| `signal` | `const` | [`packages/core/tools/src/index.ts:1372`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1372) | `const signal = exec.signal` |
| `phase` | `const` | [`packages/goal/goal/src/fold.ts:100`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L100) | `const phase = value['phase'] as GoalPhase` |
| `whenToUse` | `const` | [`packages/skill/skill/src/index.ts:752`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L752) | `const whenToUse = skill.whenToUse` |
| `log` | `const` | [`packages/test-support/acp-snapshot/src/suite.ts:1298`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts#L1298) | `const log = result.sessionLogs[index]` |
| `model` | `const` | [`packages/typert/loader/src/index.ts:109`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts#L109) | `const model = requireObject(pkgName, manifest.model, 'TYPERT.model')` |
| `phases` | `const` | [`packages/workflow/workflow-worker-thread/src/meta.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow-worker-thread/src/meta.ts#L26) | `const phases: WorkflowPhase[] = []` |
| `WorkflowError` | `class` | [`packages/workflow/workflow/src/index.ts:130`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow/src/index.ts#L130) | `export class WorkflowError extends HarnessError {` |
| `isFatalWorkflowError` | `function` | [`packages/workflow/workflow/src/index.ts:146`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow/src/index.ts#L146) | `export function isFatalWorkflowError(error: unknown): boolean {` |
| `WorkflowRun` | `interface` | [`packages/workflow/workflow/src/runtime-types.ts:40`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow/src/runtime-types.ts#L40) | `export interface WorkflowRun {` |

### Tests and executable evidence

- [`packages/skill/skill/tests/skill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/tests/skill.spec.ts) — A test under the owning area exercises or imports `whenToUse`.
- [`packages/workflow/workflow/tests/workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow/tests/workflow.spec.ts) — A test under the owning area exercises or imports `WorkflowRunInfo`. A test under the owning area exercises or imports `WorkflowRun`.
- [`packages/workflow/workflow/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow/tests/invariant.spec.ts) — A test under the owning area exercises or imports `WorkflowRunInfo`.
- [`packages/workflow/workflow-worker-thread/tests/meta.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow-worker-thread/tests/meta.spec.ts) — A test under the owning area exercises or imports `phases`.
- [`packages/workflow/workflow-worker-thread/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow-worker-thread/tests/session.spec.ts) — A test under the owning area exercises or imports `phases`.
- [`packages/workflow/workflow-worker-thread/tests/workflow-worker-thread.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow-worker-thread/tests/workflow-worker-thread.e2e.ts) — A test under the owning area exercises or imports `phases`.
- [`packages/workflow/workflow-worker-thread/tests/workflow-worker-thread.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow-worker-thread/tests/workflow-worker-thread.spec.ts) — A test under the owning area exercises or imports `phases`.
- Source verification intent: The workflow public contract contains only execution, cancellation, result, and disposal contracts with a production consumer. No workflow event, phase/log protocol message, run-id generator, progress-only metadata, host pairing ledger, or fatal-mode branch remains. The run handle has no id/meta echoes, and cancellation has one holder-owned channel after synchronous start() returns. Parallel/pipeline behavior, caps, cancellation quiescence, worker containment, structured output, and the model-facing workflow scenarios retain coverage.

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
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Rejected design. Do not implement it as written. Reuse its failure analysis, constraints, and reversal condition.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/rejected`
- Aliases: `detail`, `cancel`, `args`, `dispose`, `signal`, `phase`, `whenToUse`, `log`, `model`, `phases`, `WorkflowError`, `isFatalWorkflowError`, `WorkflowRun`, `WorkflowRunInfo`
- Regex: `(?i)(detail|cancel|args|dispose|signal|phase|whenToUse|model)`

```bash
rg -n --pcre2 "(?i)(detail|cancel|args|dispose|signal|phase|whenToUse|model)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0144. The approval seam --- one-shot permission decisions over a waterfall of answerers](0144-the-approval-seam-one-shot-permission-decisions-over-a-waterfall-of-answ.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0363. Bounded background job admission](0363-bounded-background-job-admission.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0035. Agent-scope runtime design and correctness](0035-agent-scope-runtime-design-and-correctness.md): Shares source implementation: `packages/core/agent/src/dispatch.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0448. Keep one public stop primitive](0448-keep-one-public-stop-primitive.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/agent`.
- **`shares-code-with`** — [0142. Dynamic workflows --- a script-driven multi-agent orchestration seam](0142-dynamic-workflows-a-script-driven-multi-agent-orchestration-seam.md): Shares source implementation: `packages/core/agent/src/dispatch.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0450. Unify the agent id and the session id](0450-unify-the-agent-id-and-the-session-id.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/agent`.
- **`shares-code-with`** — [0449. Stop mirroring durable boundaries as agent events](0449-stop-mirroring-durable-boundaries-as-agent-events.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/agent`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0538-collapse-workflows-to-the-exercised-foreground-core.md`.
