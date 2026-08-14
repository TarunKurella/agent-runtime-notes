---
id: "dsh-note-0647"
title: "the installer adopts an existing checkout into the managed layout"
status: "archived"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/process/2026-07-31-installer-adopts-existing-checkout.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/shell-terminal"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/generation"
aliases:
  - "dsh"
  - "current"
  - "scripts/install.sh"
  - "~/.dsh/source/master"
  - "dsh-staging/<timestamp>"
  - "bin/dsh"
  - "dsh-upgrade"
  - "git rev-parse --git-common-dir"
  - "HEAD"
  - "$DSH_SOURCE"
  - "git rev-parse"
  - ".git"
  - "DSH_SOURCE"
  - "resolve_dir"
search_regex: "(?i)(current|scripts/install\\.sh|\\~/\\.dsh/source/master|dsh\\-staging/<timestamp>|bin/dsh|dsh\\-upgrade|git[- ]rev\\-parse[- ]\\-\\-git\\-common\\-dir|HEAD)"
---

# 0647. the installer adopts an existing checkout into the managed layout — implementation context

## Open this when

scripts/install.sh produced two incompatible installation layouts. A curl … | sh install built the managed layout --- a master clone at ~/.dsh/source/master, a staging worktree on dsh-staging/, and the stable current symlink the PATH launcher resolves through. Running the same script from a checkout instead linked dsh straight at that checkout's bin/dsh, per the earlier in-repo skip-clone decision. The direct link cannot be upgraded.

## Source decision

In-repo mode still never clones and never modifies the working tree, but it now adopts the checkout into the managed layout unconditionally. There is no opt-out: one layout serves every install. The container owns staging worktrees and current; the repository is discovered, not owned. git rev-parse --git-common-dir resolves the shared git directory behind the checkout --- for a linked worktree that is the real clone rather than the worktree itself --- and its parent is the repository that serves as the upgrade base.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/process/2026-07-31-installer-adopts-existing-checkout.md](../02-notes/archived/process/2026-07-31-installer-adopts-existing-checkout.md)
- Pinned source: [.agents/notes/archived/process/2026-07-31-installer-adopts-existing-checkout.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/process/2026-07-31-installer-adopts-existing-checkout.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `current`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `dsh`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) | package contract and examples | Core file in the package named by the note: `apps/cli`. | `named-package-member` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Core file in the package named by the note: `apps/cli`. | `named-package-member` |
| [`packages/test-support/acp-snapshot/src/normalize.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/normalize.ts) | runtime implementation | Contains the exact code literal `private/var` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |
| `current` | `const` | [`packages/goal/goal/src/fold.ts:274`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L274) | `const current = state.goal` |

### Tests and executable evidence

- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `REPO_ROOT`.
- [`packages/hooks/hooks-claude-code/tests/coverage-cases.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-claude-code/tests/coverage-cases.ts) — Contains the exact code literal `private/var` named by the note.
- [`packages/subagent/subagent-acp/tests/subagent-acp.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/tests/subagent-acp.spec.ts) — Contains the exact code literal `private/var` named by the note.
- Source verification intent: scripts/install.sh now has a real-shell PTY regression suite in apps/cli/tests/install-script.spec.ts, covering adoption and curl-style paths with stubbed dependencies. Curl-style installs default to the public deepseek-ai/deepseek-harness-sdk source, while replacing the installer with pnpm/npx remains separate work. Verification was manual, through a throwaway harness driving the real script with a stubbed pnpm: adopting a standalone clone; adopting from a linked worktree into its existing container; an explicit DSH_SOURCE still opting back into cloning; a dirty tree adopting silently with no prompt or warning.

## How to read the implementation

1. Start with [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/process`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `domain/filesystem`, `domain/llm`, `domain/shell-terminal`, `domain/testing`, `lifecycle/archived`, `mechanism/generation`
- Aliases: `dsh`, `current`, `scripts/install.sh`, `~/.dsh/source/master`, `dsh-staging/<timestamp>`, `bin/dsh`, `dsh-upgrade`, `git rev-parse --git-common-dir`, `HEAD`, `$DSH_SOURCE`, `git rev-parse`, `.git`, `DSH_SOURCE`, `resolve_dir`
- Regex: `(?i)(current|scripts/install\.sh|\~/\.dsh/source/master|dsh\-staging/<timestamp>|bin/dsh|dsh\-upgrade|git[- ]rev\-parse[- ]\-\-git\-common\-dir|HEAD)`

```bash
rg -n --pcre2 "(?i)(current|scripts/install\\.sh|\\~/\\.dsh/source/master|dsh\\-staging/<timestamp>|bin/dsh|dsh\\-upgrade|git[- ]rev\\-parse[- ]\\-\\-git\\-common\\-dir|HEAD)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0642. installer skips the clone when run from inside a checkout](0642-installer-skips-the-clone-when-run-from-inside-a-checkout.md): The source note links to this decision directly.
- **`shares-code-with`** — [0485. Source run without a managed installer](0485-source-run-without-a-managed-installer.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0604. Versioned TUI first-run welcome](0604-versioned-tui-first-run-welcome.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0579. Product-level TUI session resume](0579-product-level-tui-session-resume.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0676. Explicit-config dsh entrypoint](0676-explicit-config-dsh-entrypoint.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0527. Artifact-first NPM baseline publication](0527-artifact-first-npm-baseline-publication.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0402. Product-first root README](0402-product-first-root-readme.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0597. `dsh meta` boots the TUI over the harness checkout](0597-dsh-meta-boots-the-tui-over-the-harness-checkout.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0647-the-installer-adopts-an-existing-checkout-into-the-managed-layout.md`.
