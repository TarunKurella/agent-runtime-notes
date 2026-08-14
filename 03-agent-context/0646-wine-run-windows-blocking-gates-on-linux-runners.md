---
id: "dsh-note-0646"
title: "Wine-run Windows blocking gates on Linux runners"
status: "archived"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/process/2026-07-27-wine-windows-gates-experiment.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/performance"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "windows-2025"
  - "windows node 24 / wine blocking"
  - "ubuntu-latest"
  - "tsc -b"
  - "CreateProcess"
  - "@esbuild/win32-x64"
  - ".node"
  - "serial-windows"
  - "supportedArchitectures"
  - "run-gates"
  - "nodeLinker: hoisted"
  - "pnpm run check:windows-wine"
  - ".cache/wine-windows/"
  - "Socket open EBADF"
search_regex: "(?i)(windows\\-2025|windows[- ]node[- ]24[- ]/[- ]wine[- ]blocking|ubuntu\\-latest|tsc[- ]\\-b|CreateProcess|@esbuild/win32\\-x64|\\.node|serial\\-windows)"
---

# 0646. Wine-run Windows blocking gates on Linux runners — implementation context

## Open this when

The pull-request Windows lane exists to prove the two blocking win32 surfaces --- the workspace build and the production site --- and it ran on hosted windows-2025, the slowest job in the required matrix: 7--9 minutes against 1.5--2.5 for the Linux jobs, so the Windows VM's boot, setup, and filesystem costs dominated every pull request's critical path. The question the experiment answered: can a plain Linux runner produce an equivalent win32 signal for the blocking surfaces at Linux wall clock, so no Windows VM sits on the pull-request path at all?

## Source decision

The required pull-request windows job in ci.yml (windows node 24 / wine blocking) runs the blocking gate commands on ubuntu-latest under Wine with real Windows binaries: a checksum-verified win-x64 Node.js executes tsc -b, tsdown, and the VitePress production build, so the win32 branches of the toolchain --- backslash path handling, CreateProcess spawn semantics, PE loading of @esbuild/win32-x64, and the rolldown/rollup MSVC .node addons --- actually execute.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/process/2026-07-27-wine-windows-gates-experiment.md](../02-notes/archived/process/2026-07-27-wine-windows-gates-experiment.md)
- Pinned source: [.agents/notes/archived/process/2026-07-27-wine-windows-gates-experiment.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/process/2026-07-27-wine-windows-gates-experiment.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`.github/workflows/ci.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/ci.yml) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/wine-windows-gates.sh`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/wine-windows-gates.sh) | repository automation | The source note names this file directly. | `named-file` |
| [`.agents/skills/dsh-pre-push-checks/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-pre-push-checks/SKILL.md) | package contract and examples | The source note names this file directly. | `named-file` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/clean.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/clean.spec.ts) — A test under the owning area exercises or imports `node_modules`.
- [`scripts/dev-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.spec.ts) — A test under the owning area exercises or imports `tsdown`. A test under the owning area exercises or imports `node_modules`.
- [`apps/web/tests/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/README.md) — A test under the owning area exercises or imports `tsdown`.
- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `run-gates`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `windows`. A test under the owning area exercises or imports `ubuntu-latest`.
- [`apps/web/tests/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/README.zh.md) — A test under the owning area exercises or imports `tsdown`.
- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — A test under the owning area exercises or imports `node_modules`.
- [`scripts/test-fixture-cleanup.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-fixture-cleanup.ts) — A test under the owning area exercises or imports `node_modules`.

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

- Tags: `class/process`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/performance`, `concern/recovery`, `domain/build-release`, `domain/filesystem`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/archived`, `mechanism/policy`, `mechanism/registry`
- Aliases: `windows-2025`, `windows node 24 / wine blocking`, `ubuntu-latest`, `tsc -b`, `CreateProcess`, `@esbuild/win32-x64`, `.node`, `serial-windows`, `supportedArchitectures`, `run-gates`, `nodeLinker: hoisted`, `pnpm run check:windows-wine`, `.cache/wine-windows/`, `Socket open EBADF`
- Regex: `(?i)(windows\-2025|windows[- ]node[- ]24[- ]/[- ]wine[- ]blocking|ubuntu\-latest|tsc[- ]\-b|CreateProcess|@esbuild/win32\-x64|\.node|serial\-windows)`

```bash
rg -n --pcre2 "(?i)(windows\\-2025|windows[- ]node[- ]24[- ]/[- ]wine[- ]blocking|ubuntu\\-latest|tsc[- ]\\-b|CreateProcess|@esbuild/win32\\-x64|\\.node|serial\\-windows)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0404. Portable pull-request CI recovery boundary](0404-portable-pull-request-ci-recovery-boundary.md): Shares source implementation: `.github/workflows/ci.yml`, `apps/web/tests/README.zh.md`.
- **`shares-code-with`** — [0317. Isolate pnpm setup per GitHub Actions runner](0317-isolate-pnpm-setup-per-github-actions-runner.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0382. Classify Agent Notes by kind via path-encoded subdirectories](0382-classify-agent-notes-by-kind-via-path-encoded-subdirectories.md): Shares source implementation: `apps/web/tests/README.md`, `apps/web/tests/README.zh.md`.
- **`shares-code-with`** — [0415. Make Lefthook installation worktree-local](0415-make-lefthook-installation-worktree-local.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/dev-web.spec.ts`.
- **`shares-code-with`** — [0411. Provision CI pnpm via pnpm/action-setup](0411-provision-ci-pnpm-via-pnpm-action-setup.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/dev-web.spec.ts`.
- **`shares-code-with`** — [0441. Python public publication workflow](0441-python-public-publication-workflow.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/clean.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0646-wine-run-windows-blocking-gates-on-linux-runners.md`.
