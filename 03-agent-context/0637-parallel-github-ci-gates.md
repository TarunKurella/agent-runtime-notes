---
id: "dsh-note-0637"
title: "Parallel GitHub CI gates"
status: "archived"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/process/2026-07-06-parallel-github-ci-gates.md"
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
  - "concern/ownership"
  - "concern/performance"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/jobs-tasks"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "files"
  - "verify-node-next-types"
  - "lib/"
  - "scripts/static-shards.ts"
  - "scripts/snapshot-shards.ts"
  - "verify-package-invariants"
  - "Parallel GitHub CI gates"
  - "process"
  - "boundary"
  - "cancellation timeout"
  - "compatibility"
  - "concurrency"
  - "discovery routing"
  - "evidence"
search_regex: "(?i)(files|verify\\-node\\-next\\-types|lib/|scripts/static\\-shards\\.ts|scripts/snapshot\\-shards\\.ts|verify\\-package\\-invariants|Parallel[- ]GitHub[- ]CI[- ]gates|boundary)"
---

# 0637. Parallel GitHub CI gates — implementation context

## Open this when

The keyless GitHub CI gates are mostly orthogonal: typecheck, lint, documentation freshness, coverage, snapshot replay, build, package-publication hygiene, demo smoke, and built-bin smoke fail for different reasons and do not need each other's runtime state. Running them as one ordered command chain makes the workflow wall clock equal the sum of those gates, while splitting every short leaf into its own GitHub job repeats checkout, Node setup, pnpm restore, and install work until orchestration overhead becomes the bottleneck. The original broad-lane split stopped meeting that balance as the workspace grew.

## Source decision

The production topology below is historical and is superseded by Evidence-based larger hosted runners. The larger-runner decision removes its shard selectors and workflow jobs; this note preserves why that earlier topology was implemented. CI treats one minute for non-Windows jobs and three minutes for Windows jobs as observed performance targets, not cancellation deadlines. Hosted-runner variance should leave complete timing evidence and useful failure logs instead of cancelling an otherwise-correct gate.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/process/2026-07-06-parallel-github-ci-gates.md](../02-notes/archived/process/2026-07-06-parallel-github-ci-gates.md)
- Pinned source: [.agents/notes/archived/process/2026-07-06-parallel-github-ci-gates.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/process/2026-07-06-parallel-github-ci-gates.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/run-gates.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/publint-all.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`.github/workflows/ci.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/ci.yml) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/verify-built-package-invariants.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-built-package-invariants.mjs) | repository automation | The source note names this file directly. | `named-file` |
| [`packages/typert/generator/src/analyzer.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts) | runtime implementation | Defines `files`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/workspace.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/workspace.ts) | runtime implementation | Defines `files`, a construct named by the note. | `symbol-definition` |
| [`packages/e2b/subprocess-e2b/src/process.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/process.ts) | runtime implementation | Defines `files`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs-search/src/presentation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/presentation.ts) | runtime implementation | Defines `files`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/acp-snapshot/src/suite.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts) | runtime implementation | Defines `files`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `files` | `const` | [`packages/e2b/subprocess-e2b/src/process.ts:409`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/process.ts#L409) | `const files = [` |
| `files` | `const` | [`packages/fs/tool-fs-search/src/presentation.ts:110`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/presentation.ts#L110) | `const files = [...meta.files]` |
| `files` | `const` | [`packages/test-support/acp-snapshot/src/suite.ts:1455`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts#L1455) | `const files = (await readdir(dir, { withFileTypes: true }))` |
| `files` | `const` | [`packages/typert/generator/src/analyzer.ts:220`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L220) | `const files = new Map<string, ts.SourceFile \| undefined>()` |
| `files` | `const` | [`packages/typert/generator/src/workspace.ts:87`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/workspace.ts#L87) | `const files = Array.isArray(manifest.files) ? manifest.files : []` |

### Tests and executable evidence

- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `publint`.
- [`scripts/publint-all.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.spec.ts) — A test under the owning area exercises or imports `publint`.

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
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
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

- Tags: `class/process`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/jobs-tasks`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `files`, `verify-node-next-types`, `lib/`, `scripts/static-shards.ts`, `scripts/snapshot-shards.ts`, `verify-package-invariants`, `Parallel GitHub CI gates`, `process`, `boundary`, `cancellation timeout`, `compatibility`, `concurrency`, `discovery routing`, `evidence`
- Regex: `(?i)(files|verify\-node\-next\-types|lib/|scripts/static\-shards\.ts|scripts/snapshot\-shards\.ts|verify\-package\-invariants|Parallel[- ]GitHub[- ]CI[- ]gates|boundary)`

```bash
rg -n --pcre2 "(?i)(files|verify\\-node\\-next\\-types|lib/|scripts/static\\-shards\\.ts|scripts/snapshot\\-shards\\.ts|verify\\-package\\-invariants|Parallel[- ]GitHub[- ]CI[- ]gates|boundary)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `scripts/publint-all.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0390. Parallel pre-push gates](0390-parallel-pre-push-gates.md): Shares source implementation: `scripts/publint-all.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/publint-all.ts`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0404. Portable pull-request CI recovery boundary](0404-portable-pull-request-ci-recovery-boundary.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0503. Required CI gate for web browser expected outputs](0503-required-ci-gate-for-web-browser-expected-outputs.md): Shares source implementation: `scripts/run-gates.spec.ts`, `scripts/run-gates.ts`.
- **`shares-code-with`** — [0399. Serial cross-platform CI reference](0399-serial-cross-platform-ci-reference.md): Shares source implementation: `.github/workflows/ci.yml`.
- **`shares-code-with`** — [0646. Wine-run Windows blocking gates on Linux runners](0646-wine-run-windows-blocking-gates-on-linux-runners.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/run-gates.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0637-parallel-github-ci-gates.md`.
