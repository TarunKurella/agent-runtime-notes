---
id: "dsh-note-0390"
title: "Parallel pre-push gates"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-06-parallel-pre-push-gates.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/performance"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/jobs-tasks"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
aliases:
  - "doc-sync"
  - "DSH_GATE_CONCURRENCY"
  - "packages/<group>/<pkg>"
  - "availableParallelism"
  - "DSH_PUBLINT_CONCURRENCY"
  - "publint-all.ts"
  - "Parallel pre-push gates"
  - "process"
  - "boundary"
  - "concurrency"
  - "evidence"
  - "performance"
  - "schema types"
  - "build release"
search_regex: "(?i)(doc\\-sync|DSH_GATE_CONCURRENCY|packages/<group>/<pkg>|availableParallelism|DSH_PUBLINT_CONCURRENCY|publint\\-all\\.ts|Parallel[- ]pre\\-push[- ]gates|boundary)"
---

# 0390. Parallel pre-push gates — implementation context

## Open this when

Aggregate jobs such as documentation synchronization hide long sequential chains whose members are read-only and independent. Duplicating their leaf inventory in workflow YAML gives future script changes multiple places to drift, while running package publication checks serially makes one gate consume time proportional to the package count.

## Source decision

scripts/run-gates.ts owns the bounded scheduler used by CI, doc-sync, and the opt-in check:all command. It expands named modes into leaf gates, rejects empty or ambiguous dependency graphs before starting a child, respects artifact dependencies, buffers attributable output, reports exit and signal outcomes independently, and accepts DSH_GATE_CONCURRENCY when a caller needs a different worker bound. The Node 24 consumer job is one seven-gate mode rather than a shell-owned process pool.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-06-parallel-pre-push-gates.md](../02-notes/implemented/process/2026-07-06-parallel-pre-push-gates.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-06-parallel-pre-push-gates.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-06-parallel-pre-push-gates.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/run-gates.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/publint-all.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.ts) | repository automation | The source note names this file directly. | `named-file` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `publint`.
- [`scripts/publint-all.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `publint`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`apps/web/tests/workspace-management.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/workspace-management.e2e.ts) — A test under the owning area exercises or imports `hygiene`.
- [`apps/web/tests/snapshots/permission-policy-context/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/permission-policy-context/session.jsonl) — A test under the owning area exercises or imports `hygiene`.
- [`packages/host/directory-picker-native/tests/win32-dialog-bindings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-native/tests/win32-dialog-bindings.spec.ts) — A test under the owning area exercises or imports `hygiene`.
- Source verification intent: scripts/run-gates.spec.ts rejects invalid graphs before the executor runs, pins the consumer inventory and dependency edges, and exercises signal termination through a real child process. scripts/publint-all.spec.ts rejects a missing public export before downstream artifact consumers run.

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

- Tags: `class/process`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/performance`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/jobs-tasks`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`
- Aliases: `doc-sync`, `DSH_GATE_CONCURRENCY`, `packages/<group>/<pkg>`, `availableParallelism`, `DSH_PUBLINT_CONCURRENCY`, `publint-all.ts`, `Parallel pre-push gates`, `process`, `boundary`, `concurrency`, `evidence`, `performance`, `schema types`, `build release`
- Regex: `(?i)(doc\-sync|DSH_GATE_CONCURRENCY|packages/<group>/<pkg>|availableParallelism|DSH_PUBLINT_CONCURRENCY|publint\-all\.ts|Parallel[- ]pre\-push[- ]gates|boundary)`

```bash
rg -n --pcre2 "(?i)(doc\\-sync|DSH_GATE_CONCURRENCY|packages/<group>/<pkg>|availableParallelism|DSH_PUBLINT_CONCURRENCY|publint\\-all\\.ts|Parallel[- ]pre\\-push[- ]gates|boundary)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0640. doc-sync through the gate scheduler](0640-doc-sync-through-the-gate-scheduler.md): The source note links to this decision directly.
- **`source-link`** — [0401. Fast local Git hooks](0401-fast-local-git-hooks.md): The source note links to this decision directly.
- **`shares-code-with`** — [0637. Parallel GitHub CI gates](0637-parallel-github-ci-gates.md): Shares source implementation: `scripts/publint-all.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `scripts/publint-all.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0523. Supply chain checks and vendor drift verification](0523-supply-chain-checks-and-vendor-drift-verification.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/publint-all.spec.ts`.
- **`shares-code-with`** — [0392. Package Model Experience contract](0392-package-model-experience-contract.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `scripts/publint-all.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0503. Required CI gate for web browser expected outputs](0503-required-ci-gate-for-web-browser-expected-outputs.md): Shares source implementation: `scripts/run-gates.spec.ts`, `scripts/run-gates.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0390-parallel-pre-push-gates.md`.
