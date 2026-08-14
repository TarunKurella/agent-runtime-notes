---
id: "dsh-note-0649"
title: "Review-driven Issue lifecycle triggers"
status: "archived"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/process/2026-08-08-review-driven-issue-lifecycle-triggers.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/performance"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/security"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/policy"
aliases:
  - "submitted"
  - "opened"
  - "ready_for_review"
  - "pull_request.ready_for_review"
  - "pull_request.review_requested"
  - "review_requested"
  - "pull_request_review.submitted"
  - "Review-driven Issue lifecycle triggers"
  - "process"
  - "boundary"
  - "cancellation timeout"
  - "concurrency"
  - "evidence"
  - "human control"
search_regex: "(?i)(submitted|opened|ready_for_review|pull_request\\.ready_for_review|pull_request\\.review_requested|review_requested|pull_request_review\\.submitted|Review\\-driven[- ]Issue[- ]lifecycle[- ]triggers)"
---

# 0649. Review-driven Issue lifecycle triggers — implementation context

## Open this when

The Issue lifecycle workflow reads the current pull request after each subscribed repository event and projects resolving Issues forward to In progress or In review. A resolving draft already reaches In progress from its opened event. Changing that draft to ready creates no new lifecycle outcome until a reviewer is requested or submits a review, yet subscribing to ready_for_review launches another hosted job and creates another GitHub App token. Draft-to-ready automation commonly submits a review moments later.

## Source decision

Issue lifecycle does not subscribe to pull_request.ready_for_review. It retains pull_request.review_requested and pull_request_review.submitted, so either a requested reviewer or a submitted review can advance a resolving Issue to In review. The handler continues to fetch the live pull request instead of deriving phase from the triggering payload. Issue policy still subscribes to ready_for_review. That workflow owns the required check when a human pull request enters review; removing a lifecycle trigger does not weaken policy enforcement. The workflow test parses both files and pins this split.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/process/2026-08-08-review-driven-issue-lifecycle-triggers.md](../02-notes/archived/process/2026-08-08-review-driven-issue-lifecycle-triggers.md)
- Pinned source: [.agents/notes/archived/process/2026-08-08-review-driven-issue-lifecycle-triggers.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/process/2026-08-08-review-driven-issue-lifecycle-triggers.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`.github/workflows/issue-policy.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/issue-policy.yml) | repository automation | The source note names this file directly. | `named-file` |
| [`.github/workflows/issue-lifecycle.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/issue-lifecycle.yml) | repository automation | The source note names this file directly. | `named-file` |
| [`packages/lsp/lsp-stdio/src/instance.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/instance.ts) | runtime implementation | Defines `opened`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal-round-driver/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal-round-driver/src/index.ts) | package entry point | Defines `submitted`, a construct named by the note. | `symbol-definition` |
| [`packages/sandbox/sandbox-windows-acl/src/token.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/token.ts) | runtime implementation | Defines `opened`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/input/facade.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/facade.ts) | runtime implementation | Defines `submitted`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `submitted` | `const` | [`packages/client/ui-conversation/src/client/input/facade.ts:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/facade.ts#L156) | `const submitted = new Set(imageIds)` |
| `submitted` | `const` | [`packages/goal/goal-round-driver/src/index.ts:350`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal-round-driver/src/index.ts#L350) | `const submitted = messages.find((message): message is UserMessage & { source: GoalMessageSource } =>` |
| `opened` | `let` | [`packages/lsp/lsp-stdio/src/instance.ts:154`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/instance.ts#L154) | `let opened = false` |
| `opened` | `const` | [`packages/sandbox/sandbox-windows-acl/src/token.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/token.ts#L28) | `const opened = api.openProcessToken(` |

### Tests and executable evidence

- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `ready_for_review`. A test under the owning area exercises or imports `review_requested`.
- [`.github/issue-management/policy.test.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/issue-management/policy.test.mjs) — A test under the owning area exercises or imports `review_requested`.
- [`packages/lsp/lsp-stdio/tests/lifecycle.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/lifecycle.spec.ts) — A test under the owning area exercises or imports `opened`.

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

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/performance`, `concern/simplification`, `domain/build-release`, `domain/security`, `domain/testing`, `lifecycle/archived`, `mechanism/policy`
- Aliases: `submitted`, `opened`, `ready_for_review`, `pull_request.ready_for_review`, `pull_request.review_requested`, `review_requested`, `pull_request_review.submitted`, `Review-driven Issue lifecycle triggers`, `process`, `boundary`, `cancellation timeout`, `concurrency`, `evidence`, `human control`
- Regex: `(?i)(submitted|opened|ready_for_review|pull_request\.ready_for_review|pull_request\.review_requested|review_requested|pull_request_review\.submitted|Review\-driven[- ]Issue[- ]lifecycle[- ]triggers)`

```bash
rg -n --pcre2 "(?i)(submitted|opened|ready_for_review|pull_request\\.ready_for_review|pull_request\\.review_requested|review_requested|pull_request_review\\.submitted|Review\\-driven[- ]Issue[- ]lifecycle[- ]triggers)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0438. Event-directed PR review status commands](0438-event-directed-pr-review-status-commands.md): Shares source implementation: `.github/issue-management/policy.test.mjs`, `.github/workflows/issue-lifecycle.yml`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0455. Remove implicit batching from ordinary sends](0455-remove-implicit-batching-from-ordinary-sends.md): Shares source implementation: `packages/lsp/lsp-stdio/src/instance.ts`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0469. Keep agent routing private](0469-keep-agent-routing-private.md): Shares source implementation: `packages/lsp/lsp-stdio/src/instance.ts`.
- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0090. Goal-owned durable events](0090-goal-owned-durable-events.md): Shares source implementation: `packages/goal/goal-round-driver/src/index.ts`.
- **`shares-code-with`** — [0646. Wine-run Windows blocking gates on Linux runners](0646-wine-run-windows-blocking-gates-on-linux-runners.md): Shares source implementation: `scripts/ci-workflow.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0649-review-driven-issue-lifecycle-triggers.md`.
