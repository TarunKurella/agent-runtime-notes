---
id: "dsh-note-0438"
title: "Event-directed PR review status commands"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-10-event-directed-pr-review-status.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "submitted"
  - "Inbox"
  - "state"
  - "CHANGES_REQUESTED"
  - "pull_request.review_requested"
  - "review_requested"
  - "pull_request_review.submitted"
  - "review.state"
  - "changes_requested"
  - "reviewDecision"
  - "pull_request.ready_for_review"
  - "ready_for_review"
  - "Event-directed PR review status commands"
  - "process"
search_regex: "(?i)(submitted|Inbox|state|CHANGES_REQUESTED|pull_request\\.review_requested|review_requested|pull_request_review\\.submitted|review\\.state)"
---

# 0438. Event-directed PR review status commands — implementation context

## Open this when

The Issue Project status records who owns the next step of resolving work. Aggregate pull-request review state answers whether GitHub considers the pull request mergeable, but it cannot represent that handoff: an earlier CHANGES_REQUESTED review can remain effective after the author fixes the code and requests review again. A monotonic projection also cannot return an automation-owned Issue from In review to In progress when a reviewer requests changes. Reconstructing review rounds or reviewer blockers would add state that the required two-event contract does not need.

## Source decision

The Issue lifecycle workflow treats review webhooks as commands. pull_request.review_requested, including a repeated request, targets In review. pull_request_review.submitted targets In progress only when review.state is changes_requested; the submitted event remains necessary because a reviewer can request changes without an earlier review-request event. Approved and commented submissions skip their lifecycle job before it creates a Project token, while dismissed reviews are not subscribed.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-10-event-directed-pr-review-status.md](../02-notes/implemented/process/2026-08-10-event-directed-pr-review-status.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-10-event-directed-pr-review-status.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-10-event-directed-pr-review-status.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`.github/workflows/issue-policy.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/issue-policy.yml) | repository automation | The source note names this file directly. | `named-file` |
| [`.github/workflows/issue-lifecycle.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/issue-lifecycle.yml) | repository automation | The source note names this file directly. | `named-file` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `state`, a construct named by the note. | `symbol-definition` |
| [`vendor/cosmokit/src/string.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cosmokit/src/string.ts) | runtime implementation | Defines `state`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `state`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts) | package entry point | Defines `state`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `state`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent/src/inbox.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/inbox.ts) | runtime implementation | Defines `Inbox`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal-round-driver/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal-round-driver/src/index.ts) | package entry point | Defines `submitted`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/input/facade.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/facade.ts) | runtime implementation | Defines `submitted`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `submitted` | `const` | [`packages/client/ui-conversation/src/client/input/facade.ts:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/facade.ts#L156) | `const submitted = new Set(imageIds)` |
| `Inbox` | `class` | [`packages/core/agent/src/inbox.ts:25`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/inbox.ts#L25) | `export class Inbox {` |
| `state` | `const` | [`packages/core/tools/src/index.ts:1511`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1511) | `const state = this.cancellationStates.get(exec)` |
| `submitted` | `const` | [`packages/goal/goal-round-driver/src/index.ts:350`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal-round-driver/src/index.ts#L350) | `const submitted = messages.find((message): message is UserMessage & { source: GoalMessageSource } =>` |
| `state` | `const` | [`packages/goal/goal/src/fold.ts:340`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L340) | `const state = emptyGoalFoldState()` |
| `state` | `const` | [`packages/goal/goal/src/index.ts:424`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L424) | `const state = emptyGoalFoldState()` |
| `state` | `let` | [`vendor/cosmokit/src/string.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cosmokit/src/string.ts#L24) | `let state = State.DELIM` |
| `state` | `const` | [`vendor/hmr/src/index.ts:298`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L298) | `const state = this.configRefreshes.get(key) ?? { dirty: false }` |

### Tests and executable evidence

- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `review_requested`.
- [`.github/issue-management/policy.test.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/issue-management/policy.test.mjs) — The source note names this file directly. A test under the owning area exercises or imports `review_requested`.
- [`apps/web/tests/steering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/steering.e2e.ts) — A test under the owning area exercises or imports `Ready`.
- [`apps/web/tests/schedule-after.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/schedule-after.e2e.ts) — A test under the owning area exercises or imports `Ready`.
- [`packages/core/agent/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/agent.spec.ts) — A test under the owning area exercises or imports `Inbox`.
- [`scripts/verify-doc-site-fragments.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-doc-site-fragments.spec.ts) — A test under the owning area exercises or imports `Ready`.
- [`packages/sdk/client/tests/sdk-client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/tests/sdk-client.spec.ts) — A test under the owning area exercises or imports `Resolves`.
- [`packages/core/agent/tests/consumed-work.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/consumed-work.spec.ts) — A test under the owning area exercises or imports `Inbox`.
- Source verification intent: Issue-management tests pin the event-to-command mapping, the repeated-review-request transition after a changes-requested command, the changes-requested regression, terminal protection, and human override preservation. Workflow tests pin the subscribed events, the changes-requested job condition, and the separate ready_for_review policy trigger.

## How to read the implementation

1. Start with [`.github/workflows/issue-policy.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/issue-policy.yml) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `domain/build-release`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`
- Aliases: `submitted`, `Inbox`, `state`, `CHANGES_REQUESTED`, `pull_request.review_requested`, `review_requested`, `pull_request_review.submitted`, `review.state`, `changes_requested`, `reviewDecision`, `pull_request.ready_for_review`, `ready_for_review`, `Event-directed PR review status commands`, `process`
- Regex: `(?i)(submitted|Inbox|state|CHANGES_REQUESTED|pull_request\.review_requested|review_requested|pull_request_review\.submitted|review\.state)`

```bash
rg -n --pcre2 "(?i)(submitted|Inbox|state|CHANGES_REQUESTED|pull_request\\.review_requested|review_requested|pull_request_review\\.submitted|review\\.state)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0649. Review-driven Issue lifecycle triggers](0649-review-driven-issue-lifecycle-triggers.md): Shares source implementation: `.github/issue-management/policy.test.mjs`, `.github/workflows/issue-lifecycle.yml`.
- **`shares-code-with`** — [0088. Claim inbox input before one pre-step decision](0088-claim-inbox-input-before-one-pre-step-decision.md): Shares source implementation: `packages/core/agent/src/inbox.ts`, `packages/goal/goal/src/fold.ts`.
- **`shares-code-with`** — [0454. Simplify session-log representation](0454-simplify-session-log-representation.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/goal/goal/src/index.ts`.
- **`shares-code-with`** — [0092. Typert Gateway Targeted Method Calls](0092-typert-gateway-targeted-method-calls.md): Shares source implementation: `packages/core/agent/src/inbox.ts`, `packages/goal/goal/src/index.ts`.
- **`shares-code-with`** — [0058. Separate context injection from turn execution](0058-separate-context-injection-from-turn-execution.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `vendor/cosmokit/src/string.ts`.
- **`shares-code-with`** — [0127. Trajectory assembly from registered Conversation Contexts](0127-trajectory-assembly-from-registered-conversation-contexts.md): Shares source implementation: `packages/core/tools/src/index.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0196. Web session fork actions](0196-web-session-fork-actions.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/goal/goal/src/fold.ts`.
- **`shares-code-with`** — [0342. Load sessions from the pre-react-loop format](0342-load-sessions-from-the-pre-react-loop-format.md): Shares source implementation: `packages/core/tools/src/index.ts`, `vendor/hmr/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0438-event-directed-pr-review-status-commands.md`.
