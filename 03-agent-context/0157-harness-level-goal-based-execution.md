---
id: "dsh-note-0157"
title: "Harness-level goal-based execution"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-16-harness-level-loop.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/simplification"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "subagents"
  - "resume"
  - "loop"
  - "armed"
  - "clear"
  - "blocked"
  - "active"
  - "block"
  - "disarm"
  - "code"
  - "GoalId"
  - "GoalRef"
  - "GoalPhase"
  - "GoalBlockReason"
search_regex: "(?i)(subagents|resume|loop|armed|clear|blocked|active|block)"
---

# 0157. Harness-level goal-based execution — implementation context

## Open this when

The concrete agent loop owns one turn: it drains admitted input, performs one or more model-and-tool steps, and stops. Substantial objectives often need an outer policy that can begin another turn, retain progress, stop at a budget, and remain intelligible to humans. A timed prompt, a same-session continuation, and a fresh-agent Ralph attempt all repeat work, but they do not share the same state, authority, memory, or lifecycle. Treating every repeated action as one generic "loop" obscures those differences.

## Source decision

Two explicit plugin policies over existing seams: Same-session goals retain one durable objective in the current session and admit goal-attributed continuation turns only while live activation is armed. Fresh-agent Ralph runs execute a fixed foreground workflow whose rounds each spawn a new structured child with no conversation seed. There is no packages/loop/ family, LoopDriver, LoopId, universal StopCondition, or model-facing generic loop tool.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-16-harness-level-loop.md](../02-notes/implemented/feature/2026-07-16-harness-level-loop.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-16-harness-level-loop.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-16-harness-level-loop.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/goal/goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/goal/goal`. Core file in the package named by the note: `packages/goal/goal`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/goal/goal/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/goal/goal`. Core file in the package named by the note: `packages/goal/goal`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/goal/goal/src/runtime.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/runtime.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/goal/goal`. Core file in the package named by the note: `packages/goal/goal`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/goal/goal/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/goal/goal`. Core file in the package named by the note: `packages/goal/goal`. | `named-directory-member, named-package-member` |
| [`packages/goal/tool-goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/goal/tool-goal`. Core file in the package named by the note: `packages/goal/tool-goal`. | `named-directory-member, named-package-member` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/core/agent-loop/src/agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts) | runtime implementation | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/goal/command-goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/command-goal/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/goal/command-goal`. Core file in the package named by the note: `packages/goal/command-goal`. | `named-directory-member, named-package-member` |
| [`packages/goal/tool-goal/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/goal/tool-goal`. Core file in the package named by the note: `packages/goal/tool-goal`. | `named-directory-member, named-package-member` |
| [`packages/workflow/tool-ralph/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/workflow/tool-ralph`. Core file in the package named by the note: `packages/workflow/tool-ralph`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/core/agent-loop/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/interaction/commands/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/interaction/commands`. Core file in the package named by the note: `packages/interaction/commands`. | `named-directory-member, named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `subagents` | `const` | [`packages/acp/acp/src/index.ts:376`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L376) | `const subagents = ctx.get('subagents') as ContinuableDrain \| undefined` |
| `resume` | `let` | [`packages/api/remotes/src/agent-lookup.ts:143`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/src/agent-lookup.ts#L143) | `let resume = resumes.get(sessionId)` |
| `loop` | `const` | [`packages/client/runtime/src/client/index.ts:204`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/index.ts#L204) | `const loop = connection.start({` |
| `armed` | `const` | [`packages/client/ui-directory-picker-native/src/client/flow.ts:27`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-directory-picker-native/src/client/flow.ts#L27) | `const armed = useRef(false)` |
| `clear` | `const` | [`packages/client/ui-primitives/src/ansi.ts:256`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ansi.ts#L256) | `const clear = (index: number, fill: string): void => {` |
| `blocked` | `const` | [`packages/client/ui-settings-plugins/src/client/PluginCard.tsx:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/client/PluginCard.tsx#L52) | `const blocked = !state.dirty \|\| state.invalid \|\| state.saving` |
| `active` | `let` | [`packages/core/scope/src/store.ts:47`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/store.ts#L47) | `let active = true` |
| `block` | `const` | [`packages/core/session/src/index.ts:343`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L343) | `const block = content[0]` |
| `disarm` | `function` | [`packages/goal/goal-round-driver/src/index.ts:117`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal-round-driver/src/index.ts#L117) | `function disarm(state: DriverState): void {` |
| `code` | `const` | [`packages/goal/goal/src/index.ts:170`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L170) | `const code = record?.['code']` |
| `GoalId` | `function` | [`packages/goal/goal/src/runtime.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/runtime.ts#L15) | `export function GoalId(id: string): GoalIdType {` |
| `GoalId` | `type` | [`packages/goal/goal/src/types.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/types.ts#L16) | `export type GoalId = Branded<'GoalId'>` |
| `GoalRef` | `interface` | [`packages/goal/goal/src/types.ts:19`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/types.ts#L19) | `export interface GoalRef {` |
| `GoalPhase` | `type` | [`packages/goal/goal/src/types.ts:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/types.ts#L44) | `export type GoalPhase =` |
| `GoalBlockReason` | `interface` | [`packages/goal/goal/src/types.ts:51`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/types.ts#L51) | `export interface GoalBlockReason {` |
| `GoalSnapshot` | `interface` | [`packages/goal/goal/src/types.ts:59`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/types.ts#L59) | `export interface GoalSnapshot extends GoalRef {` |

### Tests and executable evidence

- [`packages/goal/goal/tests/goal.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/tests/goal.e2e.ts) — A test under the owning area exercises or imports `maxGoalRounds`.
- [`packages/goal/goal/tests/goal.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/tests/goal.spec.ts) — A test under the owning area exercises or imports `GoalId`. A test under the owning area exercises or imports `GoalRef`.
- [`packages/goal/goal/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/tests/invariant.spec.ts) — A test under the owning area exercises or imports `GoalId`. A test under the owning area exercises or imports `maxGoalRounds`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `dsh-agent-loop`.
- [`packages/goal/goal/tests/projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/tests/projection.spec.ts) — A test under the owning area exercises or imports `GoalRef`. A test under the owning area exercises or imports `paused`.
- [`packages/core/agent-loop/tests/cancel.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/cancel.spec.ts) — A test under the owning area exercises or imports `blocked`.
- [`packages/core/agent-loop/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/settings.spec.ts) — A test under the owning area exercises or imports `dsh-agent-loop`.
- [`packages/goal/tool-goal/tests/tool-goal.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/tests/tool-goal.spec.ts) — A test under the owning area exercises or imports `GoalId`. A test under the owning area exercises or imports `GoalRef`.

## How to read the implementation

1. Start with [`packages/goal/goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/simplification`, `concern/trust`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/jobs-tasks`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/policy`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `subagents`, `resume`, `loop`, `armed`, `clear`, `blocked`, `active`, `block`, `disarm`, `code`, `GoalId`, `GoalRef`, `GoalPhase`, `GoalBlockReason`
- Regex: `(?i)(subagents|resume|loop|armed|clear|blocked|active|block)`

```bash
rg -n --pcre2 "(?i)(subagents|resume|loop|armed|clear|blocked|active|block)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0090. Goal-owned durable events](0090-goal-owned-durable-events.md): The source note links to this decision directly.
- **`source-link`** — [0159. Fresh-agent Ralph workflow tool](0159-fresh-agent-ralph-workflow-tool.md): The source note links to this decision directly.
- **`source-link`** — [0160. Human `/goal` command](0160-human-goal-command.md): The source note links to this decision directly.
- **`source-link`** — [0161. Model-facing same-session goal tools](0161-model-facing-same-session-goal-tools.md): The source note links to this decision directly.
- **`shares-code-with`** — [0164. Same-session goal-round driver](0164-same-session-goal-round-driver.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/goal/goal/src/index.ts`.
- **`shares-code-with`** — [0162. Persisted same-session goal domain](0162-persisted-same-session-goal-domain.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/goal/goal/src/index.ts`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/core/agent-loop/src/index.ts`.
- **`shares-code-with`** — [0092. Typert Gateway Targeted Method Calls](0092-typert-gateway-targeted-method-calls.md): Shares source implementation: `packages/goal/goal/src/index.ts`, `packages/goal/goal/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0157-harness-level-goal-based-execution.md`.
