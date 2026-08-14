---
id: "dsh-note-0296"
title: "Status-driven disclosure for workflow runs"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-11-workflow-run-status-driven-disclosure.md"
implementation_evidence: "medium"
target_anchor: "repository tests and release policy"
tags:
  - "class/feature"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
aliases:
  - "DisclosureRow"
  - "aria-expanded"
  - "data-expandable"
  - "Status-driven disclosure for workflow runs"
  - "feature"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "build release"
  - "configuration"
  - "extensions"
  - "session state"
  - "shell terminal"
  - "storage"
search_regex: "(?i)(DisclosureRow|aria\\-expanded|data\\-expandable|Status\\-driven[- ]disclosure[- ]for[- ]workflow[- ]runs|feature|evidence|lifecycle|ownership)"
---

# 0296. Status-driven disclosure for workflow runs — implementation context

## Open this when

A durable workflow Chat node updates in place from its running prefix to a terminal record. A disclosure choice initialized only at mount can hide a newly running phase, leave completed work occupying the conversation, or bury a failed, cancelled, or interrupted member behind two collapsed levels. Making openness a pure function of completion avoids those failures but also prevents users from reopening clean history for review. The renderer already receives every required lifecycle fact from the workflow Conversation Node.

## Source decision

Each phase derives one visibility requirement from its current members. A running, failed, cancelled, or interrupted member forces that phase open; a phase whose members are all completed is clean. The workflow forces itself open when its own status requires attention or any phase is forced open, so an abnormal member remains visible even when the workflow outcome is recorded as completed. A completed sibling phase remains independently collapsible. A forced-open level renders as an expanded static row.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-11-workflow-run-status-driven-disclosure.md](../02-notes/implemented/feature/2026-08-11-workflow-run-status-driven-disclosure.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-11-workflow-run-status-driven-disclosure.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-11-workflow-run-status-driven-disclosure.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-primitives/src/DisclosureRow.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DisclosureRow.tsx) | runtime implementation | Defines `DisclosureRow`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `DisclosureRow` | `function` | [`packages/client/ui-primitives/src/DisclosureRow.tsx:33`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DisclosureRow.tsx#L33) | `export function DisclosureRow({` |

### Tests and executable evidence

- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `aria-expanded`.
- [`apps/web/tests/workflow-run.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/workflow-run.e2e.ts) — A test under the owning area exercises or imports `aria-expanded`.
- [`apps/web/tests/queue-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/queue-actions.e2e.ts) — A test under the owning area exercises or imports `aria-expanded`.
- [`apps/web/tests/todo-row.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/todo-row.snapshot.ts) — A test under the owning area exercises or imports `aria-expanded`.
- [`apps/web/tests/pwsh-terminal.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/pwsh-terminal.e2e.ts) — A test under the owning area exercises or imports `aria-expanded`.
- [`apps/web/tests/schedule-after.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/schedule-after.e2e.ts) — A test under the owning area exercises or imports `aria-expanded`.
- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — A test under the owning area exercises or imports `aria-expanded`. A test under the owning area exercises or imports `data-expandable`.
- [`apps/web/tests/skill-tool-row.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/skill-tool-row.e2e.ts) — A test under the owning area exercises or imports `aria-expanded`.
- Source verification intent: Component tests drive the same keyed workflow and phase through running, clean completion, manual review, renewed activity, repeated clean completion, zero-member completion, and each abnormal status. They also verify abnormal-member propagation, clean-sibling independence, mouse and keyboard review, continuous-clean choice retention, and the absence of false button and ARIA semantics while expansion is mandatory. The shipped Web replay observes the real workflow, worker, Session log, browser plugin graph, and child navigation.

## How to read the implementation

1. Start with [`packages/client/ui-primitives/src/DisclosureRow.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DisclosureRow.tsx) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and release policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/feature`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/projection`
- Aliases: `DisclosureRow`, `aria-expanded`, `data-expandable`, `Status-driven disclosure for workflow runs`, `feature`, `evidence`, `lifecycle`, `ownership`, `build release`, `configuration`, `extensions`, `session state`, `shell terminal`, `storage`
- Regex: `(?i)(DisclosureRow|aria\-expanded|data\-expandable|Status\-driven[- ]disclosure[- ]for[- ]workflow[- ]runs|feature|evidence|lifecycle|ownership)`

```bash
rg -n --pcre2 "(?i)(DisclosureRow|aria\\-expanded|data\\-expandable|Status\\-driven[- ]disclosure[- ]for[- ]workflow[- ]runs|feature|evidence|lifecycle|ownership)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0598. Address pending queue occurrences for edit and removal](0598-address-pending-queue-occurrences-for-edit-and-removal.md): Shares source implementation: `apps/web/tests/pwsh-terminal.e2e.ts`, `apps/web/tests/queue-actions.e2e.ts`.
- **`shares-code-with`** — [0057. Project-grouped session directories](0057-project-grouped-session-directories.md): Shares source implementation: `apps/web/tests/bash-abort-row.e2e.ts`, `apps/web/tests/pwsh-terminal.e2e.ts`.
- **`shares-code-with`** — [0429. Browser GIFs preserve one evidence chain](0429-browser-gifs-preserve-one-evidence-chain.md): Shares source implementation: `apps/web/tests/seeded-history.e2e.ts`, `apps/web/tests/smoke-real.e2e.ts`.
- **`shares-code-with`** — [0190. Allow several `in_progress` todos at once](0190-allow-several-in-progress-todos-at-once.md): Shares source implementation: `apps/web/tests/todo-row.snapshot.ts`.
- **`shares-code-with`** — [0482. Explicit Schedule time-zone boundary](0482-explicit-schedule-time-zone-boundary.md): Shares source implementation: `apps/web/tests/bash-abort-row.e2e.ts`, `apps/web/tests/schedule-after.e2e.ts`.
- **`shares-code-with`** — [0359. Pre-Plugin Theme Bootstrap](0359-pre-plugin-theme-bootstrap.md): Shares source implementation: `apps/web/tests/smoke-real.e2e.ts`.
- **`shares-code-with`** — [0320. The approval takeover shares the composer's text cap](0320-the-approval-takeover-shares-the-composer-s-text-cap.md): Shares source implementation: `apps/web/tests/smoke-real.e2e.ts`.
- **`shares-code-with`** — [0684. Adopt execa for hand-rolled test subprocess plumbing](0684-adopt-execa-for-hand-rolled-test-subprocess-plumbing.md): Shares source implementation: `apps/web/tests/smoke-real.e2e.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0296-status-driven-disclosure-for-workflow-runs.md`.
