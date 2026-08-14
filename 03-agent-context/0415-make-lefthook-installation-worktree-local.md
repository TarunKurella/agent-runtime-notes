---
id: "dsh-note-0415"
title: "Make Lefthook installation worktree-local"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-27-worktree-local-lefthook.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/security"
  - "domain/shell-terminal"
  - "domain/testing"
  - "lifecycle/implemented"
aliases:
  - "hooksPath"
  - "worktree"
  - "install-lefthook.mjs"
  - "lefthook install --force"
  - "CI=true"
  - "GITHUB_ACTIONS=true"
  - "git config --show-scope"
  - "extensions.worktreeConfig"
  - "worktreeConfig"
  - "core.hooksPath"
  - "$GIT_DIR/dsh-hooks"
  - "extensions.*"
  - "core.worktree"
  - "core.bare=true"
search_regex: "(?i)(hooksPath|worktree|install\\-lefthook\\.mjs|lefthook[- ]install[- ]\\-\\-force|CI=true|GITHUB_ACTIONS=true|git[- ]config[- ]\\-\\-show\\-scope|extensions\\.worktreeConfig)"
---

# 0415. Make Lefthook installation worktree-local — implementation context

## Open this when

Every pnpm install runs the root postinstall, whose install-lefthook.mjs invokes lefthook install --force. Linked Git worktrees otherwise share the common repository's default hooks directory, so an install in any worktree can rewrite hooks used by every other worktree. Lefthook-generated hooks prefer an absolute binary path captured from the installing worktree before trying their current-worktree fallback. Shared hooks can therefore run another worktree's pinned binary until that worktree disappears, while concurrent installs write the same files.

## Source decision

Hook installation is worktree-scoped. With CI=true or GITHUB_ACTIONS=true, the installer returns before Git discovery or mutation because automated jobs do not consume contributor hooks. Otherwise, it requires Git 2.26 or newer so git config --show-scope can report which scope supplied a value, upgrades a format-0 repository to format 1, enables extensions.worktreeConfig, and assigns the current worktree an absolute core.hooksPath at $GIT_DIR/dsh-hooks.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-27-worktree-local-lefthook.md](../02-notes/implemented/process/2026-07-27-worktree-local-lefthook.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-27-worktree-local-lefthook.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-27-worktree-local-lefthook.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/package.json) | composition and configuration | The source note names this file directly. | `named-file` |
| [`scripts/install-lefthook.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.mjs) | repository automation | The source note names this file directly. Defines `hooksPath`, a construct named by the note. | `named-file, symbol-definition` |
| [`scripts/publish-npm-baseline.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publish-npm-baseline.ts) | repository automation | Defines `worktree`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `hooksPath` | `const` | [`scripts/install-lefthook.mjs:707`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.mjs#L707) | `const hooksPath = join(gitDirectory, HOOKS_DIRECTORY)` |
| `worktree` | `const` | [`scripts/publish-npm-baseline.ts:546`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publish-npm-baseline.ts#L546) | `const worktree = DetachedWorktree.create(this.repositoryRoot, plan.commit, this.runner)` |

### Tests and executable evidence

- [`scripts/dev-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`scripts/change-scope.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/change-scope.spec.ts) — A test under the owning area exercises or imports `hooksPath`. A test under the owning area exercises or imports `worktree`.
- [`scripts/test-fixture-cleanup.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-fixture-cleanup.ts) — A test under the owning area exercises or imports `worktree`.
- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — A test under the owning area exercises or imports `worktreeConfig`. A test under the owning area exercises or imports `hooksPath`.
- [`scripts/translation-pairing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing.spec.ts) — A test under the owning area exercises or imports `worktree`.
- [`scripts/cordis-config-files.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-config-files.spec.ts) — A test under the owning area exercises or imports `yml`.

## How to read the implementation

1. Start with [`package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/package.json) because it has the strongest evidence link to the note.
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

- Tags: `class/process`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/security`, `domain/shell-terminal`, `domain/testing`, `lifecycle/implemented`
- Aliases: `hooksPath`, `worktree`, `install-lefthook.mjs`, `lefthook install --force`, `CI=true`, `GITHUB_ACTIONS=true`, `git config --show-scope`, `extensions.worktreeConfig`, `worktreeConfig`, `core.hooksPath`, `$GIT_DIR/dsh-hooks`, `extensions.*`, `core.worktree`, `core.bare=true`
- Regex: `(?i)(hooksPath|worktree|install\-lefthook\.mjs|lefthook[- ]install[- ]\-\-force|CI=true|GITHUB_ACTIONS=true|git[- ]config[- ]\-\-show\-scope|extensions\.worktreeConfig)`

```bash
rg -n --pcre2 "(?i)(hooksPath|worktree|install\\-lefthook\\.mjs|lefthook[- ]install[- ]\\-\\-force|CI=true|GITHUB_ACTIONS=true|git[- ]config[- ]\\-\\-show\\-scope|extensions\\.worktreeConfig)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0401. Fast local Git hooks](0401-fast-local-git-hooks.md): The source note links to this decision directly.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/install-lefthook.spec.ts`.
- **`shares-code-with`** — [0411. Provision CI pnpm via pnpm/action-setup](0411-provision-ci-pnpm-via-pnpm-action-setup.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/dev-web.spec.ts`.
- **`shares-code-with`** — [0373. Unlink fixture junctions before recursive deletion](0373-unlink-fixture-junctions-before-recursive-deletion.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/dev-web.spec.ts`.
- **`shares-code-with`** — [0317. Isolate pnpm setup per GitHub Actions runner](0317-isolate-pnpm-setup-per-github-actions-runner.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/dev-web.spec.ts`.
- **`shares-code-with`** — [0644. Personal staging maintenance skills](0644-personal-staging-maintenance-skills.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0526. Remove the packed-session fixture branch migrator](0526-remove-the-packed-session-fixture-branch-migrator.md): Shares source implementation: `package.json`, `scripts/change-scope.spec.ts`.
- **`shares-code-with`** — [0421. Coverage-exempt heavy suites](0421-coverage-exempt-heavy-suites.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/install-lefthook.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0415-make-lefthook-installation-worktree-local.md`.
