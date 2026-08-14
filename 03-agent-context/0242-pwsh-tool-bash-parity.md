---
id: "dsh-note-0242"
title: "pwsh tool bash parity"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-02-pwsh-tool-bash-parity.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "dshHome"
  - "HarnessError"
  - "ShellEnvRegistry"
  - "ShellExecutor"
  - "dsh-tool-pwsh"
  - "DSH_*"
  - "[exit code: N]"
  - "ctx.shellEnv"
  - "shellEnv"
  - "dsh-tool-bash"
  - "run_in_background"
  - "job_output"
  - "job_kill"
  - "pwsh-local"
search_regex: "(?i)(dshHome|HarnessError|ShellEnvRegistry|ShellExecutor|dsh\\-tool\\-pwsh|DSH_\\*|\\[exit[- ]code:[- ]N\\]|ctx\\.shellEnv)"
---

# 0242. pwsh tool bash parity — implementation context

## Open this when

The first Windows-native foundation shipped dsh-tool-pwsh as a deliberately minimal profile --- foreground only (a fresh process per call; no persistent PTY session), no managed-environment parity beyond three hardcoded DSH_ keys, and a marker story ("always [exit code: N]") that diverged from the bash tool's rendering without being declared. The model-visible contract drifted from the implementation: the description promised spill-path reporting the renderer never performed, the README claimed exports that did not exist and rendering the tool did not do, and the tool's own tests pinned the lossy behavior.

## Source decision

dsh-tool-pwsh now mirrors dsh-tool-bash call-for-call, and its model-visible text describes exactly that behavior: Rendering adopts the bash story verbatim: stdout, a marked [stderr] section, truncation notices with spill paths, (no output) for an empty body, and exit markers only for non-zero exits --- a clean exit produces no marker. The description and the tool:pwsh prompt section state this precisely ("Non-zero exits are reported as [exit code: N] markers"), deliberately not copying the bash prompt's "every result" phrasing, which its own renderer contradicts.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-02-pwsh-tool-bash-parity.md](../02-notes/implemented/feature/2026-08-02-pwsh-tool-bash-parity.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-02-pwsh-tool-bash-parity.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-02-pwsh-tool-bash-parity.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/shell/shell-env/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell-env/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/shell-env`. Defines `ShellEnvRegistry`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-bash`. | `named-package-member` |
| [`packages/shell/tool-pwsh/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-pwsh`. | `named-package-member` |
| [`packages/shell/bash-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/bash-local`. | `named-package-member` |
| [`packages/shell/pwsh-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/pwsh-local`. | `named-package-member` |
| [`packages/shell/shell-env/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell-env/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/shell-env`. | `named-package-member` |
| [`packages/shell/tool-bash/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/tool-bash`. | `named-package-member` |
| [`packages/shell/tool-pwsh/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/tool-pwsh`. | `named-package-member` |
| [`packages/shell/bash-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/bash-local`. | `named-package-member` |
| [`packages/shell/pwsh-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/pwsh-local`. | `named-package-member` |
| [`packages/shell/shell-env`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell-env) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/shell/tool-bash`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `dshHome` | `const` | [`packages/examples/agent-spine-demo/src/index.ts:218`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/src/index.ts#L218) | `const dshHome = resolveDshHome(config.dshHome ?? nestedDshHome)` |
| `HarnessError` | `class` | [`packages/llm/llm/src/error.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts#L13) | `export class HarnessError extends Error {` |
| `ShellEnvRegistry` | `class` | [`packages/shell/shell-env/src/index.ts:89`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell-env/src/index.ts#L89) | `export class ShellEnvRegistry extends Service {` |
| `ShellExecutor` | `class` | [`packages/shell/shell/src/index.ts:65`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/index.ts#L65) | `export abstract class ShellExecutor extends Service {` |

### Tests and executable evidence

- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `HarnessError`.
- [`packages/shell/shell/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/tests/service.spec.ts) — A test under the owning area exercises or imports `ShellExecutor`.
- [`packages/shell/tool-pwsh/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/tests/tools.spec.ts) — A test under the owning area exercises or imports `dsh-tool-pwsh`. A test under the owning area exercises or imports `run_in_background`.
- [`packages/shell/tool-bash/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/tests/tools.spec.ts) — A test under the owning area exercises or imports `run_in_background`. A test under the owning area exercises or imports `job_output`.
- [`packages/shell/tool-pwsh/tests/loader.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/tests/loader.spec.ts) — A test under the owning area exercises or imports `dsh-pwsh-local`.
- [`packages/shell/shell-env/tests/shell-env.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell-env/tests/shell-env.spec.ts) — A test under the owning area exercises or imports `shellEnv`. A test under the owning area exercises or imports `ShellEnvRegistry`.
- [`packages/shell/pwsh-local/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/tests/settings.spec.ts) — A test under the owning area exercises or imports `pwsh-local`. A test under the owning area exercises or imports `dsh-pwsh-local`.
- [`packages/shell/pwsh-local/tests/executor.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/tests/executor.spec.ts) — A test under the owning area exercises or imports `powershell`. A test under the owning area exercises or imports `dsh-pwsh-local`.

## How to read the implementation

1. Start with [`packages/shell/shell-env/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell-env/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `dshHome`, `HarnessError`, `ShellEnvRegistry`, `ShellExecutor`, `dsh-tool-pwsh`, `DSH_*`, `[exit code: N]`, `ctx.shellEnv`, `shellEnv`, `dsh-tool-bash`, `run_in_background`, `job_output`, `job_kill`, `pwsh-local`
- Regex: `(?i)(dshHome|HarnessError|ShellEnvRegistry|ShellExecutor|dsh\-tool\-pwsh|DSH_\*|\[exit[- ]code:[- ]N\]|ctx\.shellEnv)`

```bash
rg -n --pcre2 "(?i)(dshHome|HarnessError|ShellEnvRegistry|ShellExecutor|dsh\\-tool\\-pwsh|DSH_\\*|\\[exit[- ]code:[- ]N\\]|ctx\\.shellEnv)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0260. pwsh UI presentation matches bash](0260-pwsh-ui-presentation-matches-bash.md): The source note links to this decision directly.
- **`source-link`** — [0279. Windows sandbox rung: raw ACL restricted tokens over mxc and AppContainer](0279-windows-sandbox-rung-raw-acl-restricted-tokens-over-mxc-and-appcontainer.md): The source note links to this decision directly.
- **`shares-code-with`** — [0240. PowerShell executor and pwsh tool](0240-powershell-executor-and-pwsh-tool.md): Shares source implementation: `packages/shell/bash-local/src/index.ts`, `packages/shell/pwsh-local/src/index.ts`.
- **`shares-code-with`** — [0124. Loader interpolates the entry `disabled` field](0124-loader-interpolates-the-entry-disabled-field.md): Shares source implementation: `packages/shell/tool-bash/src/index.ts`, `packages/shell/tool-bash/src/invariant.ts`.
- **`shares-code-with`** — [0130. Multiplex concurrent ACP sessions over one connection](0130-multiplex-concurrent-acp-sessions-over-one-connection.md): Shares source implementation: `packages/shell/tool-bash/src/index.ts`, `packages/shell/tool-pwsh/src/index.ts`.
- **`shares-code-with`** — [0066. The subprocess service is its own seam under the bash executors (`dsh-subprocess` / `dsh-subprocess-local`)](0066-the-subprocess-service-is-its-own-seam-under-the-bash-executors-dsh-subp.md): Shares source implementation: `packages/shell/bash-local/src/index.ts`, `packages/shell/bash-local/src/invariant.ts`.
- **`shares-code-with`** — [0241. Windows defaults to pwsh](0241-windows-defaults-to-pwsh.md): Shares source implementation: `packages/shell/tool-bash/src/index.ts`, `packages/shell/tool-pwsh/src/index.ts`.
- **`shares-code-with`** — [0014. Branded IDs everywhere they belong](0014-branded-ids-everywhere-they-belong.md): Shares source implementation: `packages/shell/bash-local/src/index.ts`, `packages/shell/tool-bash/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0242-pwsh-tool-bash-parity.md`.
