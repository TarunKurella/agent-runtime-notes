---
id: "dsh-note-0640"
title: "doc-sync through the gate scheduler"
status: "archived"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/process/2026-07-21-doc-sync-through-gate-scheduler.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/jobs-tasks"
  - "domain/shell-terminal"
  - "lifecycle/archived"
  - "mechanism/policy"
aliases:
  - "json"
  - "docSyncLeafGates"
  - "pnpm run doc-sync"
  - "verify-cordis-api"
  - "doc-sync"
  - "package.json"
  - "tsx scripts/run-gates.ts doc-sync"
  - "run-gates.ts"
  - "ts.Program"
  - "DSH_GATE_CONCURRENCY"
  - "scripts/doc-sync.ts"
  - "tsx scripts/*.ts"
  - "verify-*"
  - "run-gates:"
search_regex: "(?i)(json|docSyncLeafGates|pnpm[- ]run[- ]doc\\-sync|verify\\-cordis\\-api|doc\\-sync|package\\.json|tsx[- ]scripts/run\\-gates\\.ts[- ]doc\\-sync|run\\-gates\\.ts)"
---

# 0640. doc-sync through the gate scheduler — implementation context

## Open this when

pnpm run doc-sync was a && chain of 24 pnpm run subcommands. Each link paid a full pnpm wrapper start (workspace resolution, script lookup, tsx boot) before its script ran; measured on a development host, the 24 script bodies together finish in about 34 seconds while the chained form takes around 3 minutes, and the wrapper stall reproduces on local disk, so every developer and CI lane pays it, not just network-filesystem checkouts.

## Source decision

doc-sync in package.json delegates to the existing bounded scheduler --- tsx scripts/run-gates.ts doc-sync --- like the check:ci: scripts (parallel gate scheduling, current CI topology). The doc-sync mode expands to exactly docSyncLeafGates(), making the leaf list in run-gates.ts the single source of truth for the member set. The local mode caps default concurrency at four workers because several doc gates each build a full ts.Program; DSH_GATE_CONCURRENCY still overrides.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/process/2026-07-21-doc-sync-through-gate-scheduler.md](../02-notes/archived/process/2026-07-21-doc-sync-through-gate-scheduler.md)
- Pinned source: [.agents/notes/archived/process/2026-07-21-doc-sync-through-gate-scheduler.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/process/2026-07-21-doc-sync-through-gate-scheduler.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/run-gates.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts) | repository automation | The source note names this file directly. Defines `docSyncLeafGates`, a construct named by the note. | `named-file, symbol-definition` |
| [`scripts/package-graph.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-graph.ts) | repository automation | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/layout.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts) | runtime implementation | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/extensions/cordis-host-runner/src/guard.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/guard.ts) | runtime implementation | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx) | runtime implementation | Defines `json`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `json` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx:1516`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx#L1516) | `const json = parseJsonContainer(value)` |
| `json` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:851`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L851) | `const json = JSON.stringify(value, null, 2)` |
| `json` | `const` | [`packages/extensions/cordis-host-runner/src/guard.ts:525`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/guard.ts#L525) | `const json = JSON.stringify(value)` |
| `json` | `const` | [`scripts/package-graph.ts:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-graph.ts#L37) | `const json = JSON.parse(readFileSync(resolve(root, rel), 'utf8')) as {` |
| `docSyncLeafGates` | `function` | [`scripts/run-gates.ts:571`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts#L571) | `function docSyncLeafGates(options: {` |

### Tests and executable evidence

- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`apps/web/tests/pwsh-terminal.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/pwsh-terminal.e2e.ts) — A test under the owning area exercises or imports `Program`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`packages/shell/tool-pwsh/tests/loader.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/tests/loader.spec.ts) — A test under the owning area exercises or imports `Program`.
- [`packages/shell/pwsh-local/tests/executor.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/tests/executor.spec.ts) — A test under the owning area exercises or imports `Program`.
- [`packages/shell/tool-pwsh/tests/integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/tests/integration.spec.ts) — A test under the owning area exercises or imports `Program`.
- [`packages/subagent/subagent-claude-code/tests/subagent-claude-code.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-claude-code/tests/subagent-claude-code.spec.ts) — A test under the owning area exercises or imports `Program`.
- [`packages/client/ui-directory-picker-browse/tests/directory-browser.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-directory-picker-browse/tests/directory-browser.client.spec.tsx) — A test under the owning area exercises or imports `Program`.

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

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
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

- Tags: `class/process`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `domain/build-release`, `domain/extensions`, `domain/filesystem`, `domain/jobs-tasks`, `domain/shell-terminal`, `lifecycle/archived`, `mechanism/policy`
- Aliases: `json`, `docSyncLeafGates`, `pnpm run doc-sync`, `verify-cordis-api`, `doc-sync`, `package.json`, `tsx scripts/run-gates.ts doc-sync`, `run-gates.ts`, `ts.Program`, `DSH_GATE_CONCURRENCY`, `scripts/doc-sync.ts`, `tsx scripts/*.ts`, `verify-*`, `run-gates:`
- Regex: `(?i)(json|docSyncLeafGates|pnpm[- ]run[- ]doc\-sync|verify\-cordis\-api|doc\-sync|package\.json|tsx[- ]scripts/run\-gates\.ts[- ]doc\-sync|run\-gates\.ts)`

```bash
rg -n --pcre2 "(?i)(json|docSyncLeafGates|pnpm[- ]run[- ]doc\\-sync|verify\\-cordis\\-api|doc\\-sync|package\\.json|tsx[- ]scripts/run\\-gates\\.ts[- ]doc\\-sync|run\\-gates\\.ts)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0441. Python public publication workflow](0441-python-public-publication-workflow.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`.
- **`shares-code-with`** — [0411. Provision CI pnpm via pnpm/action-setup](0411-provision-ci-pnpm-via-pnpm-action-setup.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`.
- **`shares-code-with`** — [0377. Mechanical quality gates over prose guidelines](0377-mechanical-quality-gates-over-prose-guidelines.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`.
- **`shares-code-with`** — [0417. Oxlint as the repository linter](0417-oxlint-as-the-repository-linter.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/layout.ts`.
- **`shares-code-with`** — [0424. Exact uncovered locations on coverage failure](0424-exact-uncovered-locations-on-coverage-failure.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/layout.ts`.
- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `packages/client/ui-trajectory/src/client/layout.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0392. Package Model Experience contract](0392-package-model-experience-contract.md): Shares source implementation: `packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`, `scripts/gen-third-party-notices.spec.ts`.
- **`shares-code-with`** — [0390. Parallel pre-push gates](0390-parallel-pre-push-gates.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0640-doc-sync-through-the-gate-scheduler.md`.
