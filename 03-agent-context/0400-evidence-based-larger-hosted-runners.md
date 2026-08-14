---
id: "dsh-note-0400"
title: "Evidence-based larger hosted runners"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-22-evidence-based-larger-hosted-runners.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/streaming"
aliases:
  - "startedAt"
  - "suite=larger-runner-benchmark"
  - "suite=consolidated-runner-benchmark"
  - "completedAt"
  - "scripts/publint-all.ts"
  - "scripts/verify-built-package-invariants.mjs"
  - "lib/"
  - "actions/setup-node"
  - "scripts/prepare-ci-bubblewrap.sh"
  - "vm-backup"
  - "DSH_CI_FAILOVER_LINUX"
  - "pull_request"
  - "run-gates"
  - "Evidence-based larger hosted runners"
search_regex: "(?i)(startedAt|suite=larger\\-runner\\-benchmark|suite=consolidated\\-runner\\-benchmark|completedAt|scripts/publint\\-all\\.ts|scripts/verify\\-built\\-package\\-invariants\\.mjs|lib/|actions/setup\\-node)"
---

# 0400. Evidence-based larger hosted runners — implementation context

## Open this when

The shard-heavy CI topology met its latency targets by spreading primary Node work across 40 Linux jobs and Windows work across nine jobs. Most gates were shorter than checkout, runner setup, cache restore, and dependency installation, so repeated setup waves created both cost and latency variance. One hosted run finished its slowest Linux job in 49 seconds yet took 231 seconds for a Windows lint shard whose checkout, cache restore, and install alone consumed 158 seconds.

## Source decision

The enterprise keeps repo-restricted x64 larger-runner pools for Ubuntu and Windows. Ordinary pull requests name three 32-core pools directly: Ubuntu 24.04 for exhaustive coverage, Ubuntu latest for the remaining primary Node 24 inventory, and Windows 2025 for blocking Windows contracts. Public IPs are disabled, and workflow concurrency remains bounded because an autoscaling ceiling neither allocates idle machines nor makes repository work scale without limit. The required primary path depends on those enterprise pools.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-22-evidence-based-larger-hosted-runners.md](../02-notes/implemented/process/2026-07-22-evidence-based-larger-hosted-runners.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-22-evidence-based-larger-hosted-runners.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-22-evidence-based-larger-hosted-runners.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/publint-all.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/prepare-ci-bubblewrap.sh`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/prepare-ci-bubblewrap.sh) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/verify-built-package-invariants.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-built-package-invariants.mjs) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/run-gates.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts) | repository automation | Defines `startedAt`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `startedAt`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/layout.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts) | runtime implementation | Defines `startedAt`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx) | runtime implementation | Defines `startedAt`, a construct named by the note. | `symbol-definition` |
| [`package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/package.json) | composition and configuration | Contains the exact code literal `scripts/publint-all.ts` named by the note. Contains the exact code literal `scripts/verify-built-package-invariants.mjs` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/ci.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/ci.yml) | repository automation | Contains the exact code literal `actions/setup-node` named by the note. Contains the exact code literal `scripts/prepare-ci-bubblewrap.sh` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/e2e.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/e2e.yml) | repository automation | Contains the exact code literal `actions/setup-node` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/e2b-e2e.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/e2b-e2e.yml) | repository automation | Contains the exact code literal `actions/setup-node` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/sandbox.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/sandbox.yml) | repository automation | Contains the exact code literal `actions/setup-node` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `startedAt` | `const` | [`packages/client/connection/src/client/fixture.ts:2022`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L2022) | `const startedAt = Date.now()` |
| `startedAt` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx:74`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx#L74) | `const startedAt = cell.startedAt === null \|\| !Number.isFinite(cell.startedAt)` |
| `startedAt` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:149`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L149) | `const startedAt = finiteTime(result.callTime)` |
| `startedAt` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:153`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L153) | `const startedAt = finiteTime(call.time)` |
| `startedAt` | `const` | [`scripts/run-gates.ts:89`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts#L89) | `const startedAt = performance.now()` |

### Tests and executable evidence

- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `run-gates`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `master`. A test under the owning area exercises or imports `vm-backup`.
- [`scripts/change-scope.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/change-scope.spec.ts) — A test under the owning area exercises or imports `master`.
- [`scripts/translation-pairing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing.spec.ts) — A test under the owning area exercises or imports `master`.
- [`.github/issue-management/policy.test.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/issue-management/policy.test.mjs) — A test under the owning area exercises or imports `pull_request`.
- [`scripts/translation-pairing-merge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing-merge.spec.ts) — A test under the owning area exercises or imports `master`.
- [`packages/client/ui-trajectory/tests/views.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/tests/views.client.spec.tsx) — A test under the owning area exercises or imports `startedAt`. A test under the owning area exercises or imports `completedAt`.
- [`packages/client/ui-trajectory/tests/layout.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/tests/layout.client.spec.tsx) — A test under the owning area exercises or imports `startedAt`. A test under the owning area exercises or imports `completedAt`.

## How to read the implementation

1. Start with [`scripts/publint-all.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/process`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/jobs-tasks`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/streaming`
- Aliases: `startedAt`, `suite=larger-runner-benchmark`, `suite=consolidated-runner-benchmark`, `completedAt`, `scripts/publint-all.ts`, `scripts/verify-built-package-invariants.mjs`, `lib/`, `actions/setup-node`, `scripts/prepare-ci-bubblewrap.sh`, `vm-backup`, `DSH_CI_FAILOVER_LINUX`, `pull_request`, `run-gates`, `Evidence-based larger hosted runners`
- Regex: `(?i)(startedAt|suite=larger\-runner\-benchmark|suite=consolidated\-runner\-benchmark|completedAt|scripts/publint\-all\.ts|scripts/verify\-built\-package\-invariants\.mjs|lib/|actions/setup\-node)`

```bash
rg -n --pcre2 "(?i)(startedAt|suite=larger\\-runner\\-benchmark|suite=consolidated\\-runner\\-benchmark|completedAt|scripts/publint\\-all\\.ts|scripts/verify\\-built\\-package\\-invariants\\.mjs|lib/|actions/setup\\-node)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0399. Serial cross-platform CI reference](0399-serial-cross-platform-ci-reference.md): The source note links to this decision directly.
- **`source-link`** — [0404. Portable pull-request CI recovery boundary](0404-portable-pull-request-ci-recovery-boundary.md): The source note links to this decision directly.
- **`source-link`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): The source note links to this decision directly.
- **`source-link`** — [0420. Independent CI consumer build](0420-independent-ci-consumer-build.md): The source note links to this decision directly.
- **`shares-code-with`** — [0637. Parallel GitHub CI gates](0637-parallel-github-ci-gates.md): Shares source implementation: `scripts/publint-all.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0194. Trajectory inspection ledger](0194-trajectory-inspection-ledger.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0503. Required CI gate for web browser expected outputs](0503-required-ci-gate-for-web-browser-expected-outputs.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/run-gates.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0400-evidence-based-larger-hosted-runners.md`.
