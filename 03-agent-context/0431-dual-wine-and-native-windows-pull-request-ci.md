---
id: "dsh-note-0431"
title: "Dual Wine and native Windows pull-request CI"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-08-native-windows-pull-request-ci.md"
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
  - "domain/filesystem"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "canonicalizeWatchPath"
  - "windows node 24 / wine blocking"
  - "ubuntu-latest"
  - "windows-native"
  - "windows node 24 / native complete"
  - "dsh-windows-2025-16core"
  - "pnpm/action-setup"
  - "pnpm run check:ci:windows-complete"
  - "all-checks-passed.needs"
  - "continue-on-error"
  - "; they now use Node's platform basename. Chokidar consumers received"
  - "ERR_INVALID_ARG_VALUE"
  - "ENOTDIR"
  - "ENOENT"
search_regex: "(?i)(canonicalizeWatchPath|windows[- ]node[- ]24[- ]/[- ]wine[- ]blocking|ubuntu\\-latest|windows\\-native|windows[- ]node[- ]24[- ]/[- ]native[- ]complete|dsh\\-windows\\-2025\\-16core|pnpm/action\\-setup|pnpm[- ]run[- ]check:ci:windows\\-complete)"
---

# 0431. Dual Wine and native Windows pull-request CI — implementation context

## Open this when

The required pull-request Windows verdict needs a fast win32 toolchain signal without making the aggregate wait for scarce Windows capacity. Wine provides that critical-path signal but runs over a Linux kernel and case-sensitive ext4, uses a hoisted dependency layout, and cannot prove NTFS, DACL, ConPTY, crash durability, or native process behavior. With the native serial references disabled, every pull-request head also needs an automatic real Windows-kernel result. A coverage audit found that stale branch state had restored temporary exclusions for supported LSP sources.

## Source decision

The required windows job in ci.yml remains windows node 24 / wine blocking on ubuntu-latest. It retains the checksum-verified Windows Node, Wine apt and pnpm caches, a hoisted install confined to a workspace snapshot, and the shared Wine gate script that runs the workspace build and production site. Node distribution transfers use bounded retries; when nodejs.org stalls on the large archive, a range-capable transport mirror resumes the same bytes, but nodejs.org remains the version and SHA-256 authority and the archive is never promoted before that checksum passes.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-08-native-windows-pull-request-ci.md](../02-notes/implemented/process/2026-08-08-native-windows-pull-request-ci.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-08-native-windows-pull-request-ci.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-08-native-windows-pull-request-ci.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`.github/workflows/ci.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/ci.yml) | repository automation | The source note names this file directly. Contains the exact code literal `dsh-windows-2025-16core` named by the note. | `exact-code-occurrence, named-file` |
| [`scripts/wine-windows-gates.sh`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/wine-windows-gates.sh) | repository automation | The source note names this file directly. | `named-file` |
| [`packages/util/home-paths/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/index.ts) | package entry point | Defines `canonicalizeWatchPath`, a construct named by the note. | `symbol-definition` |
| [`.github/workflows/e2e.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/e2e.yml) | repository automation | Contains the exact code literal `pnpm/action-setup` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/e2b-e2e.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/e2b-e2e.yml) | repository automation | Contains the exact code literal `pnpm/action-setup` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/landlock-run.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/landlock-run.yml) | repository automation | Contains the exact code literal `pnpm/action-setup` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/release-vendor.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/release-vendor.yml) | repository automation | Contains the exact code literal `pnpm/action-setup` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/landlock-run-release.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/landlock-run-release.yml) | repository automation | Contains the exact code literal `pnpm/action-setup` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/build-exe-for-python-sdk.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/build-exe-for-python-sdk.yml) | repository automation | Contains the exact code literal `pnpm/action-setup` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `canonicalizeWatchPath` | `function` | [`packages/util/home-paths/src/index.ts:33`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/index.ts#L33) | `export async function canonicalizeWatchPath(path: string): Promise<string> {` |

### Tests and executable evidence

- [`vitest.e2e.config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vitest.e2e.config.ts) — A test under the owning area exercises or imports `needs`.
- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `needs`.
- [`scripts/run-oxlint.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-oxlint.spec.ts) — A test under the owning area exercises or imports `PATH`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `windows`. A test under the owning area exercises or imports `ubuntu-latest`.
- [`scripts/test-fixture-cleanup.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-fixture-cleanup.ts) — A test under the owning area exercises or imports `ENOENT`.
- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — A test under the owning area exercises or imports `PATH`.
- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — A test under the owning area exercises or imports `PATH`.
- [`scripts/gen-client-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-client-catalog.spec.ts) — A test under the owning area exercises or imports `needs`.

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

- Tags: `class/process`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/jobs-tasks`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `canonicalizeWatchPath`, `windows node 24 / wine blocking`, `ubuntu-latest`, `windows-native`, `windows node 24 / native complete`, `dsh-windows-2025-16core`, `pnpm/action-setup`, `pnpm run check:ci:windows-complete`, `all-checks-passed.needs`, `continue-on-error`, `; they now use Node's platform basename. Chokidar consumers received`, `ERR_INVALID_ARG_VALUE`, `ENOTDIR`, `ENOENT`
- Regex: `(?i)(canonicalizeWatchPath|windows[- ]node[- ]24[- ]/[- ]wine[- ]blocking|ubuntu\-latest|windows\-native|windows[- ]node[- ]24[- ]/[- ]native[- ]complete|dsh\-windows\-2025\-16core|pnpm/action\-setup|pnpm[- ]run[- ]check:ci:windows\-complete)`

```bash
rg -n --pcre2 "(?i)(canonicalizeWatchPath|windows[- ]node[- ]24[- ]/[- ]wine[- ]blocking|ubuntu\\-latest|windows\\-native|windows[- ]node[- ]24[- ]/[- ]native[- ]complete|dsh\\-windows\\-2025\\-16core|pnpm/action\\-setup|pnpm[- ]run[- ]check:ci:windows\\-complete)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0646. Wine-run Windows blocking gates on Linux runners](0646-wine-run-windows-blocking-gates-on-linux-runners.md): The source note links to this decision directly.
- **`source-link`** — [0338. Bounded, escalating signal shutdown for Web and headless](0338-bounded-escalating-signal-shutdown-for-web-and-headless.md): The source note links to this decision directly.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0404. Portable pull-request CI recovery boundary](0404-portable-pull-request-ci-recovery-boundary.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0415. Make Lefthook installation worktree-local](0415-make-lefthook-installation-worktree-local.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/install-lefthook.spec.ts`.
- **`shares-code-with`** — [0637. Parallel GitHub CI gates](0637-parallel-github-ci-gates.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0399. Serial cross-platform CI reference](0399-serial-cross-platform-ci-reference.md): Shares source implementation: `.github/workflows/ci.yml`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0431-dual-wine-and-native-windows-pull-request-ci.md`.
