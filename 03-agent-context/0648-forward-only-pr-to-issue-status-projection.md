---
id: "dsh-note-0648"
title: "Forward-only PR-to-Issue status projection"
status: "archived"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/process/2026-08-04-forward-only-pr-issue-status.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "Inbox"
  - "check-all"
  - "ci-primary"
  - "ci-static"
  - ".github/issue-management/policy.test.mjs"
  - "scripts/run-gates.ts"
  - "Forward-only PR-to-Issue status projection"
  - "process"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "recovery"
  - "schema types"
  - "build release"
search_regex: "(?i)(Inbox|check\\-all|ci\\-primary|ci\\-static|\\.github/issue\\-management/policy\\.test\\.mjs|scripts/run\\-gates\\.ts|Forward\\-only[- ]PR\\-to\\-Issue[- ]status[- ]projection|evidence)"
---

# 0648. Forward-only PR-to-Issue status projection — implementation context

## Open this when

The Issue Project status represents the phase of the work, while an exact same-repository resolving keyword establishes the authoritative PR-to-Issue relationship. Restricting lifecycle advancement to Issues already in Ready leaves an Issue in Inbox or Backlog after implementation has demonstrably started. Requiring otherwise valid PR metadata before projecting the phase also conflates policy compliance with the work's observable state.

## Source decision

PR and PR-review events project the current PR phase to every exact same-repository resolving Issue. A draft PR, or a non-draft PR without a review request or submitted review, targets In progress. A non-draft PR with either form of review activity targets In review. The active statuses have the order Inbox, Backlog, Ready, In progress, and In review. Projection writes only when the target is later in that order. It does not move an Issue backward, alter Done or No action, or add an Issue that has no Project status.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/process/2026-08-04-forward-only-pr-issue-status.md](../02-notes/archived/process/2026-08-04-forward-only-pr-issue-status.md)
- Pinned source: [.agents/notes/archived/process/2026-08-04-forward-only-pr-issue-status.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/process/2026-08-04-forward-only-pr-issue-status.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/run-gates.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`packages/core/agent/src/inbox.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/inbox.ts) | runtime implementation | Defines `Inbox`, a construct named by the note. | `symbol-definition` |
| [`CLAUDE.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/CLAUDE.md) | package contract and examples | Contains the exact code literal `scripts/run-gates.ts` named by the note. | `exact-code-occurrence` |
| [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) | package contract and examples | Contains the exact code literal `scripts/run-gates.ts` named by the note. | `exact-code-occurrence` |
| [`package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/package.json) | composition and configuration | Contains the exact code literal `scripts/run-gates.ts` named by the note. | `exact-code-occurrence` |
| [`docs/development.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/development.md) | package contract and examples | Contains the exact code literal `scripts/run-gates.ts` named by the note. | `exact-code-occurrence` |
| [`docs/development.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/development.zh.md) | package contract and examples | Contains the exact code literal `scripts/run-gates.ts` named by the note. | `exact-code-occurrence` |
| [`scripts/snapshots/translation-prompt-v4/request-response.expected.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/snapshots/translation-prompt-v4/request-response.expected.json) | repository automation | Contains the exact code literal `scripts/run-gates.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `Inbox` | `class` | [`packages/core/agent/src/inbox.ts:25`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/inbox.ts#L25) | `export class Inbox {` |

### Tests and executable evidence

- [`.github/issue-management/policy.test.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/issue-management/policy.test.mjs) — The source note names this file directly. A test under the owning area exercises or imports `Ready`.
- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `check-all`. A test under the owning area exercises or imports `ci-primary`.
- [`apps/web/tests/steering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/steering.e2e.ts) — A test under the owning area exercises or imports `Ready`.
- [`apps/web/tests/schedule-after.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/schedule-after.e2e.ts) — A test under the owning area exercises or imports `Ready`.
- [`apps/web/tests/navigation-panes.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/navigation-panes.e2e.ts) — A test under the owning area exercises or imports `Done`.
- [`packages/core/agent/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/agent.spec.ts) — A test under the owning area exercises or imports `Inbox`.
- [`scripts/verify-doc-site-fragments.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-doc-site-fragments.spec.ts) — A test under the owning area exercises or imports `Ready`.
- [`apps/web/tests/snapshots/steering/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/steering/session.jsonl) — A test under the owning area exercises or imports `Ready`.
- Source verification intent: .github/issue-management/policy.test.mjs covers advancement from every earlier active status, the draft and review distinctions, metadata-policy independence, and protection against backward or terminal transitions. scripts/run-gates.ts owns execution of that focused policy test in top-level local and CI gate modes.

## How to read the implementation

1. Start with [`scripts/run-gates.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `lifecycle/archived`, `mechanism/policy`, `mechanism/projection`
- Aliases: `Inbox`, `check-all`, `ci-primary`, `ci-static`, `.github/issue-management/policy.test.mjs`, `scripts/run-gates.ts`, `Forward-only PR-to-Issue status projection`, `process`, `evidence`, `lifecycle`, `ownership`, `recovery`, `schema types`, `build release`
- Regex: `(?i)(Inbox|check\-all|ci\-primary|ci\-static|\.github/issue\-management/policy\.test\.mjs|scripts/run\-gates\.ts|Forward\-only[- ]PR\-to\-Issue[- ]status[- ]projection|evidence)`

```bash
rg -n --pcre2 "(?i)(Inbox|check\\-all|ci\\-primary|ci\\-static|\\.github/issue\\-management/policy\\.test\\.mjs|scripts/run\\-gates\\.ts|Forward\\-only[- ]PR\\-to\\-Issue[- ]status[- ]projection|evidence)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0404. Portable pull-request CI recovery boundary](0404-portable-pull-request-ci-recovery-boundary.md): Shares source implementation: `apps/web/tests/steering.e2e.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0438. Event-directed PR review status commands](0438-event-directed-pr-review-status-commands.md): Shares source implementation: `.github/issue-management/policy.test.mjs`, `packages/core/agent/src/inbox.ts`.
- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `scripts/run-gates.spec.ts`, `scripts/run-gates.ts`.
- **`shares-code-with`** — [0219. Steer a queued Web message into the active turn](0219-steer-a-queued-web-message-into-the-active-turn.md): Shares source implementation: `apps/web/tests/steering.e2e.ts`, `packages/core/agent/tests/agent.spec.ts`.
- **`shares-code-with`** — [0637. Parallel GitHub CI gates](0637-parallel-github-ci-gates.md): Shares source implementation: `scripts/run-gates.spec.ts`, `scripts/run-gates.ts`.
- **`shares-code-with`** — [0503. Required CI gate for web browser expected outputs](0503-required-ci-gate-for-web-browser-expected-outputs.md): Shares source implementation: `scripts/run-gates.spec.ts`, `scripts/run-gates.ts`.
- **`shares-code-with`** — [0390. Parallel pre-push gates](0390-parallel-pre-push-gates.md): Shares source implementation: `scripts/run-gates.spec.ts`, `scripts/run-gates.ts`.
- **`shares-code-with`** — [0640. doc-sync through the gate scheduler](0640-doc-sync-through-the-gate-scheduler.md): Shares source implementation: `scripts/run-gates.spec.ts`, `scripts/run-gates.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0648-forward-only-pr-to-issue-status-projection.md`.
