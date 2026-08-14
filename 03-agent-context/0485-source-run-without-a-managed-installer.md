---
id: "dsh-note-0485"
title: "Source run without a managed installer"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-08-10-source-run-without-managed-installer.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/simplification"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/protocols"
  - "domain/security"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/generation"
aliases:
  - "json"
  - "dsh"
  - "current"
  - "package.json"
  - "apps/cli/src/bin.ts"
  - "node --import tsx/esm"
  - "NODE_USE_ENV_PROXY=1"
  - "HTTP_PROXY"
  - "HTTPS_PROXY"
  - "pnpm dsh --profile headless \"task"
  - "Source run without a managed installer"
  - "simplification"
  - "compatibility"
  - "evidence"
search_regex: "(?i)(json|current|package\\.json|apps/cli/src/bin\\.ts|node[- ]\\-\\-import[- ]tsx/esm|NODE_USE_ENV_PROXY=1|HTTP_PROXY|HTTPS_PROXY)"
---

# 0485. Source run without a managed installer — implementation context

## Open this when

A repository-owned source installer can provide a stable launcher, isolated staging worktrees, atomic upgrades, rollback storage, and shared maintenance workflows for personal customizations. It also makes the repository responsible for a second lifecycle beside the package manager: host dependency installation, credential prompting, checkout adoption, symlink ownership, staging branch coordination, upgrade recovery, and continued compatibility between the installer and bundled maintenance skills. That lifecycle is not required to run or develop DeepSeek Harness from a source checkout.

## Source decision

The repository supports source execution through its root pnpm scripts. The dsh entry in package.json launches apps/cli/src/bin.ts directly through node --import tsx/esm; artifact generation is the separate pnpm run build operation defined by the source-launch/build separation decision. The package script forwards arguments and inherits the caller's environment, including NODE_USE_ENV_PROXY=1 when a supporting Node version must honor HTTP_PROXY and HTTPS_PROXY. Users select Web with pnpm dsh web and headless execution with pnpm dsh --profile headless "task".

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-08-10-source-run-without-managed-installer.md](../02-notes/implemented/simplification/2026-08-10-source-run-without-managed-installer.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-08-10-source-run-without-managed-installer.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-08-10-source-run-without-managed-installer.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/cli/src/bin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/bin.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `current`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `dsh`, a construct named by the note. Defines `json`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) | package contract and examples | Core file in the package named by the note: `apps/cli`. | `named-package-member` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Core file in the package named by the note: `apps/cli`. | `named-package-member` |
| [`package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/package.json) | composition and configuration | Contains the exact code literal `apps/cli/src/bin.ts` named by the note. | `exact-code-occurrence` |
| [`scripts/demo-cordis.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/demo-cordis.mjs) | repository automation | Contains the exact code literal `apps/cli/src/bin.ts` named by the note. | `exact-code-occurrence` |
| [`apps/cli/reference/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/reference/README.md) | package contract and examples | Contains the exact code literal `apps/cli/src/bin.ts` named by the note. | `exact-code-occurrence` |
| [`apps/cli/reference/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/reference/README.zh.md) | package contract and examples | Contains the exact code literal `apps/cli/src/bin.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |
| `current` | `const` | [`packages/goal/goal/src/fold.ts:274`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L274) | `const current = state.goal` |

### Tests and executable evidence

- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — Contains the exact code literal `apps/cli/src/bin.ts` named by the note.
- [`apps/cli/tests/source-launch.compat.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/source-launch.compat.spec.ts) — Contains the exact code literal `apps/cli/src/bin.ts` named by the note.
- [`examples/headless-agent/tests/headless.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/headless-agent/tests/headless.snapshot.ts) — Contains the exact code literal `apps/cli/src/bin.ts` named by the note.

## How to read the implementation

1. Start with [`apps/cli/src/bin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/bin.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/compatibility`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/filesystem`, `domain/protocols`, `domain/security`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/generation`
- Aliases: `json`, `dsh`, `current`, `package.json`, `apps/cli/src/bin.ts`, `node --import tsx/esm`, `NODE_USE_ENV_PROXY=1`, `HTTP_PROXY`, `HTTPS_PROXY`, `pnpm dsh --profile headless "task`, `Source run without a managed installer`, `simplification`, `compatibility`, `evidence`
- Regex: `(?i)(json|current|package\.json|apps/cli/src/bin\.ts|node[- ]\-\-import[- ]tsx/esm|NODE_USE_ENV_PROXY=1|HTTP_PROXY|HTTPS_PROXY)`

```bash
rg -n --pcre2 "(?i)(json|current|package\\.json|apps/cli/src/bin\\.ts|node[- ]\\-\\-import[- ]tsx/esm|NODE_USE_ENV_PROXY=1|HTTP_PROXY|HTTPS_PROXY)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0492. Separate source launch from repository build](0492-separate-source-launch-from-repository-build.md): The source note links to this decision directly.
- **`shares-code-with`** — [0647. the installer adopts an existing checkout into the managed layout](0647-the-installer-adopts-an-existing-checkout-into-the-managed-layout.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0527. Artifact-first NPM baseline publication](0527-artifact-first-npm-baseline-publication.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0402. Product-first root README](0402-product-first-root-readme.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0579. Product-level TUI session resume](0579-product-level-tui-session-resume.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0604. Versioned TUI first-run welcome](0604-versioned-tui-first-run-welcome.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0676. Explicit-config dsh entrypoint](0676-explicit-config-dsh-entrypoint.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0556. Native TypeScript source launch for dsh](0556-native-typescript-source-launch-for-dsh.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0485-source-run-without-a-managed-installer.md`.
