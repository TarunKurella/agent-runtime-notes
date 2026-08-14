---
id: "dsh-note-0240"
title: "PowerShell executor and pwsh tool"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-01-pwsh-tool-and-executor.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/security"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/registry"
aliases:
  - "shell"
  - "approval"
  - "resolvePwshPath"
  - "signal"
  - "resolve"
  - "run"
  - "windowsUnsupportedPackages"
  - "dsh-bash-local"
  - "sandbox_permissions"
  - "packages/shell/"
  - "@deepseek-ai/dsh-pwsh-local"
  - "ctx.shell"
  - "ctx.subprocess"
  - "pwsh -NoLogo -NoProfile -NonInteractive -Command"
search_regex: "(?i)(shell|approval|resolvePwshPath|signal|resolve|windowsUnsupportedPackages|dsh\\-bash\\-local|sandbox_permissions)"
---

# 0240. PowerShell executor and pwsh tool — implementation context

## Open this when

The harness spoke one shell dialect on every platform: bash. Windows hosts could run it only through WSL or Git-Bash shims, and the shipped dsh-bash-local executor is POSIX-only (bash hardcoded, process-group semantics POSIX). The Windows roadmap --- defaulting hosts to pwsh, later pwsh TUI/GUI rendering --- had no execution foundation: there was no PowerShell implementation of the bash executor seam and no model-facing tool that taught the PowerShell dialect.

## Source decision

Two new packages under packages/shell/: @deepseek-ai/dsh-pwsh-local --- a local implementation of the ctx.shell executor seam over ctx.subprocess, mirroring dsh-bash-local call-for-call: resolve() defaults and caps from config, run() fuses the config-clamped timeout with the caller's signal through one deadline, start() returns a consuming background handle whose processes belong to the subprocess service. The command string rides as ONE argv element to pwsh -NoLogo -NoProfile -NonInteractive -Command, so PowerShell parses it and no shell-quoting layer exists.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-01-pwsh-tool-and-executor.md](../02-notes/implemented/feature/2026-08-01-pwsh-tool-and-executor.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-01-pwsh-tool-and-executor.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-01-pwsh-tool-and-executor.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/shell/shell/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/shell/shell/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/shell/shell/src/render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/render.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/shell`. Core file in the package named by the note: `packages/shell/shell`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/shell/shell/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/shell/shell-env/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell-env/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/shell-env`. | `named-package-member` |
| [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-bash`. | `named-package-member` |
| [`packages/shell/tool-pwsh/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-pwsh`. | `named-package-member` |
| [`packages/shell/bash-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/bash-local`. | `named-package-member` |
| [`packages/shell/pwsh-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/pwsh-local`. | `named-package-member` |
| [`packages/shell/pwsh-local/src/resolve.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/resolve.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/shell`. Core file in the package named by the note: `packages/shell/pwsh-local`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/shell/shell-env/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell-env/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/shell-env`. | `named-package-member` |
| [`packages/shell/tool-bash/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/tool-bash`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `shell` | `const` | [`packages/client/web/src/boot.tsx:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L121) | `const shell = this.ctx.get('appShell')` |
| `approval` | `const` | [`packages/core/tools/src/index.ts:1693`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1693) | `const approval = this.ctx.get('approval')` |
| `resolvePwshPath` | `function` | [`packages/shell/pwsh-local/src/resolve.ts:67`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/resolve.ts#L67) | `export function resolvePwshPath(` |
| `signal` | `const` | [`packages/shell/shell/src/render.ts:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/render.ts#L37) | `const signal = /\n\[killed by signal: ([^\]\n]+)\]$/.exec(text)` |
| `resolve` | `function` | [`vendor/cordis/src/registry.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts#L71) | `export function resolve(inject: Inject \| null \| undefined, result: Dict = Object.create(null)) {` |
| `run` | `const` | [`vendor/include/src/index.ts:226`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts#L226) | `const run = this.applyQueue.then(task, task)` |
| `windowsUnsupportedPackages` | `const` | [`vitest.config.ts:21`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vitest.config.ts#L21) | `const windowsUnsupportedPackages = process.platform === 'win32'` |

### Tests and executable evidence

- [`packages/shell/tool-bash/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/tests/tools.spec.ts) — A test under the owning area exercises or imports `sandbox_permissions`. A test under the owning area exercises or imports `run_in_background`.
- [`packages/shell/tool-pwsh/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/tests/tools.spec.ts) — A test under the owning area exercises or imports `sandbox_permissions`. A test under the owning area exercises or imports `run_in_background`.
- [`packages/shell/tool-pwsh/tests/loader.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/tests/loader.spec.ts) — A test under the owning area exercises or imports `resolvePwshPath`. A test under the owning area exercises or imports `tool-pwsh`.
- [`packages/shell/bash-local/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/tests/settings.spec.ts) — A test under the owning area exercises or imports `dsh-bash-local`.
- [`packages/shell/bash-local/tests/executor.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/tests/executor.spec.ts) — A test under the owning area exercises or imports `dsh-bash-local`.
- [`packages/shell/pwsh-local/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/tests/settings.spec.ts) — A test under the owning area exercises or imports `pwsh`. A test under the owning area exercises or imports `pwsh-local`.
- [`packages/shell/pwsh-local/tests/executor.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/tests/executor.spec.ts) — A test under the owning area exercises or imports `pwsh`. A test under the owning area exercises or imports `resolvePwshPath`.
- [`packages/shell/shell-env/tests/shell-env.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell-env/tests/shell-env.spec.ts) — A test under the owning area exercises or imports `dsh-shell-env`.

## How to read the implementation

1. Start with [`packages/shell/shell/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/security`, `domain/shell-terminal`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/registry`
- Aliases: `shell`, `approval`, `resolvePwshPath`, `signal`, `resolve`, `run`, `windowsUnsupportedPackages`, `dsh-bash-local`, `sandbox_permissions`, `packages/shell/`, `@deepseek-ai/dsh-pwsh-local`, `ctx.shell`, `ctx.subprocess`, `pwsh -NoLogo -NoProfile -NonInteractive -Command`
- Regex: `(?i)(shell|approval|resolvePwshPath|signal|resolve|windowsUnsupportedPackages|dsh\-bash\-local|sandbox_permissions)`

```bash
rg -n --pcre2 "(?i)(shell|approval|resolvePwshPath|signal|resolve|windowsUnsupportedPackages|dsh\\-bash\\-local|sandbox_permissions)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0241. Windows defaults to pwsh](0241-windows-defaults-to-pwsh.md): The source note links to this decision directly.
- **`source-link`** — [0242. pwsh tool bash parity](0242-pwsh-tool-bash-parity.md): The source note links to this decision directly.
- **`source-link`** — [0279. Windows sandbox rung: raw ACL restricted tokens over mxc and AppContainer](0279-windows-sandbox-rung-raw-acl-restricted-tokens-over-mxc-and-appcontainer.md): The source note links to this decision directly.
- **`shares-code-with`** — [0260. pwsh UI presentation matches bash](0260-pwsh-ui-presentation-matches-bash.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0020. stdin + extra env on the bash seam](0020-stdin-extra-env-on-the-bash-seam.md): Shares source implementation: `packages/shell/bash-local/src/index.ts`, `packages/shell/shell/src/index.ts`.
- **`shares-code-with`** — [0066. The subprocess service is its own seam under the bash executors (`dsh-subprocess` / `dsh-subprocess-local`)](0066-the-subprocess-service-is-its-own-seam-under-the-bash-executors-dsh-subp.md): Shares source implementation: `packages/shell/bash-local/src/index.ts`, `packages/shell/shell/src/index.ts`.
- **`shares-code-with`** — [0014. Branded IDs everywhere they belong](0014-branded-ids-everywhere-they-belong.md): Shares source implementation: `packages/shell/bash-local/src/index.ts`, `packages/shell/shell/src/index.ts`.
- **`shares-code-with`** — [0011. Agent lifecycle and ownership contracts](0011-agent-lifecycle-and-ownership-contracts.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0240-powershell-executor-and-pwsh-tool.md`.
