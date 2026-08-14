---
id: "dsh-note-0407"
title: "CI failover runbook --- hosted pools → in-house pool"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-26-ci-failover-runbook.md"
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
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "push"
  - "cancelled"
  - "concurrency"
  - "node 24 / static"
  - "node 24 / coverage"
  - "node 24 / snapshots and artifacts"
  - "ubuntu-latest"
  - "windows node 24 / native complete"
  - "dsh-windows-2025-16core"
  - "DSH_CI_FAILOVER_LINUX"
  - "DSH_CI_FAILOVER_WINDOWS"
  - "node-compat"
  - "python-sdk"
  - "vm-backup"
search_regex: "(?i)(push|cancelled|concurrency|node[- ]24[- ]/[- ]static|node[- ]24[- ]/[- ]coverage|node[- ]24[- ]/[- ]snapshots[- ]and[- ]artifacts|ubuntu\\-latest|windows[- ]node[- ]24[- ]/[- ]native[- ]complete)"
---

# 0407. CI failover runbook --- hosted pools → in-house pool — implementation context

## Open this when

The three required Linux worker jobs in CI (node 24 / static, node 24 / coverage, node 24 / snapshots and artifacts) run on the hosted enterprise 32-core pools; the required verdict job that aggregates them (all checks passed) runs on standard ubuntu-latest; the independent native Windows job (windows node 24 / native complete) runs on the hosted dsh-windows-2025-16core larger runner.

## Source decision

Each of the three required Linux worker jobs, the independent native Windows job, and the all checks passed verdict job --- which would otherwise stay queued on the failed pool even after every worker passed --- resolves its runner pool through a repository variable, and the switch is split by platform so an outage on one platform does not retarget the other. The three Linux workers and the all checks passed verdict (whose needs are the required Linux workers and which runs on the vm-backup pool) resolve through DSH_CI_FAILOVER_LINUX; the native Windows job resolves through DSH_CI_FAILOVER_WINDOWS.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-26-ci-failover-runbook.md](../02-notes/implemented/process/2026-07-26-ci-failover-runbook.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-26-ci-failover-runbook.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-26-ci-failover-runbook.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`.github/workflows/ci.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/ci.yml) | repository automation | The source note names this file directly. Contains the exact code literal `dsh-windows-2025-16core` named by the note. | `exact-code-occurrence, named-file` |
| [`scripts/publint-all.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.ts) | repository automation | Defines `concurrency`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `cancelled`, a construct named by the note. | `symbol-definition` |
| [`packages/schedule/schedule/src/tools.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/tools.ts) | runtime implementation | Defines `cancelled`, a construct named by the note. | `symbol-definition` |
| [`packages/context/session-reference/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/index.ts) | package entry point | Defines `cancelled`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `push`, a construct named by the note. | `symbol-definition` |
| [`.github/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/AGENTS.md) | repository automation | Contains the exact code literal `dsh-win-ci` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `push` | `const` | [`packages/client/connection/src/client/fixture.ts:361`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L361) | `const push = (e: Record<string, unknown>): number => {` |
| `cancelled` | `function` | [`packages/context/session-reference/src/index.ts:299`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/index.ts#L299) | `function cancelled(signal: AbortSignal): SessionReferenceError {` |
| `cancelled` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2037`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2037) | `const cancelled = () => err<{ items: SessionSearchItem[]; hasMore: boolean }>(request, {` |
| `cancelled` | `const` | [`packages/schedule/schedule/src/tools.ts:193`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/tools.ts#L193) | `const cancelled = cancellationPlaceholder(signal)` |
| `concurrency` | `const` | [`scripts/publint-all.ts:157`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.ts#L157) | `const concurrency = publintConcurrency(packages.length)` |

### Tests and executable evidence

- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `ubuntu-latest`.
- [`vitest.e2e.config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vitest.e2e.config.ts) — A test under the owning area exercises or imports `needs`. A test under the owning area exercises or imports `concurrency`.
- [`scripts/dev-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `node-compat`. A test under the owning area exercises or imports `needs`.
- [`scripts/run-oxlint.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-oxlint.spec.ts) — A test under the owning area exercises or imports `PATH`.
- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — A test under the owning area exercises or imports `PATH`.
- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — A test under the owning area exercises or imports `yml`. A test under the owning area exercises or imports `concurrency`.

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

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/process`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/filesystem`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`
- Aliases: `push`, `cancelled`, `concurrency`, `node 24 / static`, `node 24 / coverage`, `node 24 / snapshots and artifacts`, `ubuntu-latest`, `windows node 24 / native complete`, `dsh-windows-2025-16core`, `DSH_CI_FAILOVER_LINUX`, `DSH_CI_FAILOVER_WINDOWS`, `node-compat`, `python-sdk`, `vm-backup`
- Regex: `(?i)(push|cancelled|concurrency|node[- ]24[- ]/[- ]static|node[- ]24[- ]/[- ]coverage|node[- ]24[- ]/[- ]snapshots[- ]and[- ]artifacts|ubuntu\-latest|windows[- ]node[- ]24[- ]/[- ]native[- ]complete)`

```bash
rg -n --pcre2 "(?i)(push|cancelled|concurrency|node[- ]24[- ]/[- ]static|node[- ]24[- ]/[- ]coverage|node[- ]24[- ]/[- ]snapshots[- ]and[- ]artifacts|ubuntu\\-latest|windows[- ]node[- ]24[- ]/[- ]native[- ]complete)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0404. Portable pull-request CI recovery boundary](0404-portable-pull-request-ci-recovery-boundary.md): Shares source implementation: `.github/workflows/ci.yml`, `packages/client/connection/src/client/fixture.ts`.
- **`shares-code-with`** — [0646. Wine-run Windows blocking gates on Linux runners](0646-wine-run-windows-blocking-gates-on-linux-runners.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0637. Parallel GitHub CI gates](0637-parallel-github-ci-gates.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/publint-all.ts`.
- **`shares-code-with`** — [0496. Real-API e2e in CI against the external DeepSeek API](0496-real-api-e2e-in-ci-against-the-external-deepseek-api.md): Shares source implementation: `.github/workflows/ci.yml`, `packages/client/connection/src/client/fixture.ts`.
- **`shares-code-with`** — [0070. A capability-discriminated directory-picker seam for the web-GUI host](0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0317. Isolate pnpm setup per GitHub Actions runner](0317-isolate-pnpm-setup-per-github-actions-runner.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/ci-workflow.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0407-ci-failover-runbook-hosted-pools-in-house-pool.md`.
