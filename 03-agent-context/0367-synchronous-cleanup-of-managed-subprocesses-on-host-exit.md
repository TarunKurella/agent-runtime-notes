---
id: "dsh-note-0367"
title: "Synchronous cleanup of managed subprocesses on host exit"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-11-synchronous-subprocess-exit-cleanup.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/trust"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/shell-terminal"
  - "domain/testing"
  - "lifecycle/implemented"
aliases:
  - "exit"
  - "LocalSubprocessRuntime"
  - "terminate"
  - "SubprocessHandle"
  - "SubprocessTerminalHandle"
  - "process.exit"
  - "taskkill /PID <pid> /T /F"
  - "SIGTERM"
  - "SIGINT"
  - "SIGHUP"
  - "SIGKILL"
  - "process.abort"
  - "forceKill"
  - "Synchronous cleanup of managed subprocesses on host exit"
search_regex: "(?i)(exit|LocalSubprocessRuntime|terminate|SubprocessHandle|SubprocessTerminalHandle|process\\.exit|taskkill[- ]/PID[- ]<pid>[- ]/T[- ]/F|SIGTERM)"
---

# 0367. Synchronous cleanup of managed subprocesses on host exit — implementation context

## Open this when

The local subprocess provider owns ordinary detached process trees and terminal sessions, but it previously reached them only through asynchronous Cordis disposal. A fatal launcher may call process.exit() before that disposal finishes: the fail-loud release waits at most two seconds, while a local process can have a longer termination grace. Once Node enters its synchronous exit phase, pending promises and escalation timers do not continue, so a TERM-resistant child can outlive the host and keep CPU, memory, or ports. Some ACP, JSON-RPC, and SDK entry points also have no root release callback.

## Source decision

LocalSubprocessRuntime installs one synchronous Node exit listener in its Cordis effect. The same effect removes the listener only after normal disposal settles. Ordinary and terminal handles remain in the service's existing live sets while asynchronous cleanup is pending, so a shorter outer exit bound still sees and force-terminates them. If awaited disposal reports a cleanup failure, the service invokes the same synchronous final operations before clearing the sets and removing the listener.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-11-synchronous-subprocess-exit-cleanup.md](../02-notes/implemented/bug-fix/2026-08-11-synchronous-subprocess-exit-cleanup.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-11-synchronous-subprocess-exit-cleanup.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-11-synchronous-subprocess-exit-cleanup.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/sdk/server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts) | package entry point | Defines `exit`, a construct named by the note. | `symbol-definition` |
| [`packages/boot/cmdline/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline/src/index.ts) | package entry point | Defines `exit`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/shell/src/render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/render.ts) | runtime implementation | Defines `exit`, a construct named by the note. | `symbol-definition` |
| [`packages/bundle/headless/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts) | package entry point | Defines `exit`, a construct named by the note. | `symbol-definition` |
| [`packages/subprocess/subprocess/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/types.ts) | public types and contract | Defines `SubprocessHandle`, a construct named by the note. Defines `SubprocessTerminalHandle`, a construct named by the note. | `symbol-definition` |
| [`packages/subprocess/subprocess-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/index.ts) | package entry point | Defines `LocalSubprocessRuntime`, a construct named by the note. | `symbol-definition` |
| [`packages/subprocess/subprocess-local/src/spawn.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts) | runtime implementation | Defines `terminate`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `exit` | `const` | [`packages/boot/cmdline/src/index.ts:102`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline/src/index.ts#L102) | `const exit = ctx.get('appExit')` |
| `exit` | `const` | [`packages/bundle/headless/src/index.ts:144`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts#L144) | `const exit = ctx.get('appExit')` |
| `exit` | `const` | [`packages/sdk/server/src/index.ts:57`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts#L57) | `const exit = config.exit ?? ((code: number): void => { process.exit(code) })` |
| `exit` | `const` | [`packages/shell/shell/src/render.ts:39`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/render.ts#L39) | `const exit = /\n\[exit code: (\d+)\]$/.exec(text)` |
| `LocalSubprocessRuntime` | `class` | [`packages/subprocess/subprocess-local/src/index.ts:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/index.ts#L37) | `export class LocalSubprocessRuntime extends SubprocessRuntime {` |
| `terminate` | `const` | [`packages/subprocess/subprocess-local/src/spawn.ts:439`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts#L439) | `const terminate = (): void => {` |
| `SubprocessHandle` | `interface` | [`packages/subprocess/subprocess/src/types.ts:167`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/types.ts#L167) | `export interface SubprocessHandle {` |
| `SubprocessTerminalHandle` | `interface` | [`packages/subprocess/subprocess/src/types.ts:235`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/types.ts#L235) | `export interface SubprocessTerminalHandle {` |

### Tests and executable evidence

- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `SIGTERM`.
- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — A test under the owning area exercises or imports `SIGTERM`. A test under the owning area exercises or imports `SIGKILL`.
- [`python/sdk/tests/test_client.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_client.py) — A test under the owning area exercises or imports `SIGTERM`.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `SIGTERM`. A test under the owning area exercises or imports `SIGKILL`.
- [`apps/cli/tests/headless-shutdown.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/headless-shutdown.e2e.ts) — A test under the owning area exercises or imports `SIGKILL`.
- [`apps/cli/tests/source-launch.compat.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/source-launch.compat.spec.ts) — A test under the owning area exercises or imports `SIGKILL`.
- [`packages/lsp/lsp-stdio/tests/instance.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/instance.spec.ts) — A test under the owning area exercises or imports `SIGTERM`. A test under the owning area exercises or imports `SIGKILL`.
- [`packages/lsp/lsp-stdio/tests/built-lib.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/built-lib.e2e.ts) — A test under the owning area exercises or imports `SIGKILL`.
- Source verification intent: A parent test starts an isolated TypeScript host through the repository source launcher, waits until exact root and descendant process identities are observable, then allows the host to take each fatal path. Direct exit, default uncaught exception, and default unhandled rejection cover ordinary TERM-resistant trees; direct exit also covers a real terminal root and descendant. The parent asserts the original host exit category and waits for every recorded process to disappear, while failure cleanup targets only recorded identities or the recorded Windows tree.

## How to read the implementation

1. Start with [`packages/sdk/server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/trust`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/shell-terminal`, `domain/testing`, `lifecycle/implemented`
- Aliases: `exit`, `LocalSubprocessRuntime`, `terminate`, `SubprocessHandle`, `SubprocessTerminalHandle`, `process.exit`, `taskkill /PID <pid> /T /F`, `SIGTERM`, `SIGINT`, `SIGHUP`, `SIGKILL`, `process.abort`, `forceKill`, `Synchronous cleanup of managed subprocesses on host exit`
- Regex: `(?i)(exit|LocalSubprocessRuntime|terminate|SubprocessHandle|SubprocessTerminalHandle|process\.exit|taskkill[- ]/PID[- ]<pid>[- ]/T[- ]/F|SIGTERM)`

```bash
rg -n --pcre2 "(?i)(exit|LocalSubprocessRuntime|terminate|SubprocessHandle|SubprocessTerminalHandle|process\\.exit|taskkill[- ]/PID[- ]<pid>[- ]/T[- ]/F|SIGTERM)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0066. The subprocess service is its own seam under the bash executors (`dsh-subprocess` / `dsh-subprocess-local`)](0066-the-subprocess-service-is-its-own-seam-under-the-bash-executors-dsh-subp.md): The source note links to this decision directly.
- **`source-link`** — [0329. fail-loud releases the terminal before exiting](0329-fail-loud-releases-the-terminal-before-exiting.md): The source note links to this decision directly.
- **`shares-code-with`** — [0456. Remove the stdio and Echo agents](0456-remove-the-stdio-and-echo-agents.md): Shares source implementation: `apps/cli/tests/built-bin.e2e.ts`, `packages/bundle/headless/src/index.ts`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `apps/cli/tests/built-bin.e2e.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0534. Drop bash full-output spill files](0534-drop-bash-full-output-spill-files.md): Shares source implementation: `packages/subprocess/subprocess-local/src/spawn.ts`, `packages/subprocess/subprocess/src/types.ts`.
- **`shares-code-with`** — [0249. Claude Code and Codex subagent backends](0249-claude-code-and-codex-subagent-backends.md): Shares source implementation: `packages/subprocess/subprocess/src/types.ts`.
- **`shares-code-with`** — [0105. Apps own their command line through `ctx.cmdlineArgs`](0105-apps-own-their-command-line-through-ctx-cmdlineargs.md): Shares source implementation: `packages/boot/cmdline/src/index.ts`, `packages/bundle/headless/src/index.ts`.
- **`shares-code-with`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): Shares source implementation: `packages/sdk/server/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0367-synchronous-cleanup-of-managed-subprocesses-on-host-exit.md`.
