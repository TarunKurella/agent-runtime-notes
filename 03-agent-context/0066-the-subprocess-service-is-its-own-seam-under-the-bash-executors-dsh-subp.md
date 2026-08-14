---
id: "dsh-note-0066"
title: "The subprocess service is its own seam under the bash executors (`dsh-subprocess` / `dsh-subprocess-local`)"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-26-subprocess-seam.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "argv"
  - "jobs"
  - "ENV_OVERRIDES"
  - "timedOut"
  - "aborted"
  - "ShellExecRequest"
  - "ShellExecSpec"
  - "ShellProcess"
  - "LocalSubprocessRuntime"
  - "terminate"
  - "done"
  - "waitForExit"
  - "SubprocessRuntime"
  - "DSH_ENV_PREFIX"
search_regex: "(?i)(argv|jobs|ENV_OVERRIDES|timedOut|aborted|ShellExecRequest|ShellExecSpec|ShellProcess)"
---

# 0066. The subprocess service is its own seam under the bash executors (`dsh-subprocess` / `dsh-subprocess-local`) — implementation context

## Open this when

dsh-bash-local bundled two capabilities that change for different reasons: running a bash command (command defaulting, timeout classification, model-friendly terminal environment, the stdout/stderr merge the bash tool renders) and running and managing a child process (detached process groups, bounded tail-keep output with spill files, the credential scrub and DSH_ merge order, SIGTERM→grace→SIGKILL escalation, kill-and-join disposal).

## Source decision

A new subprocess/ capability family owns "run and manage a process"; the bash family keeps "run a bash command" and consumes it: @deepseek-ai/dsh-subprocess (Service Definition) --- the abstract SubprocessRuntime owning ctx.subprocess: executable lookup, fully explicit ordinary spawns, and the terminal primitive added by the portable execution-world decision. Each stdio stream independently selects 'pipe', 'inherit', or bounded collection { maxBytes, spill? }; stdin selects 'ignore', 'pipe', or { data }.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-26-subprocess-seam.md](../02-notes/implemented/architecture/2026-07-26-subprocess-seam.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-26-subprocess-seam.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-26-subprocess-seam.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/jobs/jobs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/jobs/jobs`. | `named-package-member` |
| [`packages/jobs/jobs/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/jobs/jobs`. | `named-package-member` |
| [`packages/shell/shell/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/shell/shell/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/shell/shell`. Defines `ShellExecSpec`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/jobs/jobs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/jobs/jobs`. | `named-package-member` |
| [`packages/shell/shell/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/shell/bash-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/bash-local`. Defines `timedOut`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/bash-sandbox/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/bash-sandbox`. | `named-package-member` |
| [`packages/shell/bash-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/bash-local`. | `named-package-member` |
| [`packages/subprocess/subprocess/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subprocess/subprocess`. Defines `SubprocessRuntime`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subprocess/subprocess/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/subprocess/subprocess`. Defines `CollectedOutput`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/bash-sandbox/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/bash-sandbox`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `argv` | `const` | [`packages/e2b/subprocess-e2b/src/process.ts:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/process.ts#L121) | `const argv = spec.argv.map(quoteE2BShellArg).join(' ')` |
| `jobs` | `const` | [`packages/jobs/tool-jobs/src/index.ts:356`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L356) | `const jobs = ctx.jobs.list(exec.agent)` |
| `ENV_OVERRIDES` | `const` | [`packages/shell/bash-local/src/index.ts:27`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts#L27) | `export const ENV_OVERRIDES = {` |
| `timedOut` | `const` | [`packages/shell/bash-local/src/index.ts:230`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts#L230) | `const timedOut = timeoutOf(d.signal, 'BASH_TIMEOUT') !== undefined` |
| `aborted` | `const` | [`packages/shell/bash-local/src/index.ts:231`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts#L231) | `const aborted = d.signal.aborted && !timedOut` |
| `ShellExecRequest` | `interface` | [`packages/shell/shell/src/types.ts:38`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts#L38) | `export interface ShellExecRequest {` |
| `ShellExecSpec` | `interface` | [`packages/shell/shell/src/types.ts:86`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts#L86) | `export interface ShellExecSpec {` |
| `ShellProcess` | `interface` | [`packages/shell/shell/src/types.ts:161`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts#L161) | `export interface ShellProcess {` |
| `LocalSubprocessRuntime` | `class` | [`packages/subprocess/subprocess-local/src/index.ts:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/index.ts#L37) | `export class LocalSubprocessRuntime extends SubprocessRuntime {` |
| `terminate` | `const` | [`packages/subprocess/subprocess-local/src/spawn.ts:439`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts#L439) | `const terminate = (): void => {` |
| `done` | `const` | [`packages/subprocess/subprocess-local/src/spawn.ts:470`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts#L470) | `const done = new Promise<SubprocessOutcome>((resolve, reject) => {` |
| `waitForExit` | `const` | [`packages/subprocess/subprocess-local/src/spawn.ts:507`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts#L507) | `const waitForExit = async (signal?: AbortSignal): Promise<boolean> => {` |
| `aborted` | `const` | [`packages/subprocess/subprocess-local/src/spawn.ts:515`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts#L515) | `const aborted = Promise.withResolvers<boolean>()` |
| `SubprocessRuntime` | `class` | [`packages/subprocess/subprocess/src/index.ts:102`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/index.ts#L102) | `export abstract class SubprocessRuntime extends Service {` |
| `DSH_ENV_PREFIX` | `const` | [`packages/subprocess/subprocess/src/types.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/types.ts#L13) | `export const DSH_ENV_PREFIX = 'DSH_' as const` |
| `DshEnvironment` | `type` | [`packages/subprocess/subprocess/src/types.ts:19`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/types.ts#L19) | `export type DshEnvironment = Readonly<Record<DshEnvironmentKey, string>>` |

### Tests and executable evidence

- [`packages/shell/shell/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/tests/service.spec.ts) — A test under the owning area exercises or imports `ShellExecSpec`. A test under the owning area exercises or imports `timedOut`.
- [`packages/shell/bash-sandbox/tests/bwrap.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/tests/bwrap.e2e.ts) — A test under the owning area exercises or imports `LocalSubprocessRuntime`. A test under the owning area exercises or imports `dsh-bash-sandbox`.
- [`packages/shell/bash-local/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/tests/settings.spec.ts) — A test under the owning area exercises or imports `dsh-bash-local`. A test under the owning area exercises or imports `LocalSubprocessRuntime`.
- [`packages/shell/bash-local/tests/executor.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/tests/executor.spec.ts) — A test under the owning area exercises or imports `dsh-bash-local`. A test under the owning area exercises or imports `LocalSubprocessRuntime`.
- [`packages/shell/bash-sandbox/tests/sandbox.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/tests/sandbox.spec.ts) — A test under the owning area exercises or imports `CollectedOutput`. A test under the owning area exercises or imports `SubprocessRuntime`.
- [`packages/shell/bash-sandbox/tests/landlock.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/tests/landlock.e2e.ts) — A test under the owning area exercises or imports `LocalSubprocessRuntime`. A test under the owning area exercises or imports `dsh-bash-sandbox`.
- [`packages/shell/bash-sandbox/tests/seatbelt.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/tests/seatbelt.e2e.ts) — A test under the owning area exercises or imports `LocalSubprocessRuntime`. A test under the owning area exercises or imports `dsh-bash-sandbox`.
- [`packages/subprocess/subprocess/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/tests/service.spec.ts) — A test under the owning area exercises or imports `dsh-subprocess`. A test under the owning area exercises or imports `SubprocessRuntime`.

## How to read the implementation

1. Start with [`packages/jobs/jobs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `argv`, `jobs`, `ENV_OVERRIDES`, `timedOut`, `aborted`, `ShellExecRequest`, `ShellExecSpec`, `ShellProcess`, `LocalSubprocessRuntime`, `terminate`, `done`, `waitForExit`, `SubprocessRuntime`, `DSH_ENV_PREFIX`
- Regex: `(?i)(argv|jobs|ENV_OVERRIDES|timedOut|aborted|ShellExecRequest|ShellExecSpec|ShellProcess)`

```bash
rg -n --pcre2 "(?i)(argv|jobs|ENV_OVERRIDES|timedOut|aborted|ShellExecRequest|ShellExecSpec|ShellProcess)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0064. The job registry is a capability seam (`dsh-jobs` / `dsh-jobs-local`)](0064-the-job-registry-is-a-capability-seam-dsh-jobs-dsh-jobs-local.md): The source note links to this decision directly.
- **`source-link`** — [0072. Portable consumers over filesystem and subprocess execution worlds](0072-portable-consumers-over-filesystem-and-subprocess-execution-worlds.md): The source note links to this decision directly.
- **`shares-code-with`** — [0020. stdin + extra env on the bash seam](0020-stdin-extra-env-on-the-bash-seam.md): Shares source implementation: `packages/shell/bash-local/src/index.ts`, `packages/shell/bash-local/src/invariant.ts`.
- **`shares-code-with`** — [0158. persistent PTY sessions](0158-persistent-pty-sessions.md): Shares source implementation: `packages/jobs/jobs/src/index.ts`, `packages/jobs/jobs/src/types.ts`.
- **`shares-code-with`** — [0240. PowerShell executor and pwsh tool](0240-powershell-executor-and-pwsh-tool.md): Shares source implementation: `packages/shell/bash-local/src/index.ts`, `packages/shell/shell/src/index.ts`.
- **`shares-code-with`** — [0015. The background job runtime (`ctx.jobs`) and generic task control tools](0015-the-background-job-runtime-ctx-jobs-and-generic-task-control-tools.md): Shares source implementation: `packages/jobs/jobs/src/index.ts`, `packages/jobs/jobs/src/invariant.ts`.
- **`shares-code-with`** — [0134. Subagent capability seam](0134-subagent-capability-seam.md): Shares source implementation: `packages/jobs/jobs/src/index.ts`, `packages/jobs/jobs/src/invariant.ts`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0066-the-subprocess-service-is-its-own-seam-under-the-bash-executors-dsh-subp.md`.
