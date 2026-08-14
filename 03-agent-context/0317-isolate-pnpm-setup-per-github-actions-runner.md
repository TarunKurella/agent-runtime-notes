---
id: "dsh-note-0317"
title: "Isolate pnpm setup per GitHub Actions runner"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-29-pnpm-setup-runner-isolation.md"
implementation_evidence: "high"
target_anchor: "repository tests and release policy"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "pnpm/action-setup@v4"
  - "~/setup-pnpm"
  - "uv_cwd"
  - "pnpm/action-setup"
  - "dest: ${{ runner.temp }}/setup-pnpm"
  - "PNPM_CONFIG_STORE_DIR"
  - "ci.yml"
  - "HOME"
  - "Isolate pnpm setup per GitHub Actions runner"
  - "bug fix"
  - "boundary"
  - "concurrency"
  - "evidence"
  - "ownership"
search_regex: "(?i)(pnpm/action\\-setup@v4|\\~/setup\\-pnpm|uv_cwd|pnpm/action\\-setup|dest:[- ]\\$\\{\\{[- ]runner\\.temp[- ]\\}\\}/setup\\-pnpm|PNPM_CONFIG_STORE_DIR|ci\\.yml|HOME)"
---

# 0317. Isolate pnpm setup per GitHub Actions runner — implementation context

## Open this when

pnpm/action-setup@v4 defaults its install destination to ~/setup-pnpm and replaces that directory during setup. The self-hosted CI failover runs six GitHub Actions runner services under one VM user, so concurrent jobs shared the same destination. In the reproducing run, three jobs entered pnpm setup within 73 milliseconds; one setup removed another process's current working directory and two jobs failed in Node's uv_cwd initialization. A retry on another runner passed, making the failure timing-dependent rather than a repository-test regression.

## Source decision

Every pnpm/action-setup step in the primary CI workflow sets dest: ${{ runner.temp }}/setup-pnpm. Each runner service owns its temporary directory, so one setup cannot replace another runner's install directory. Persistent store reuse remains separate through PNPM_CONFIG_STORE_DIR, as established by the pnpm provisioning decision. The workflow regression test discovers every pnpm/action-setup step in ci.yml and rejects one without the runner-private destination. This keeps newly added jobs inside the same isolation boundary.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-29-pnpm-setup-runner-isolation.md](../02-notes/implemented/bug-fix/2026-07-29-pnpm-setup-runner-isolation.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-29-pnpm-setup-runner-isolation.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-29-pnpm-setup-runner-isolation.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`.github/workflows/ci.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/ci.yml) | repository automation | The source note names this file directly. Contains the exact code literal `pnpm/action-setup` named by the note. | `exact-code-occurrence, named-file` |
| [`.github/workflows/e2e.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/e2e.yml) | repository automation | Contains the exact code literal `pnpm/action-setup` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/e2b-e2e.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/e2b-e2e.yml) | repository automation | Contains the exact code literal `pnpm/action-setup` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/landlock-run.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/landlock-run.yml) | repository automation | Contains the exact code literal `pnpm/action-setup` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/release-vendor.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/release-vendor.yml) | repository automation | Contains the exact code literal `pnpm/action-setup` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/landlock-run-release.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/landlock-run-release.yml) | repository automation | Contains the exact code literal `pnpm/action-setup` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/build-exe-for-python-sdk.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/build-exe-for-python-sdk.yml) | repository automation | Contains the exact code literal `pnpm/action-setup` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `yml`.
- [`scripts/dev-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — A test under the owning area exercises or imports `yml`. A test under the owning area exercises or imports `HOME`.
- [`packages/e2b/e2b/tests/e2b.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/tests/e2b.spec.ts) — A test under the owning area exercises or imports `HOME`.
- [`scripts/cordis-config-files.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-config-files.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`apps/web/tests/workspace-management.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/workspace-management.e2e.ts) — A test under the owning area exercises or imports `HOME`.
- [`packages/e2b/fs-e2b/tests/filesystem.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/tests/filesystem.spec.ts) — A test under the owning area exercises or imports `HOME`.

## How to read the implementation

1. Start with [`.github/workflows/ci.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/ci.yml) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and release policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `domain/filesystem`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `pnpm/action-setup@v4`, `~/setup-pnpm`, `uv_cwd`, `pnpm/action-setup`, `dest: ${{ runner.temp }}/setup-pnpm`, `PNPM_CONFIG_STORE_DIR`, `ci.yml`, `HOME`, `Isolate pnpm setup per GitHub Actions runner`, `bug fix`, `boundary`, `concurrency`, `evidence`, `ownership`
- Regex: `(?i)(pnpm/action\-setup@v4|\~/setup\-pnpm|uv_cwd|pnpm/action\-setup|dest:[- ]\$\{\{[- ]runner\.temp[- ]\}\}/setup\-pnpm|PNPM_CONFIG_STORE_DIR|ci\.yml|HOME)`

```bash
rg -n --pcre2 "(?i)(pnpm/action\\-setup@v4|\\~/setup\\-pnpm|uv_cwd|pnpm/action\\-setup|dest:[- ]\\$\\{\\{[- ]runner\\.temp[- ]\\}\\}/setup\\-pnpm|PNPM_CONFIG_STORE_DIR|ci\\.yml|HOME)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0411. Provision CI pnpm via pnpm/action-setup](0411-provision-ci-pnpm-via-pnpm-action-setup.md): The source note links to this decision directly.
- **`shares-code-with`** — [0646. Wine-run Windows blocking gates on Linux runners](0646-wine-run-windows-blocking-gates-on-linux-runners.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0415. Make Lefthook installation worktree-local](0415-make-lefthook-installation-worktree-local.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/dev-web.spec.ts`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0404. Portable pull-request CI recovery boundary](0404-portable-pull-request-ci-recovery-boundary.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0421. Coverage-exempt heavy suites](0421-coverage-exempt-heavy-suites.md): Shares source implementation: `scripts/install-lefthook.spec.ts`, `scripts/oxlint-contract.spec.ts`.
- **`shares-code-with`** — [0428. Automatically compose translation pairing records](0428-automatically-compose-translation-pairing-records.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/install-lefthook.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0317-isolate-pnpm-setup-per-github-actions-runner.md`.
