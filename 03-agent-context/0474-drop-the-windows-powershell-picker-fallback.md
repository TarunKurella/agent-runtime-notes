---
id: "dsh-note-0474"
title: "Drop the Windows PowerShell picker fallback"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-08-04-drop-windows-powershell-picker-fallback.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/adapter"
aliases:
  - "koffi"
  - "pickNativeDirectory"
  - "resolvePwshPath"
  - "IFileOpenDialog"
  - "pwsh.exe"
  - "powershell.exe"
  - "SetProcessDPIAware"
  - "@koromix/koffi-win32-x64"
  - "ENOENT"
  - "AggregateError"
  - "directory-picker-auto"
  - "dsh-native-command"
  - "IFileDialog"
  - "FolderBrowserDialog"
search_regex: "(?i)(koffi|pickNativeDirectory|resolvePwshPath|IFileOpenDialog|pwsh\\.exe|powershell\\.exe|SetProcessDPIAware|@koromix/koffi\\-win32\\-x64)"
---

# 0474. Drop the Windows PowerShell picker fallback — implementation context

## Open this when

The win32 branch of the native directory picker kept a two-tier PowerShell fallback under the koffi IFileOpenDialog child process: pwsh.exe first, then powershell.exe (Windows PowerShell 5.1), both running the same WinForms script with a SetProcessDPIAware opt-in. The chain existed to keep a working chooser when the koffi tier was "unavailable", but every trigger it plausibly protected was a failure of our own packaging or deployment, not of the operating system: koffi's native binary ships as an ordinary optional dependency (@koromix/koffi-win32-x64, no install script); a host that installs the package at all.

## Source decision

The win32 tier is exactly the koffi IFileOpenDialog child process; any failure surfaces as-is with no fallback. The PowerShell chain --- the pwsh → Windows PowerShell 5.1 cascade, the DPI-corrected WinForms script, the AggregateError aggregation --- is deleted, and pickNativeDirectory's win32 branch is a single call. dsh-native-command remains a dependency for the POSIX tiers.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-08-04-drop-windows-powershell-picker-fallback.md](../02-notes/implemented/simplification/2026-08-04-drop-windows-powershell-picker-fallback.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-08-04-drop-windows-powershell-picker-fallback.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-08-04-drop-windows-powershell-picker-fallback.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/util/native-command/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/native-command/src/index.ts) | package entry point | Core file in the package named by the note: `packages/util/native-command`. | `named-package-member` |
| [`packages/util/native-command/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/native-command/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/util/native-command`. | `named-package-member` |
| [`packages/host/directory-picker-auto/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-auto/src/index.ts) | package entry point | Core file in the package named by the note: `packages/host/directory-picker-auto`. | `named-package-member` |
| [`packages/host/directory-picker-auto/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-auto/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/host/directory-picker-auto`. | `named-package-member` |
| [`packages/util/native-command`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/util/native-command) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/host/directory-picker-auto`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-auto) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/fs/fs-local/src/win32.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/win32.ts) | runtime implementation | Defines `koffi`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/pwsh-local/src/resolve.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/resolve.ts) | runtime implementation | Defines `resolvePwshPath`, a construct named by the note. | `symbol-definition` |
| [`packages/host/directory-picker-native/src/native-picker.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-native/src/native-picker.ts) | runtime implementation | Defines `pickNativeDirectory`, a construct named by the note. | `symbol-definition` |
| [`packages/util/native-command/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/native-command/README.md) | package contract and examples | Core file in the package named by the note: `packages/util/native-command`. | `named-package-member` |
| [`packages/util/native-command/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/native-command/package.json) | composition and configuration | Core file in the package named by the note: `packages/util/native-command`. | `named-package-member` |
| [`packages/host/directory-picker-auto/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-auto/README.md) | package contract and examples | Core file in the package named by the note: `packages/host/directory-picker-auto`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `koffi` | `const` | [`packages/fs/fs-local/src/win32.ts:48`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/win32.ts#L48) | `const koffi = (await import('koffi')).default` |
| `pickNativeDirectory` | `function` | [`packages/host/directory-picker-native/src/native-picker.ts:48`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-native/src/native-picker.ts#L48) | `export async function pickNativeDirectory(` |
| `resolvePwshPath` | `function` | [`packages/shell/pwsh-local/src/resolve.ts:67`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/resolve.ts#L67) | `export function resolvePwshPath(` |

### Tests and executable evidence

- [`packages/fs/fs-local/tests/win32.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/tests/win32.spec.ts) — A test under the owning area exercises or imports `koffi`.
- [`packages/shell/pwsh-local/tests/executor.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/tests/executor.spec.ts) — A test under the owning area exercises or imports `resolvePwshPath`.
- [`packages/host/directory-picker-auto/tests/resolve.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-auto/tests/resolve.spec.ts) — A test under the owning area exercises or imports `browse`. A test under the owning area exercises or imports `zenity`.
- [`packages/util/native-command/tests/native-command.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/native-command/tests/native-command.spec.ts) — A test under the owning area exercises or imports `dsh-native-command`.
- [`packages/host/directory-picker-native/tests/native-picker.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-native/tests/native-picker.spec.ts) — A test under the owning area exercises or imports `pickNativeDirectory`.
- [`packages/host/directory-picker-auto/tests/loader-composition.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-auto/tests/loader-composition.spec.ts) — A test under the owning area exercises or imports `browse`. A test under the owning area exercises or imports `zenity`.

## How to read the implementation

1. Start with [`packages/util/native-command/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/native-command/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/filesystem`, `domain/llm`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/adapter`
- Aliases: `koffi`, `pickNativeDirectory`, `resolvePwshPath`, `IFileOpenDialog`, `pwsh.exe`, `powershell.exe`, `SetProcessDPIAware`, `@koromix/koffi-win32-x64`, `ENOENT`, `AggregateError`, `directory-picker-auto`, `dsh-native-command`, `IFileDialog`, `FolderBrowserDialog`
- Regex: `(?i)(koffi|pickNativeDirectory|resolvePwshPath|IFileOpenDialog|pwsh\.exe|powershell\.exe|SetProcessDPIAware|@koromix/koffi\-win32\-x64)`

```bash
rg -n --pcre2 "(?i)(koffi|pickNativeDirectory|resolvePwshPath|IFileOpenDialog|pwsh\\.exe|powershell\\.exe|SetProcessDPIAware|@koromix/koffi\\-win32\\-x64)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares source implementation: `packages/fs/fs-local/src/win32.ts`.
- **`shares-code-with`** — [0209. Adaptive default for the directory-picker interaction](0209-adaptive-default-for-the-directory-picker-interaction.md): Shares source implementation: `packages/host/directory-picker-auto/src/index.ts`, `packages/host/directory-picker-auto/src/invariant.ts`.
- **`shares-code-with`** — [0240. PowerShell executor and pwsh tool](0240-powershell-executor-and-pwsh-tool.md): Shares source implementation: `packages/shell/pwsh-local/src/resolve.ts`.
- **`shares-code-with`** — [0300. Preserve Windows DACLs during atomic file replacement](0300-preserve-windows-dacls-during-atomic-file-replacement.md): Shares source implementation: `packages/fs/fs-local/src/win32.ts`.
- **`shares-code-with`** — [0284. A minimal read_image tool over existing seams](0284-a-minimal-read-image-tool-over-existing-seams.md): Shares source implementation: `packages/shell/pwsh-local/src/resolve.ts`.
- **`same-design-pressure`** — [0458. Plan-specific collaboration state](0458-plan-specific-collaboration-state.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.
- **`same-design-pressure`** — [0496. Real-API e2e in CI against the external DeepSeek API](0496-real-api-e2e-in-ci-against-the-external-deepseek-api.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0474-drop-the-windows-powershell-picker-fallback.md`.
