---
id: "dsh-note-0161"
title: "Model-facing same-session goal tools"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-19-model-facing-goal-tools.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "resume"
  - "blocked"
  - "AgentRegistry"
  - "goals"
  - "complete"
  - "pause"
  - "@deepseek-ai/dsh-tool-goal"
  - "packages/goal/tool-goal/"
  - "ctx.goals"
  - "get_goal"
  - "create_goal"
  - "update_goal"
  - "blocked_reason"
  - "model-reported"
search_regex: "(?i)(resume|blocked|AgentRegistry|goals|complete|pause|@deepseek\\-ai/dsh\\-tool\\-goal|packages/goal/tool\\-goal/)"
---

# 0161. Model-facing same-session goal tools — implementation context

## Open this when

The persisted goal domain deliberately exposes lifecycle verbs to plugins, not directly to a model. A model still needs a small control API for discovering the current goal, creating one from human intent, and changing its lifecycle. Prompt guidance alone cannot establish who authorized a mutation: a subagent, injected plugin message, stale model turn, or resumed session could all produce the same tool arguments. The tool API also needs to preserve the separation between durable state and live execution authority.

## Source decision

@deepseek-ai/dsh-tool-goal in packages/goal/tool-goal/ contributes three exclusive tools and one system-prompt policy section over ctx.goals: get_goal, create_goal, and update_goal. The names and read-create-update shape follow Codex's compact goal tool surface while the authority rules use this repository's public agent, session, tool, and goal services. get_goal() returns the current goal or null. A non-null result contains the compare-and-set id and revision, objective, durable phase, admitted and maximum goal rounds, any blocker reason, plus the process-local activation observation.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-19-model-facing-goal-tools.md](../02-notes/implemented/feature/2026-07-19-model-facing-goal-tools.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-19-model-facing-goal-tools.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-19-model-facing-goal-tools.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/goal/command-goal/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/command-goal/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/goal/command-goal`. | `named-file, named-package-member` |
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. Defines `AgentRegistry`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/goal/tool-goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/goal/tool-goal`. Core file in the package named by the note: `packages/goal/tool-goal`. | `named-directory-member, named-package-member` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/goal/command-goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/command-goal/src/index.ts) | package entry point | Core file in the package named by the note: `packages/goal/command-goal`. | `named-package-member` |
| [`packages/goal/tool-goal/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/goal/tool-goal`. Core file in the package named by the note: `packages/goal/tool-goal`. | `named-directory-member, named-package-member` |
| [`packages/goal/tool-goal/src/authority.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/src/authority.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/goal/tool-goal`. Core file in the package named by the note: `packages/goal/tool-goal`. | `named-directory-member, named-package-member` |
| [`packages/goal/command-goal/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/command-goal/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/goal/command-goal`. | `named-package-member` |
| [`packages/goal/tool-goal/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/goal/tool-goal`. Core file in the package named by the note: `packages/goal/tool-goal`. | `named-directory-member, named-package-member` |
| [`packages/goal/tool-goal/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/goal/tool-goal`. Core file in the package named by the note: `packages/goal/tool-goal`. | `named-directory-member, named-package-member` |
| [`packages/goal/tool-goal`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal) | package or module directory | The source note names this implementation area directly. The note names this package or capability. | `named-directory, named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `resume` | `let` | [`packages/api/remotes/src/agent-lookup.ts:143`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/src/agent-lookup.ts#L143) | `let resume = resumes.get(sessionId)` |
| `blocked` | `const` | [`packages/client/ui-settings-plugins/src/client/PluginCard.tsx:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/client/PluginCard.tsx#L52) | `const blocked = !state.dirty \|\| state.invalid \|\| state.saving` |
| `AgentRegistry` | `class` | [`packages/core/agent/src/index.ts:256`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L256) | `export class AgentRegistry extends Service {` |
| `goals` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1798`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1798) | `const goals = presets?.serviceFor(agent, 'goals') ?? ctx.get('goals')` |
| `complete` | `const` | [`packages/jobs/tool-jobs/src/index.ts:129`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L129) | `const complete = \`${content}${suffix}\`` |
| `pause` | `function` | [`packages/shell/tool-bash-persistent/src/index.ts:141`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash-persistent/src/index.ts#L141) | `async function pause(): Promise<void> {` |

### Tests and executable evidence

- [`packages/core/agent/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/agent.spec.ts) — A test under the owning area exercises or imports `AgentRegistry`. A test under the owning area exercises or imports `steer`.
- [`packages/goal/tool-goal/tests/tool-goal.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/tests/tool-goal.spec.ts) — A test under the owning area exercises or imports `get_goal`. A test under the owning area exercises or imports `create_goal`.
- [`packages/core/agent/tests/agent-initiator.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/agent-initiator.spec.ts) — A test under the owning area exercises or imports `AgentRegistry`.
- [`packages/goal/command-goal/tests/command-goal.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/command-goal/tests/command-goal.spec.ts) — A test under the owning area exercises or imports `dsh-command-goal`.
- Source verification intent: Unit coverage pins registration and disposal, exclusive scheduling, generated prompt policy, filler-safe generic presentation, direct-human creation in a non-English turn, exact/stale/non-running agent and driver checks, live-child rejection, resumed-fork root authority, steering, mismatched initiators, read/create/partial-edit/pause/resume behavior including strict-schema fillers, conditional blocker explanations, rearming after a session-start edge, authority-before-conditional-argument failures, exact goal-round completion, autonomous-only terminal stopping, the configured blocking threshold, and immediate.

## How to read the implementation

1. Start with [`packages/goal/command-goal/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/command-goal/README.md) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/jobs-tasks`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/policy`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `resume`, `blocked`, `AgentRegistry`, `goals`, `complete`, `pause`, `@deepseek-ai/dsh-tool-goal`, `packages/goal/tool-goal/`, `ctx.goals`, `get_goal`, `create_goal`, `update_goal`, `blocked_reason`, `model-reported`
- Regex: `(?i)(resume|blocked|AgentRegistry|goals|complete|pause|@deepseek\-ai/dsh\-tool\-goal|packages/goal/tool\-goal/)`

```bash
rg -n --pcre2 "(?i)(resume|blocked|AgentRegistry|goals|complete|pause|@deepseek\\-ai/dsh\\-tool\\-goal|packages/goal/tool\\-goal/)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0335. Goal-round wrap-up message](0335-goal-round-wrap-up-message.md): The source note links to this decision directly.
- **`shares-code-with`** — [0157. Harness-level goal-based execution](0157-harness-level-goal-based-execution.md): Shares source implementation: `packages/goal/command-goal/src/index.ts`, `packages/goal/tool-goal/src/index.ts`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0035. Agent-scope runtime design and correctness](0035-agent-scope-runtime-design-and-correctness.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0015. The background job runtime (`ctx.jobs`) and generic task control tools](0015-the-background-job-runtime-ctx-jobs-and-generic-task-control-tools.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0164. Same-session goal-round driver](0164-same-session-goal-round-driver.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0144. The approval seam --- one-shot permission decisions over a waterfall of answerers](0144-the-approval-seam-one-shot-permission-decisions-over-a-waterfall-of-answ.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0161-model-facing-same-session-goal-tools.md`.
