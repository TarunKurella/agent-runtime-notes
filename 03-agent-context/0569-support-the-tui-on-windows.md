---
id: "dsh-note-0569"
title: "Support the TUI on Windows"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-20-windows-tui-support.md"
implementation_evidence: "lead-only"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/evidence"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
aliases:
  - "ProcessTerminal"
  - "@deepseek-ai/dsh-tui"
  - "SIGWINCH"
  - "node-pty"
  - "pnpm-workspace.yaml"
  - "Support the TUI on Windows"
  - "feature"
  - "boundary"
  - "cancellation timeout"
  - "evidence"
  - "recovery"
  - "build release"
  - "filesystem"
  - "shell terminal"
search_regex: "(?i)(ProcessTerminal|@deepseek\\-ai/dsh\\-tui|SIGWINCH|node\\-pty|pnpm\\-workspace\\.yaml|Support[- ]the[- ]TUI[- ]on[- ]Windows|feature|boundary)"
---

# 0569. Support the TUI on Windows — implementation context

## Open this when

The full-screen TUI delegates raw input, ANSI rendering, resize events, and terminal restoration to pi-tui's ProcessTerminal. That dependency contains a native Windows console path, but the repository's real-process smoke used Python's POSIX-only pty and termios modules. Skipping that smoke on Windows would leave the supported product path without coverage for startup, input, interaction, failure reporting, or restoration. The TUI platform contract must follow the runtime shipped to users rather than the portability of one test driver.

## Source decision

@deepseek-ai/dsh-tui supports interactive terminals on Windows as well as macOS and Linux. The product continues to use pi-tui's ProcessTerminal; on Windows it enables virtual-terminal input after raw mode and avoids the Unix-only SIGWINCH refresh. DeepSeek Harness adds no platform rejection or reduced Windows mode. The real Loader smoke selects a native pseudo-terminal boundary by host. macOS and Linux retain the Python POSIX PTY driver. Windows uses node-pty and ConPTY.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-20-windows-tui-support.md](../02-notes/archived/feature/2026-07-20-windows-tui-support.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-20-windows-tui-support.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-20-windows-tui-support.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

No implementation path could be confirmed from the pinned tree. Use the search handles below; do not invent a source location.

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `node-pty`.
- [`apps/cli/tests/headless-shutdown.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/headless-shutdown.e2e.ts) — A test under the owning area exercises or imports `pty`.
- [`examples/acp-agent/tests/acp.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/acp.snapshot.ts) — A test under the owning area exercises or imports `pty`.
- [`python/sdk/tests/test_runtime_resolution.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_runtime_resolution.py) — A test under the owning area exercises or imports `node-pty`.
- [`packages/e2b/subprocess-e2b/tests/terminal.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/tests/terminal.spec.ts) — A test under the owning area exercises or imports `pty`.
- [`examples/headless-agent/tests/headless.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/headless-agent/tests/headless.snapshot.ts) — A test under the owning area exercises or imports `pty`.
- [`packages/terminal/terminal-bash/tests/local.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal-bash/tests/local.spec.ts) — A test under the owning area exercises or imports `termios`.
- [`packages/subprocess/subprocess-local/tests/local.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/tests/local.spec.ts) — A test under the owning area exercises or imports `node-pty`.

## How to read the implementation

1. Start from the source note and run the regex below across the pinned repository.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`, `concern/recovery`, `domain/build-release`, `domain/filesystem`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`
- Aliases: `ProcessTerminal`, `@deepseek-ai/dsh-tui`, `SIGWINCH`, `node-pty`, `pnpm-workspace.yaml`, `Support the TUI on Windows`, `feature`, `boundary`, `cancellation timeout`, `evidence`, `recovery`, `build release`, `filesystem`, `shell terminal`
- Regex: `(?i)(ProcessTerminal|@deepseek\-ai/dsh\-tui|SIGWINCH|node\-pty|pnpm\-workspace\.yaml|Support[- ]the[- ]TUI[- ]on[- ]Windows|feature|boundary)`

```bash
rg -n --pcre2 "(?i)(ProcessTerminal|@deepseek\\-ai/dsh\\-tui|SIGWINCH|node\\-pty|pnpm\\-workspace\\.yaml|Support[- ]the[- ]TUI[- ]on[- ]Windows|feature|boundary)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0456. Remove the stdio and Echo agents](0456-remove-the-stdio-and-echo-agents.md): Shares source implementation: `apps/cli/tests/headless-shutdown.e2e.ts`, `examples/headless-agent/tests/headless.snapshot.ts`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0604. Versioned TUI first-run welcome](0604-versioned-tui-first-run-welcome.md): Shares source implementation: `apps/cli/tests/headless-shutdown.e2e.ts`.
- **`shares-code-with`** — [0646. Wine-run Windows blocking gates on Linux runners](0646-wine-run-windows-blocking-gates-on-linux-runners.md): Shares source implementation: `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0644. Personal staging maintenance skills](0644-personal-staging-maintenance-skills.md): Shares source implementation: `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0503. Required CI gate for web browser expected outputs](0503-required-ci-gate-for-web-browser-expected-outputs.md): Shares source implementation: `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0429. Browser GIFs preserve one evidence chain](0429-browser-gifs-preserve-one-evidence-chain.md): Shares source implementation: `apps/cli/tests/headless-shutdown.e2e.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0569-support-the-tui-on-windows.md`.
