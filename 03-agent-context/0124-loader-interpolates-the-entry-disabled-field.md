---
id: "dsh-note-0124"
title: "Loader interpolates the entry `disabled` field"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-11-loader-entry-disabled-interpolation.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/trust"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "disabled"
  - "platform"
  - "inject"
  - "group"
  - "config"
  - "windows.cordis.patch.yml"
  - "tool-bash"
  - "tool-pwsh"
  - "disabled: !!js ..."
  - "vendor/loader/src/config/entry.ts"
  - "disabled: !!js process.platform === 'win32"
  - "verify-cordis-config"
  - "cordis.patch.yml"
  - "bash-sandbox"
search_regex: "(?i)(disabled|platform|inject|group|config|windows\\.cordis\\.patch\\.yml|tool\\-bash|tool\\-pwsh)"
---

# 0124. Loader interpolates the entry `disabled` field — implementation context

## Open this when

The Windows platform layer (then a separate windows.cordis.patch.yml beside the base patch, since folded into the base rows --- see Decision) disabled tool-bash on win32, but the shipped presets each mount a tool-bash row. Preset rows compose last, so the same-id row re-enabled the tool on Windows --- the session had both tool-bash (PowerShell-backed) and tool-pwsh, silently, because no spec pinned the composed preset layer. Entry metadata had no conditional mechanism: !!js interpolates only under plugin config, and postmortem 0002 documents that disabled: !!js ...

## Source decision

The Loader interpolates the entry disabled field (vendor/loader/src/config/entry.ts): a !!js expression evaluates against the loader context at every mount decision. disabled is the only interpolated metadata field; id, name, group, and inject stay static. The raw node stays in the options, so write-back keeps the !!js form. The shipped presets (standard, code, cordis) declare the shell tool rows themselves and gate them by platform --- tool-bash with disabled: !!js process.platform === 'win32' and its tool-pwsh twin with the inverted expression --- so the preset layer exposes exactly one shell tool per host.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-11-loader-entry-disabled-interpolation.md](../02-notes/implemented/architecture/2026-08-11-loader-entry-disabled-interpolation.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-11-loader-entry-disabled-interpolation.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-11-loader-entry-disabled-interpolation.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/loader/src/config/entry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/entry.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`docs/postmortem/0002-js-expression-disabled-filesystem-tools.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/postmortem/0002-js-expression-disabled-filesystem-tools.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`vendor/group/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/group/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/group`. | `named-package-member` |
| [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-bash`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/tool-pwsh/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-pwsh`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/bash-sandbox/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/bash-sandbox`. | `named-package-member` |
| [`packages/shell/pwsh-sandbox/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-sandbox/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/pwsh-sandbox`. | `named-package-member` |
| [`packages/shell/tool-bash/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/tool-bash`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/tool-pwsh/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/tool-pwsh`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/bash-sandbox/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/bash-sandbox`. | `named-package-member` |
| [`packages/shell/pwsh-sandbox/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-sandbox/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/pwsh-sandbox`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`vendor/group`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/vendor/group) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `disabled` | `const` | [`packages/client/ui-settings-plugins/src/client/BashCard.tsx:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/client/BashCard.tsx#L23) | `const disabled = !state.writable` |
| `platform` | `const` | [`packages/fs/fs-local/src/fsio.ts:550`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L550) | `const platform = internals.platform ?? process.platform` |
| `inject` | `const` | [`packages/shell/pwsh-sandbox/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-sandbox/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `inject` | `const` | [`packages/shell/tool-bash/src/index.ts:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts#L31) | `export const inject = ['tools', 'shell', 'systemPrompt', 'shellEnv']` |
| `inject` | `const` | [`packages/shell/tool-bash/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `inject` | `const` | [`packages/shell/tool-pwsh/src/index.ts:49`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/src/index.ts#L49) | `export const inject = ['tools', 'shell', 'systemPrompt', 'shellEnv']` |
| `inject` | `const` | [`packages/shell/tool-pwsh/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `group` | `const` | [`vendor/loader/src/config/tree.ts:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/tree.ts#L98) | `const group = this.resolveGroup(parent)` |
| `config` | `const` | [`vendor/loader/src/index.ts:93`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts#L93) | `const config = next()` |

### Tests and executable evidence

- [`packages/shell/tool-pwsh/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/tests/tools.spec.ts) — A test under the owning area exercises or imports `tool-pwsh`.
- [`packages/shell/pwsh-sandbox/tests/acl.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-sandbox/tests/acl.e2e.ts) — A test under the owning area exercises or imports `pwsh-sandbox`.
- [`packages/shell/tool-pwsh/tests/loader.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/tests/loader.spec.ts) — A test under the owning area exercises or imports `tool-pwsh`.
- [`packages/shell/bash-sandbox/tests/bwrap.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/tests/bwrap.e2e.ts) — A test under the owning area exercises or imports `bash-sandbox`.
- [`packages/shell/pwsh-sandbox/tests/sandbox.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-sandbox/tests/sandbox.spec.ts) — A test under the owning area exercises or imports `bash-sandbox`.
- [`packages/shell/bash-sandbox/tests/landlock.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/tests/landlock.e2e.ts) — A test under the owning area exercises or imports `bash-sandbox`.
- [`packages/shell/bash-sandbox/tests/seatbelt.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/tests/seatbelt.e2e.ts) — A test under the owning area exercises or imports `bash-sandbox`.

## How to read the implementation

1. Start with [`vendor/loader/src/config/entry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/entry.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/evidence`, `concern/ownership`, `concern/trust`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `disabled`, `platform`, `inject`, `group`, `config`, `windows.cordis.patch.yml`, `tool-bash`, `tool-pwsh`, `disabled: !!js ...`, `vendor/loader/src/config/entry.ts`, `disabled: !!js process.platform === 'win32`, `verify-cordis-config`, `cordis.patch.yml`, `bash-sandbox`
- Regex: `(?i)(disabled|platform|inject|group|config|windows\.cordis\.patch\.yml|tool\-bash|tool\-pwsh)`

```bash
rg -n --pcre2 "(?i)(disabled|platform|inject|group|config|windows\\.cordis\\.patch\\.yml|tool\\-bash|tool\\-pwsh)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0241. Windows defaults to pwsh](0241-windows-defaults-to-pwsh.md): The source note links to this decision directly.
- **`shares-code-with`** — [0242. pwsh tool bash parity](0242-pwsh-tool-bash-parity.md): Shares source implementation: `packages/shell/tool-bash/src/index.ts`, `packages/shell/tool-bash/src/invariant.ts`.
- **`shares-code-with`** — [0260. pwsh UI presentation matches bash](0260-pwsh-ui-presentation-matches-bash.md): Shares source implementation: `packages/shell/tool-bash/src/index.ts`, `packages/shell/tool-bash/src/invariant.ts`.
- **`shares-code-with`** — [0130. Multiplex concurrent ACP sessions over one connection](0130-multiplex-concurrent-acp-sessions-over-one-connection.md): Shares source implementation: `packages/shell/tool-bash/src/index.ts`, `packages/shell/tool-pwsh/src/index.ts`.
- **`shares-code-with`** — [0238. Workspace-write defaults for shipped surfaces](0238-workspace-write-defaults-for-shipped-surfaces.md): Shares source implementation: `packages/shell/bash-sandbox/src/index.ts`, `packages/shell/bash-sandbox/src/invariant.ts`.
- **`shares-code-with`** — [0240. PowerShell executor and pwsh tool](0240-powershell-executor-and-pwsh-tool.md): Shares source implementation: `packages/shell/tool-bash/src/index.ts`, `packages/shell/tool-pwsh/src/index.ts`.
- **`shares-code-with`** — [0555. Consolidated TUI presentation and navigation](0555-consolidated-tui-presentation-and-navigation.md): Shares source implementation: `packages/shell/tool-bash/src/index.ts`, `packages/shell/tool-bash/src/invariant.ts`.
- **`shares-code-with`** — [0011. Agent lifecycle and ownership contracts](0011-agent-lifecycle-and-ownership-contracts.md): Shares source implementation: `packages/shell/tool-bash/src/index.ts`, `packages/shell/tool-bash/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0124-loader-interpolates-the-entry-disabled-field.md`.
