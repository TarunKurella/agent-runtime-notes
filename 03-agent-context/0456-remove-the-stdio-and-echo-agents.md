---
id: "dsh-note-0456"
title: "Remove the stdio and Echo agents"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-07-20-remove-stdio-and-echo-agents.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
aliases:
  - "dsh"
  - "stdio"
  - "examples/repl-agent"
  - "examples/echo-agent"
  - "@deepseek-ai/dsh-tui"
  - "apps/cli/config/base.cordis.yml"
  - "tui.cordis.yml"
  - "apps/cli/tests/"
  - "dsh --profile headless"
  - "examples/headless-agent"
  - "@deepseek-ai/dsh-acp-demo"
  - "@deepseek-ai/dsh-sdk-jsonrpc-server"
  - "createStdioChat"
  - "StdioRuntime"
search_regex: "(?i)(stdio|examples/repl\\-agent|examples/echo\\-agent|@deepseek\\-ai/dsh\\-tui|apps/cli/config/base\\.cordis\\.yml|tui\\.cordis\\.yml|apps/cli/tests/|dsh[- ]\\-\\-profile[- ]headless)"
---

# 0456. Remove the stdio and Echo agents — implementation context

## Open this when

DeepSeek Harness exposed two redundant product agents beside the TUI and Headless coding agents. The line-oriented stdio agent duplicated terminal interaction and non-interactive execution with a mixed prompt/output protocol. Echo duplicated Headless as a network-free mock model plus one teaching tool, making a test fixture into a user-facing agent and the default quick-start path. Both agents carried support surfaces beyond their leaf configurations. Stdio owned a UI plugin, app package, SDK interface, REPL leaf, prompt protocol, and Loader tests.

## Source decision

The stdio and Echo agents are removed without compatibility packages, modes, commands, or aliases. The stdio UI and app packages, examples/repl-agent, examples/echo-agent, demo:repl, demo:echo, their dedicated tests, and supporting manifests, gates, graphs, and documentation entries are deleted. The remaining application roles are explicit: @deepseek-ai/dsh-tui owns terminal-interactive execution. It rejects non-TTY streams before Loader boot; apps/cli/config/base.cordis.yml plus the tui.cordis.yml overlay own the complete coding composition, with PTY plus terminal-snapshot coverage in apps/cli/tests/.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-07-20-remove-stdio-and-echo-agents.md](../02-notes/implemented/simplification/2026-07-20-remove-stdio-and-echo-agents.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-07-20-remove-stdio-and-echo-agents.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-07-20-remove-stdio-and-echo-agents.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `apps/cli`. | `named-file, named-package-member` |
| [`packages/examples/acp-demo/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/examples/acp-demo`. | `named-file, named-package-member` |
| [`packages/sdk/server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts) | package entry point | Core file in the package named by the note: `packages/sdk/server`. | `named-package-member` |
| [`packages/sdk/server/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/sdk/server`. | `named-package-member` |
| [`packages/bundle/headless/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts) | package entry point | Core file in the package named by the note: `packages/bundle/headless`. | `named-package-member` |
| [`packages/examples/acp-demo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/src/index.ts) | package entry point | Core file in the package named by the note: `packages/examples/acp-demo`. | `named-package-member` |
| [`packages/bundle/headless/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/bundle/headless`. | `named-package-member` |
| [`packages/examples/acp-demo/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/examples/acp-demo`. | `named-package-member` |
| [`examples/headless-agent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/headless-agent/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `examples/headless-agent`. | `named-directory-member` |
| [`examples/headless-agent/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/headless-agent/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `examples/headless-agent`. | `named-directory-member` |
| [`apps/cli/tests`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`examples/headless-agent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/examples/headless-agent) | package or module directory | The source note names this implementation area directly. | `named-directory` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |
| `stdio` | `const` | [`packages/host/directory-picker-native/src/win32-dialog-host.ts:25`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-native/src/win32-dialog-host.ts#L25) | `const stdio: StdioOptions = ['ignore', 'inherit', 'inherit', 'ipc']` |

### Tests and executable evidence

- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — The source note names this file directly. A test under the owning area exercises or imports `headless`.
- [`apps/cli/tests/headless-shutdown.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/headless-shutdown.e2e.ts) — The source note names this file directly. A test under the owning area exercises or imports `headless`.
- [`examples/headless-agent/tests/headless.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/headless-agent/tests/headless.snapshot.ts) — The source note names this file directly.
- [`apps/cli/tests/args.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/args.spec.ts) — A test under the owning area exercises or imports `headless`.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `ask_user_question`.
- [`apps/cli/tests/memory-mcp-configs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/memory-mcp-configs.spec.ts) — A test under the owning area exercises or imports `stdio`. A test under the owning area exercises or imports `DEEPSEEK_API_KEY`.
- [`apps/cli/tests/lazy-search-startup.compat.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/lazy-search-startup.compat.spec.ts) — A test under the owning area exercises or imports `stdio`. A test under the owning area exercises or imports `DEEPSEEK_API_KEY`.
- [`packages/examples/acp-demo/tests/acp-agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/tests/acp-agent.spec.ts) — A test under the owning area exercises or imports `ask_user_question`.
- Source verification intent: TUI and Headless Loader coverage run the real app packages in source and built modes. PTY-driven subprocess coverage is reserved for the TUI lifecycle; other entry-point smokes use the one-shot pipe protocol. Headless proves its task/result and tool-call contracts. Generated graphs and repository searches reject stale package, command, leaf, SDK-interface, createStdioChat, and StdioRuntime references.

## How to read the implementation

1. Start with [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`
- Aliases: `dsh`, `stdio`, `examples/repl-agent`, `examples/echo-agent`, `@deepseek-ai/dsh-tui`, `apps/cli/config/base.cordis.yml`, `tui.cordis.yml`, `apps/cli/tests/`, `dsh --profile headless`, `examples/headless-agent`, `@deepseek-ai/dsh-acp-demo`, `@deepseek-ai/dsh-sdk-jsonrpc-server`, `createStdioChat`, `StdioRuntime`
- Regex: `(?i)(stdio|examples/repl\-agent|examples/echo\-agent|@deepseek\-ai/dsh\-tui|apps/cli/config/base\.cordis\.yml|tui\.cordis\.yml|apps/cli/tests/|dsh[- ]\-\-profile[- ]headless)`

```bash
rg -n --pcre2 "(?i)(stdio|examples/repl\\-agent|examples/echo\\-agent|@deepseek\\-ai/dsh\\-tui|apps/cli/config/base\\.cordis\\.yml|tui\\.cordis\\.yml|apps/cli/tests/|dsh[- ]\\-\\-profile[- ]headless)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0490. Remove the SDK project toolchain](0490-remove-the-sdk-project-toolchain.md): The source note links to this decision directly.
- **`shares-code-with`** — [0604. Versioned TUI first-run welcome](0604-versioned-tui-first-run-welcome.md): Shares source implementation: `apps/cli/README.md`, `apps/cli/tests/built-bin.e2e.ts`.
- **`shares-code-with`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares source implementation: `packages/sdk/server/src/index.ts`, `packages/sdk/server/src/invariant.ts`.
- **`shares-code-with`** — [0293. Minimal profiles use the bare two-tool runtime](0293-minimal-profiles-use-the-bare-two-tool-runtime.md): Shares source implementation: `packages/sdk/server/src/index.ts`, `packages/sdk/server/src/invariant.ts`.
- **`shares-code-with`** — [0684. Adopt execa for hand-rolled test subprocess plumbing](0684-adopt-execa-for-hand-rolled-test-subprocess-plumbing.md): Shares source implementation: `apps/cli/tests/built-bin.e2e.ts`, `packages/examples/acp-demo/src/index.ts`.
- **`shares-code-with`** — [0367. Synchronous cleanup of managed subprocesses on host exit](0367-synchronous-cleanup-of-managed-subprocesses-on-host-exit.md): Shares source implementation: `apps/cli/tests/built-bin.e2e.ts`, `packages/bundle/headless/src/index.ts`.
- **`shares-code-with`** — [0553. Parse `dsh` argv through one Commander adapter](0553-parse-dsh-argv-through-one-commander-adapter.md): Shares source implementation: `apps/cli/tests/built-bin.e2e.ts`, `packages/examples/acp-demo/src/index.ts`.
- **`shares-code-with`** — [0676. Explicit-config dsh entrypoint](0676-explicit-config-dsh-entrypoint.md): Shares source implementation: `apps/cli/README.md`, `apps/cli/tests/built-bin.e2e.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0456-remove-the-stdio-and-echo-agents.md`.
