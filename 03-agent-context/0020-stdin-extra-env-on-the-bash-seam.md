---
id: "dsh-note-0020"
title: "stdin + extra env on the bash seam"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-06-30-bash-stdin-env-trusted-plugin-api.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/trust"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/tools"
  - "lifecycle/implemented"
aliases:
  - "shell"
  - "stdin"
  - "ENV_OVERRIDES"
  - "owner"
  - "signal"
  - "ShellExecRequest"
  - "ShellExecSpec"
  - "dshEnv"
  - "env"
  - "scrub"
  - "resolve"
  - "run"
  - "CLAUDE_PROJECT_DIR"
  - "CLAUDE_PLUGIN_ROOT"
search_regex: "(?i)(shell|stdin|ENV_OVERRIDES|owner|signal|ShellExecRequest|ShellExecSpec|dshEnv)"
---

# 0020. stdin + extra env on the bash seam — implementation context

## Open this when

The hooks subsystem runs external hook commands the way Claude Code and Codex do: a hook is a shell command that receives its event payload as JSON on stdin and reads context from a handful of environment variables (CLAUDE_PROJECT_DIR, CLAUDE_PLUGIN_ROOT, PLUGIN_ROOT, …). The harness already has a perfectly good command runner behind the ctx.shell capability seam (dsh-shell → dsh-bash-local), with process-group kills, output truncation/spill, and a credential scrub.

## Source decision

Add stdin?: string and env?: Record to both ShellExecRequest (the model-/plugin-facing request) and ShellExecSpec (the resolved spec run/start act on), and thread them through dsh-bash-local: resolve() carries them verbatim, run()/start() pass them to runBash, which writes the bytes to the child's stdin and merges the extra env. Three deliberate choices: The model-facing tool omits stdin and env. Shell syntax already covers those needs, so duplicate parameters would add surface without authority separation.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-06-30-bash-stdin-env-trusted-plugin-api.md](../02-notes/implemented/architecture/2026-06-30-bash-stdin-env-trusted-plugin-api.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-06-30-bash-stdin-env-trusted-plugin-api.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-06-30-bash-stdin-env-trusted-plugin-api.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/subsystems/shell.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/shell.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/defensive-patterns.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/defensive-patterns.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/shell/shell/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/shell/shell`. Core file in the package named by the note: `packages/shell/shell`. | `named-directory-member, named-package-member` |
| [`packages/shell/shell/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/shell/shell`. Core file in the package named by the note: `packages/shell/shell`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/shell/shell/src/render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/render.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/shell/shell`. Core file in the package named by the note: `packages/shell/shell`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/shell/shell/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/shell/shell`. Core file in the package named by the note: `packages/shell/shell`. | `named-directory-member, named-package-member` |
| [`packages/shell/bash-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/shell/bash-local`. Core file in the package named by the note: `packages/shell/bash-local`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/shell/bash-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/shell/bash-local`. Core file in the package named by the note: `packages/shell/bash-local`. | `named-directory-member, named-package-member` |
| [`packages/shell/shell/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/shell/shell`. Core file in the package named by the note: `packages/shell/shell`. | `named-directory-member, named-package-member` |
| [`packages/shell/shell/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/shell/shell`. Core file in the package named by the note: `packages/shell/shell`. | `named-directory-member, named-package-member` |
| [`packages/shell/bash-local/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/shell/bash-local`. Core file in the package named by the note: `packages/shell/bash-local`. | `named-directory-member, named-package-member` |
| [`packages/shell/bash-local/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/shell/bash-local`. Core file in the package named by the note: `packages/shell/bash-local`. | `named-directory-member, named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `shell` | `const` | [`packages/client/web/src/boot.tsx:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L121) | `const shell = this.ctx.get('appShell')` |
| `stdin` | `const` | [`packages/hooks/hook-protocol/src/runner.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/runner.ts#L75) | `const stdin = JSON.stringify(options.payload) + (options.trailingNewline ? '\n' : '')` |
| `ENV_OVERRIDES` | `const` | [`packages/shell/bash-local/src/index.ts:27`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts#L27) | `export const ENV_OVERRIDES = {` |
| `owner` | `const` | [`packages/shell/shell-env/src/index.ts:131`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell-env/src/index.ts#L131) | `const owner = this.keyOwners.get(key)` |
| `signal` | `const` | [`packages/shell/shell/src/render.ts:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/render.ts#L37) | `const signal = /\n\[killed by signal: ([^\]\n]+)\]$/.exec(text)` |
| `ShellExecRequest` | `interface` | [`packages/shell/shell/src/types.ts:38`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts#L38) | `export interface ShellExecRequest {` |
| `ShellExecSpec` | `interface` | [`packages/shell/shell/src/types.ts:86`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts#L86) | `export interface ShellExecSpec {` |
| `dshEnv` | `const` | [`packages/shell/tool-bash/src/index.ts:341`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts#L341) | `const dshEnv = ctx.shellEnv.collect(exec)` |
| `env` | `const` | [`packages/subprocess/subprocess/src/index.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/index.ts#L61) | `const env: Record<string, string> = {}` |
| `scrub` | `const` | [`packages/test-support/acp-snapshot/src/suite.ts:1223`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts#L1223) | `const scrub = scenario.pinsHeader === true` |
| `resolve` | `function` | [`vendor/cordis/src/registry.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts#L71) | `export function resolve(inject: Inject \| null \| undefined, result: Dict = Object.create(null)) {` |
| `run` | `const` | [`vendor/include/src/index.ts:226`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts#L226) | `const run = this.applyQueue.then(task, task)` |

### Tests and executable evidence

- [`packages/shell/shell/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/tests/service.spec.ts) — A test under the owning area exercises or imports `ShellExecRequest`. A test under the owning area exercises or imports `ShellExecSpec`.
- [`packages/shell/tool-bash/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/tests/tools.spec.ts) — A test under the owning area exercises or imports `dshEnv`.
- [`packages/shell/bash-local/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/tests/settings.spec.ts) — A test under the owning area exercises or imports `dsh-bash-local`.
- [`packages/shell/bash-local/tests/executor.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/tests/executor.spec.ts) — A test under the owning area exercises or imports `dsh-bash-local`. A test under the owning area exercises or imports `dshEnv`.
- [`packages/test-support/acp-snapshot/tests/fixtures/fake-acp-agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/tests/fixtures/fake-acp-agent.ts) — A test under the owning area exercises or imports `scrub`.

## How to read the implementation

1. Start with [`docs/subsystems/shell.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/shell.md) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/discovery-routing`, `concern/lifecycle`, `concern/ownership`, `concern/trust`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/tools`, `lifecycle/implemented`
- Aliases: `shell`, `stdin`, `ENV_OVERRIDES`, `owner`, `signal`, `ShellExecRequest`, `ShellExecSpec`, `dshEnv`, `env`, `scrub`, `resolve`, `run`, `CLAUDE_PROJECT_DIR`, `CLAUDE_PLUGIN_ROOT`
- Regex: `(?i)(shell|stdin|ENV_OVERRIDES|owner|signal|ShellExecRequest|ShellExecSpec|dshEnv)`

```bash
rg -n --pcre2 "(?i)(shell|stdin|ENV_OVERRIDES|owner|signal|ShellExecRequest|ShellExecSpec|dshEnv)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0150. Expose agent session identity and JSONL location to tools and hooks](0150-expose-agent-session-identity-and-jsonl-location-to-tools-and-hooks.md): The source note links to this decision directly.
- **`shares-code-with`** — [0066. The subprocess service is its own seam under the bash executors (`dsh-subprocess` / `dsh-subprocess-local`)](0066-the-subprocess-service-is-its-own-seam-under-the-bash-executors-dsh-subp.md): Shares source implementation: `packages/shell/bash-local/src/index.ts`, `packages/shell/bash-local/src/invariant.ts`.
- **`shares-code-with`** — [0240. PowerShell executor and pwsh tool](0240-powershell-executor-and-pwsh-tool.md): Shares source implementation: `packages/shell/bash-local/src/index.ts`, `packages/shell/shell/src/index.ts`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `docs/subsystems/shell.md`, `packages/shell/shell/src/index.ts`.
- **`shares-code-with`** — [0260. pwsh UI presentation matches bash](0260-pwsh-ui-presentation-matches-bash.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0030. Tool-call timeout policy as a plugin](0030-tool-call-timeout-policy-as-a-plugin.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/render.ts`.
- **`shares-code-with`** — [0287. Plugin configuration in the web settings page](0287-plugin-configuration-in-the-web-settings-page.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0007. Capability seams --- Service Definition / Service Provider / Consumer roles](0007-capability-seams-service-definition-service-provider-consumer-roles.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0020-stdin-extra-env-on-the-bash-seam.md`.
