---
id: "dsh-note-0090"
title: "Goal-owned durable events"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-31-goal-owned-durable-events.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "goal"
  - "GoalMessageSource"
  - "GoalService"
  - "roundsStarted"
  - "@deepseek-ai/dsh-goal"
  - "goal/change"
  - "goal/changed"
  - "user/message"
  - "@deepseek-ai/dsh-goal-round-driver"
  - "<goal_state>"
  - "Goal-owned durable events"
  - "architecture"
  - "boundary"
  - "cancellation timeout"
search_regex: "(?i)(goal|GoalMessageSource|GoalService|roundsStarted|@deepseek\\-ai/dsh\\-goal|goal/change|goal/changed|user/message)"
---

# 0090. Goal-owned durable events — implementation context

## Open this when

Goal state and inbox state have different lifecycles. A goal mutation must survive restart and fork whether or not any related model context is admitted, while an inbox message may be edited, claimed, rejected, or discarded as part of step scheduling. Encoding a goal mutation inside a round-zero inbox message made queue placement the domain commit point and required replay to reconcile insertion, admission, message identity, source metadata, and rendered content. The goal domain needs durable state, but it does not need ownership of pending model input.

## Source decision

@deepseek-ai/dsh-goal owns a durable goal/change session event. Each event carries the complete post-mutation goal snapshot or a revisioned clear tombstone. GoalService appends that event synchronously, then emits goal/changed; strict replay and the goal session projection fold only goal/change for lifecycle state. GoalMessageSource identifies only positive admitted continuation rounds. A matching user/message advances roundsStarted; ordinary user messages and inbox splice events do not change goal state. The goal package never inserts, claims, removes, or inspects inbox messages.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-31-goal-owned-durable-events.md](../02-notes/implemented/architecture/2026-07-31-goal-owned-durable-events.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-31-goal-owned-durable-events.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-31-goal-owned-durable-events.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/goal/goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts) | package entry point | Core file in the package named by the note: `packages/goal/goal`. Defines `GoalService`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/goal/goal/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/goal/goal`. | `named-package-member` |
| [`packages/goal/goal/src/domain.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/domain.ts) | runtime implementation | Core file in the package named by the note: `packages/goal/goal`. Defines `GoalMessageSource`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/goal/goal/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/goal/goal`. | `named-package-member` |
| [`packages/goal/goal-round-driver/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal-round-driver/src/index.ts) | package entry point | Core file in the package named by the note: `packages/goal/goal-round-driver`. Defines `goal`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/goal/goal-round-driver/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal-round-driver/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/goal/goal-round-driver`. | `named-package-member` |
| [`packages/goal/goal`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/goal/goal-round-driver`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal-round-driver) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/workflow/tool-ralph/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts) | package entry point | Defines `roundsStarted`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/README.md) | package contract and examples | Core file in the package named by the note: `packages/goal/goal`. | `named-package-member` |
| [`packages/goal/goal/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/package.json) | composition and configuration | Core file in the package named by the note: `packages/goal/goal`. | `named-package-member` |
| [`packages/goal/goal-round-driver/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal-round-driver/README.md) | package contract and examples | Core file in the package named by the note: `packages/goal/goal-round-driver`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `goal` | `const` | [`packages/goal/goal-round-driver/src/index.ts:119`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal-round-driver/src/index.ts#L119) | `const goal = currentGoal(state)` |
| `GoalMessageSource` | `interface` | [`packages/goal/goal/src/domain.ts:47`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/domain.ts#L47) | `export interface GoalMessageSource {` |
| `GoalService` | `class` | [`packages/goal/goal/src/index.ts:183`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L183) | `export class GoalService extends TypertRemoteService {` |
| `goal` | `const` | [`packages/goal/goal/src/index.ts:259`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L259) | `const goal: GoalSnapshot = {` |
| `goal` | `const` | [`packages/goal/goal/src/index.ts:283`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L283) | `const goal: GoalSnapshot = {` |
| `goal` | `const` | [`packages/goal/goal/src/index.ts:551`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L551) | `const goal = this.view(cache)` |
| `goal` | `const` | [`packages/goal/goal/src/index.ts:562`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L562) | `const goal = cache.state.goal` |
| `roundsStarted` | `const` | [`packages/workflow/tool-ralph/src/index.ts:291`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts#L291) | `const roundsStarted = value['roundsStarted']` |

### Tests and executable evidence

- [`packages/goal/goal/tests/goal.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/tests/goal.e2e.ts) — A test under the owning area exercises or imports `roundsStarted`.
- [`packages/goal/goal/tests/goal.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/tests/goal.spec.ts) — A test under the owning area exercises or imports `GoalService`. A test under the owning area exercises or imports `roundsStarted`.
- [`packages/goal/goal/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/tests/invariant.spec.ts) — A test under the owning area exercises or imports `roundsStarted`.
- [`packages/goal/goal/tests/projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/tests/projection.spec.ts) — A test under the owning area exercises or imports `GoalService`. A test under the owning area exercises or imports `roundsStarted`.
- [`packages/workflow/tool-ralph/tests/tool-ralph.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/tests/tool-ralph.spec.ts) — A test under the owning area exercises or imports `roundsStarted`.
- [`packages/goal/goal-round-driver/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal-round-driver/tests/invariant.spec.ts) — A test under the owning area exercises or imports `roundsStarted`.
- [`packages/goal/goal-round-driver/tests/goal-round-driver.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal-round-driver/tests/goal-round-driver.spec.ts) — A test under the owning area exercises or imports `GoalService`. A test under the owning area exercises or imports `roundsStarted`.
- [`packages/core/scope/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/tests/invariant.spec.ts) — Contains the exact code literal `goal/changed` named by the note.

## How to read the implementation

1. Start with [`packages/goal/goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`
- Aliases: `goal`, `GoalMessageSource`, `GoalService`, `roundsStarted`, `@deepseek-ai/dsh-goal`, `goal/change`, `goal/changed`, `user/message`, `@deepseek-ai/dsh-goal-round-driver`, `<goal_state>`, `Goal-owned durable events`, `architecture`, `boundary`, `cancellation timeout`
- Regex: `(?i)(goal|GoalMessageSource|GoalService|roundsStarted|@deepseek\-ai/dsh\-goal|goal/change|goal/changed|user/message)`

```bash
rg -n --pcre2 "(?i)(goal|GoalMessageSource|GoalService|roundsStarted|@deepseek\\-ai/dsh\\-goal|goal/change|goal/changed|user/message)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0583. Docked web goal bar](0583-docked-web-goal-bar.md): Shares source implementation: `packages/goal/goal`, `packages/goal/goal/README.md`.
- **`shares-code-with`** — [0157. Harness-level goal-based execution](0157-harness-level-goal-based-execution.md): Shares source implementation: `packages/goal/goal/src/index.ts`, `packages/goal/goal/src/invariant.ts`.
- **`shares-code-with`** — [0164. Same-session goal-round driver](0164-same-session-goal-round-driver.md): Shares source implementation: `packages/goal/goal/src/domain.ts`, `packages/goal/goal/src/index.ts`.
- **`shares-code-with`** — [0162. Persisted same-session goal domain](0162-persisted-same-session-goal-domain.md): Shares source implementation: `packages/goal/goal/src/index.ts`, `packages/goal/goal/src/invariant.ts`.
- **`shares-code-with`** — [0160. Human `/goal` command](0160-human-goal-command.md): Shares source implementation: `packages/goal/goal/src/index.ts`, `packages/goal/goal/src/invariant.ts`.
- **`shares-code-with`** — [0092. Typert Gateway Targeted Method Calls](0092-typert-gateway-targeted-method-calls.md): Shares source implementation: `packages/goal/goal/src/index.ts`, `packages/goal/goal/src/types.ts`.
- **`shares-code-with`** — [0046. Meaningful package invariant contracts](0046-meaningful-package-invariant-contracts.md): Shares source implementation: `packages/goal/goal/src/index.ts`, `packages/goal/goal/src/types.ts`.
- **`shares-code-with`** — [0567. Dedicated full-screen TUI front door](0567-dedicated-full-screen-tui-front-door.md): Shares source implementation: `packages/workflow/tool-ralph/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0090-goal-owned-durable-events.md`.
