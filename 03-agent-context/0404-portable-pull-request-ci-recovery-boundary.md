---
id: "dsh-note-0404"
title: "Portable pull-request CI recovery boundary"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-23-portable-required-pull-request-ci.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/observability"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "startedAt"
  - "ubuntu-latest"
  - "windows-2025"
  - "windows node 24 / wine blocking"
  - "windows node 24 / native complete"
  - "completedAt"
  - "Portable pull-request CI recovery boundary"
  - "process"
  - "boundary"
  - "compatibility"
  - "concurrency"
  - "discovery routing"
  - "evidence"
  - "ownership"
search_regex: "(?i)(startedAt|ubuntu\\-latest|windows\\-2025|windows[- ]node[- ]24[- ]/[- ]wine[- ]blocking|windows[- ]node[- ]24[- ]/[- ]native[- ]complete|completedAt|Portable[- ]pull\\-request[- ]CI[- ]recovery[- ]boundary|boundary)"
---

# 0404. Portable pull-request CI recovery boundary — implementation context

## Open this when

Required pull-request jobs assigned to organization-owned runner labels remain queued when GitHub cannot allocate those pools. The workflow is valid and standard GitHub-hosted jobs can still pass, but all checks passed never starts and an otherwise healthy pull request cannot satisfy branch protection. Billing health, a runner definition's Ready state, and a large autoscaling ceiling do not prove that a named pool can receive a job. Required correctness checks need a known portable recovery path even when the ordinary low-latency path depends on repository-external runner provisioning.

## Source decision

CI runs the required primary Node 24 jobs, plus the stable all checks passed aggregate, on repo-restricted enterprise 32-core pools. The aggregate performs no checkout or repository gate, but sharing the enterprise pool prevents the required verdict from introducing a separate standard-hosted billing dependency after its substantive jobs have already succeeded. The required Windows job runs Windows Node under Wine on standard ubuntu-latest for the blocking surfaces; an independent native windows-2025 job starts automatically but does not participate in the aggregate (dual Windows decision).

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-23-portable-required-pull-request-ci.md](../02-notes/implemented/process/2026-07-23-portable-required-pull-request-ci.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-23-portable-required-pull-request-ci.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-23-portable-required-pull-request-ci.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`.github/workflows/ci.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/ci.yml) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/run-gates.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts) | repository automation | Defines `startedAt`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `startedAt`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/layout.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts) | runtime implementation | Defines `startedAt`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx) | runtime implementation | Defines `startedAt`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `startedAt` | `const` | [`packages/client/connection/src/client/fixture.ts:2022`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L2022) | `const startedAt = Date.now()` |
| `startedAt` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx:74`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx#L74) | `const startedAt = cell.startedAt === null \|\| !Number.isFinite(cell.startedAt)` |
| `startedAt` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:149`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L149) | `const startedAt = finiteTime(result.callTime)` |
| `startedAt` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:153`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L153) | `const startedAt = finiteTime(call.time)` |
| `startedAt` | `const` | [`scripts/run-gates.ts:89`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts#L89) | `const startedAt = performance.now()` |

### Tests and executable evidence

- [`vitest.e2e.config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vitest.e2e.config.ts) — A test under the owning area exercises or imports `e2e`.
- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `e2e`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `ubuntu-latest`. A test under the owning area exercises or imports `e2e`.
- [`apps/web/tests/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/README.zh.md) — A test under the owning area exercises or imports `e2e`.
- [`apps/web/tests/steering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/steering.e2e.ts) — A test under the owning area exercises or imports `Ready`.
- [`apps/web/tests/default-model.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/default-model.e2e.ts) — A test under the owning area exercises or imports `e2e`.
- [`apps/web/tests/queue-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/queue-actions.e2e.ts) — A test under the owning area exercises or imports `e2e`.
- [`apps/web/tests/schedule-after.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/schedule-after.e2e.ts) — A test under the owning area exercises or imports `Ready`. A test under the owning area exercises or imports `e2e`.

## How to read the implementation

1. Start with [`.github/workflows/ci.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/ci.yml) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/observability`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `startedAt`, `ubuntu-latest`, `windows-2025`, `windows node 24 / wine blocking`, `windows node 24 / native complete`, `completedAt`, `Portable pull-request CI recovery boundary`, `process`, `boundary`, `compatibility`, `concurrency`, `discovery routing`, `evidence`, `ownership`
- Regex: `(?i)(startedAt|ubuntu\-latest|windows\-2025|windows[- ]node[- ]24[- ]/[- ]wine[- ]blocking|windows[- ]node[- ]24[- ]/[- ]native[- ]complete|completedAt|Portable[- ]pull\-request[- ]CI[- ]recovery[- ]boundary|boundary)`

```bash
rg -n --pcre2 "(?i)(startedAt|ubuntu\\-latest|windows\\-2025|windows[- ]node[- ]24[- ]/[- ]wine[- ]blocking|windows[- ]node[- ]24[- ]/[- ]native[- ]complete|completedAt|Portable[- ]pull\\-request[- ]CI[- ]recovery[- ]boundary|boundary)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0399. Serial cross-platform CI reference](0399-serial-cross-platform-ci-reference.md): The source note links to this decision directly.
- **`source-link`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): The source note links to this decision directly.
- **`source-link`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): The source note links to this decision directly.
- **`source-link`** — [0505. Required Python runtime pull-request validation](0505-required-python-runtime-pull-request-validation.md): The source note links to this decision directly.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `.github/workflows/ci.yml`, `packages/client/connection/src/client/fixture.ts`.
- **`shares-code-with`** — [0194. Trajectory inspection ledger](0194-trajectory-inspection-ledger.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx`.
- **`shares-code-with`** — [0646. Wine-run Windows blocking gates on Linux runners](0646-wine-run-windows-blocking-gates-on-linux-runners.md): Shares source implementation: `.github/workflows/ci.yml`, `apps/web/tests/README.zh.md`.
- **`shares-code-with`** — [0637. Parallel GitHub CI gates](0637-parallel-github-ci-gates.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/run-gates.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0404-portable-pull-request-ci-recovery-boundary.md`.
