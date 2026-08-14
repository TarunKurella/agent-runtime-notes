---
id: "dsh-note-0162"
title: "Persisted same-session goal domain"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-19-persisted-same-session-goal-domain.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "armed"
  - "blocked"
  - "active"
  - "GOAL_CHANGE_VERSION"
  - "GoalRef"
  - "goals"
  - "complete"
  - "@deepseek-ai/dsh-goal"
  - "packages/goal/goal/"
  - "ctx.goals"
  - "maxGoalRounds"
  - "defaultMaxGoalRounds"
  - "goal/change"
  - "user/message"
search_regex: "(?i)(armed|blocked|active|GOAL_CHANGE_VERSION|GoalRef|goals|complete|@deepseek\\-ai/dsh\\-goal)"
---

# 0162. Persisted same-session goal domain — implementation context

## Open this when

A long-running objective outlives one prompt, turn, or model request. Treating that objective as an in-memory loop variable loses it on process restart, while putting it only in UI state makes model behavior impossible to reconstruct. Treating every session turn as progress also charges unrelated human messages against an automatic-work budget. Durable lifecycle and permission to continue are different facts. A session may retain an active objective after restart or fork, but silently starting work when a user opens that session is surprising.

## Source decision

@deepseek-ai/dsh-goal in packages/goal/goal/ owns one current same-session goal through ctx.goals. A goal has a branded id, objective, durable phase, compare-and-set revision, and maxGoalRounds. defaultMaxGoalRounds is a validated deployment setting with default 256; create() materializes it internally before mutation rather than exposing resolution as another service verb. The durable phases are active, paused, blocked, and complete.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-19-persisted-same-session-goal-domain.md](../02-notes/implemented/feature/2026-07-19-persisted-same-session-goal-domain.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-19-persisted-same-session-goal-domain.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-19-persisted-same-session-goal-domain.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/goal/goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/goal/goal`. Core file in the package named by the note: `packages/goal/goal`. | `named-directory-member, named-package-member` |
| [`packages/goal/goal/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/goal/goal`. Core file in the package named by the note: `packages/goal/goal`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/goal/goal/src/runtime.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/runtime.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/goal/goal`. Core file in the package named by the note: `packages/goal/goal`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/goal/goal/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/goal/goal`. Core file in the package named by the note: `packages/goal/goal`. | `named-directory-member, named-package-member` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/core/agent/src/runtime-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/runtime-types.ts) | runtime implementation | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent-loop/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/goal/goal/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/goal/goal`. Core file in the package named by the note: `packages/goal/goal`. | `named-directory-member, named-package-member` |
| [`packages/goal/goal/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/goal/goal`. Core file in the package named by the note: `packages/goal/goal`. | `named-directory-member, named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `armed` | `const` | [`packages/client/ui-directory-picker-native/src/client/flow.ts:27`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-directory-picker-native/src/client/flow.ts#L27) | `const armed = useRef(false)` |
| `blocked` | `const` | [`packages/client/ui-settings-plugins/src/client/PluginCard.tsx:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/client/PluginCard.tsx#L52) | `const blocked = !state.dirty \|\| state.invalid \|\| state.saving` |
| `active` | `let` | [`packages/core/scope/src/store.ts:47`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/store.ts#L47) | `let active = true` |
| `GOAL_CHANGE_VERSION` | `const` | [`packages/goal/goal/src/runtime.ts:8`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/runtime.ts#L8) | `export const GOAL_CHANGE_VERSION = 1` |
| `GoalRef` | `interface` | [`packages/goal/goal/src/types.ts:19`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/types.ts#L19) | `export interface GoalRef {` |
| `goals` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1798`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1798) | `const goals = presets?.serviceFor(agent, 'goals') ?? ctx.get('goals')` |
| `complete` | `const` | [`packages/jobs/tool-jobs/src/index.ts:129`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L129) | `const complete = \`${content}${suffix}\`` |

### Tests and executable evidence

- [`packages/goal/goal/tests/goal.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/tests/goal.e2e.ts) — A test under the owning area exercises or imports `maxGoalRounds`.
- [`packages/goal/goal/tests/goal.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/tests/goal.spec.ts) — A test under the owning area exercises or imports `maxGoalRounds`. A test under the owning area exercises or imports `defaultMaxGoalRounds`.
- [`packages/goal/goal/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/tests/invariant.spec.ts) — A test under the owning area exercises or imports `maxGoalRounds`.
- [`packages/goal/goal/tests/projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/tests/projection.spec.ts) — A test under the owning area exercises or imports `maxGoalRounds`. A test under the owning area exercises or imports `paused`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `dsh-agent-loop`.
- [`packages/core/agent-loop/tests/cancel.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/cancel.spec.ts) — A test under the owning area exercises or imports `blocked`.
- [`packages/core/agent-loop/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/settings.spec.ts) — A test under the owning area exercises or imports `dsh-agent-loop`.
- [`packages/core/agent-loop/tests/tool-calls.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/tool-calls.spec.ts) — A test under the owning area exercises or imports `blocked`. A test under the owning area exercises or imports `dsh-agent-loop`.
- Source verification intent: Unit coverage pins creation defaults, exact-live-agent checks, compare-and-set rejection, every lifecycle transition, blocker reason validation and retention, cap enforcement on resume, clear/replacement, seeded replay and SessionStore.fork() inheritance, session-start and lifecycle-owner disarming, active-goal rearming, durable event folding, inbox independence, stable corrupt-event replay, service/listener disposal, listener containment, backward-clock clamping, strict record decoding, lifecycle continuity, and sequential round attribution.

## How to read the implementation

1. Start with [`packages/goal/goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/jobs-tasks`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`
- Aliases: `armed`, `blocked`, `active`, `GOAL_CHANGE_VERSION`, `GoalRef`, `goals`, `complete`, `@deepseek-ai/dsh-goal`, `packages/goal/goal/`, `ctx.goals`, `maxGoalRounds`, `defaultMaxGoalRounds`, `goal/change`, `user/message`
- Regex: `(?i)(armed|blocked|active|GOAL_CHANGE_VERSION|GoalRef|goals|complete|@deepseek\-ai/dsh\-goal)`

```bash
rg -n --pcre2 "(?i)(armed|blocked|active|GOAL_CHANGE_VERSION|GoalRef|goals|complete|@deepseek\\-ai/dsh\\-goal)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0090. Goal-owned durable events](0090-goal-owned-durable-events.md): The source note links to this decision directly.
- **`shares-code-with`** — [0164. Same-session goal-round driver](0164-same-session-goal-round-driver.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0157. Harness-level goal-based execution](0157-harness-level-goal-based-execution.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/goal/goal/src/index.ts`.
- **`shares-code-with`** — [0563. Repeat-tool-call guard plugin](0563-repeat-tool-call-guard-plugin.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/agent-loop/src/invariant.ts`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0092. Typert Gateway Targeted Method Calls](0092-typert-gateway-targeted-method-calls.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0042. Explicit turn cancellation capability](0042-explicit-turn-cancellation-capability.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0047. Package-owned invariant service contract](0047-package-owned-invariant-service-contract.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/agent-loop/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0162-persisted-same-session-goal-domain.md`.
