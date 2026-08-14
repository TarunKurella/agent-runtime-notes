---
id: "dsh-note-0241"
title: "Windows defaults to pwsh"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-01-windows-pwsh-default.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "healProfilesModuleFallback"
  - "dsh"
  - "disabled"
  - "shell"
  - "permission"
  - "dsh-bash-local"
  - "bash -c"
  - "ctx.shell"
  - "dsh --profile headless"
  - "bash-sandbox"
  - "tool-bash"
  - "disabled: !!js process.platform === 'win32"
  - "pwsh-sandbox"
  - "tool-pwsh"
search_regex: "(?i)(healProfilesModuleFallback|disabled|shell|permission|dsh\\-bash\\-local|bash[- ]\\-c|ctx\\.shell|dsh[- ]\\-\\-profile[- ]headless)"
---

# 0241. Windows defaults to pwsh — implementation context

## Open this when

The harness's shipped execution profile is bash-first on every platform. Windows hosts must install a bash shim (WSL or Git-Bash) or fall back to the POSIX-only dsh-bash-local behavior (hardcoded bash -c argv, process-group semantics); the model-facing bash tool teaches the bash dialect. The Windows-native foundation shipped in the pwsh executor and tool decision --- a PowerShell implementation of the ctx.shell seam and a parity pwsh tool --- but shipped compositions still mounted the bash stack on Windows, so a Windows host without a shim could not run the shipped shell.

## Source decision

Windows hosts booting a shipped profile (dsh web, dsh --profile headless, one-shot tasks) get the PowerShell stack by default; POSIX hosts are unchanged. The base patch gates both shell stacks on its own rows (the loader disabled interpolation note records the mechanism and the platform-layer fold): bash-sandbox/tool-bash carry disabled: !!js process.platform === 'win32' (bash has no Windows runner), and their twins pwsh-sandbox/tool-pwsh mount only on win32 with the inverted expression --- one shared patch file, exactly one shell stack per host.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-01-windows-pwsh-default.md](../02-notes/implemented/feature/2026-08-01-windows-pwsh-default.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-01-windows-pwsh-default.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-01-windows-pwsh-default.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/bundle/base/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/index.ts) | package entry point | Core file in the package named by the note: `packages/bundle/base`. | `named-package-member` |
| [`packages/shell/shell/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/shell/shell/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/bundle/web-app/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/src/index.ts) | package entry point | Core file in the package named by the note: `packages/bundle/web-app`. | `named-package-member` |
| [`packages/bundle/base/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/bundle/base`. | `named-package-member` |
| [`packages/shell/shell/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-bash`. | `named-package-member` |
| [`packages/shell/tool-pwsh/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-pwsh`. | `named-package-member` |
| [`packages/shell/bash-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/bash-local`. | `named-package-member` |
| [`packages/shell/pwsh-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/pwsh-local`. | `named-package-member` |
| [`packages/bundle/web-app/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/bundle/web-app`. | `named-package-member` |
| [`packages/shell/bash-sandbox/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/bash-sandbox`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `healProfilesModuleFallback` | `function` | [`packages/boot/app-boot/src/profile.ts:223`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/profile.ts#L223) | `export function healProfilesModuleFallback(installAnchor: string, home: string = resolveDshHome()): void {` |
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |
| `disabled` | `const` | [`packages/client/ui-settings-plugins/src/client/BashCard.tsx:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/client/BashCard.tsx#L23) | `const disabled = !state.writable` |
| `shell` | `const` | [`packages/client/web/src/boot.tsx:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L121) | `const shell = this.ctx.get('appShell')` |
| `permission` | `const` | [`packages/hooks/hook-protocol/src/codec.ts:125`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/codec.ts#L125) | `const permission = permissionDecisionOf(str(hso, 'permissionDecision'))` |

### Tests and executable evidence

- [`apps/cli/tests/windows-shell.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/windows-shell.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `pwsh`.
- [`packages/bundle/base/tests/base.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/tests/base.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `pwsh-sandbox`.
- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — A test under the owning area exercises or imports `dsh-base`.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `healProfilesModuleFallback`. A test under the owning area exercises or imports `dsh-base`.
- [`apps/cli/tests/headless-shutdown.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/headless-shutdown.e2e.ts) — A test under the owning area exercises or imports `dsh-base`.
- [`packages/shell/shell/tests/render.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/tests/render.spec.ts) — A test under the owning area exercises or imports `dsh-tool-pwsh`.
- [`packages/shell/shell/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/tests/service.spec.ts) — A test under the owning area exercises or imports `sandboxMode`.
- [`packages/shell/pwsh-sandbox/tests/acl.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-sandbox/tests/acl.e2e.ts) — A test under the owning area exercises or imports `pwsh`. A test under the owning area exercises or imports `pwsh-sandbox`.
- Source verification intent: Unit: apps/cli/tests/windows-shell.spec.ts composes the REAL shipped bundle layers (dsh-base + dsh-web-app resolved from the app installation) through the boot's patch algorithm and pins the effective per-platform roster --- the win32 pwsh roster, the POSIX bash roster, and the base-only profile --- plus the preset-level shell-tool gates (tool-bash/tool-pwsh) and the cold-start resolution closure; packages/bundle/base/tests/base.spec.ts pins the four shell rows' symmetric !!js platform gates and that no separate platform patch ships.

## How to read the implementation

1. Start with [`packages/bundle/base/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `healProfilesModuleFallback`, `dsh`, `disabled`, `shell`, `permission`, `dsh-bash-local`, `bash -c`, `ctx.shell`, `dsh --profile headless`, `bash-sandbox`, `tool-bash`, `disabled: !!js process.platform === 'win32`, `pwsh-sandbox`, `tool-pwsh`
- Regex: `(?i)(healProfilesModuleFallback|disabled|shell|permission|dsh\-bash\-local|bash[- ]\-c|ctx\.shell|dsh[- ]\-\-profile[- ]headless)`

```bash
rg -n --pcre2 "(?i)(healProfilesModuleFallback|disabled|shell|permission|dsh\\-bash\\-local|bash[- ]\\-c|ctx\\.shell|dsh[- ]\\-\\-profile[- ]headless)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0124. Loader interpolates the entry `disabled` field](0124-loader-interpolates-the-entry-disabled-field.md): The source note links to this decision directly.
- **`source-link`** — [0240. PowerShell executor and pwsh tool](0240-powershell-executor-and-pwsh-tool.md): The source note links to this decision directly.
- **`source-link`** — [0242. pwsh tool bash parity](0242-pwsh-tool-bash-parity.md): The source note links to this decision directly.
- **`source-link`** — [0260. pwsh UI presentation matches bash](0260-pwsh-ui-presentation-matches-bash.md): The source note links to this decision directly.
- **`shares-code-with`** — [0287. Plugin configuration in the web settings page](0287-plugin-configuration-in-the-web-settings-page.md): Shares source implementation: `packages/bundle/base/src/index.ts`, `packages/bundle/base/src/invariant.ts`.
- **`shares-code-with`** — [0095. One ordering for configuration sources, and what a discovered file may not decide](0095-one-ordering-for-configuration-sources-and-what-a-discovered-file-may-no.md): Shares source implementation: `packages/bundle/base/src/index.ts`, `packages/bundle/base/src/invariant.ts`.
- **`shares-code-with`** — [0011. Agent lifecycle and ownership contracts](0011-agent-lifecycle-and-ownership-contracts.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0007. Capability seams --- Service Definition / Service Provider / Consumer roles](0007-capability-seams-service-definition-service-provider-consumer-roles.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0241-windows-defaults-to-pwsh.md`.
