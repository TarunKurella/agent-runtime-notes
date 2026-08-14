---
id: "dsh-note-0245"
title: "Win32 folder picker moves to koffi in a child process"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-02-win32-in-process-folder-dialog.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
aliases:
  - "WM_CLOSE"
  - "koffi"
  - "FolderBrowserDialog"
  - "ENOENT"
  - "SetProcessDPIAware"
  - "packages/host/directory-picker-native"
  - "IFileOpenDialog"
  - "FOS_PICKFOLDERS | FOS_FORCEFILESYSTEM | FOS_NOCHANGEDIR"
  - "win32.ts"
  - "EnumThreadWindows"
  - "SetThreadDpiAwarenessContext"
  - "win32-dialog-logic.ts"
  - "win32-dialog.ts"
  - "win32-dialog-bindings.ts"
search_regex: "(?i)(WM_CLOSE|koffi|FolderBrowserDialog|ENOENT|SetProcessDPIAware|packages/host/directory\\-picker\\-native|IFileOpenDialog|FOS_PICKFOLDERS[- ]\\|[- ]FOS_FORCEFILESYSTEM[- ]\\|[- ]FOS_NOCHANGEDIR)"
---

# 0245. Win32 folder picker moves to koffi in a child process — implementation context

## Open this when

The Windows directory picker's primary tier was a spawned PowerShell script around WinForms FolderBrowserDialog: the modern dialog only where PowerShell 7 happens to be installed, a regression where PowerShell 6 resolves but has no WinForms (exit 1 is not ENOENT, so the 5.1 fallback never ran), a SetProcessDPIAware ceiling of system DPI, and a picker whose behavior depended on which shells a machine ships rather than on Windows itself.

## Source decision

The source note does not isolate a compact implementation decision; read it as a whole before changing code.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-02-win32-in-process-folder-dialog.md](../02-notes/implemented/feature/2026-08-02-win32-in-process-folder-dialog.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-02-win32-in-process-folder-dialog.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-02-win32-in-process-folder-dialog.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`native/landlock-run/packages/entry/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/packages/entry/src/index.ts) | package entry point | Core file in the package named by the note: `native/landlock-run/packages/entry`. | `named-package-member` |
| [`packages/session/session-persistence-jsonl/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session/session-persistence-jsonl`. | `named-package-member` |
| [`packages/session/session-persistence-jsonl/src/win32.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/win32.ts) | runtime implementation | Core file in the package named by the note: `packages/session/session-persistence-jsonl`. Defines `koffi`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session/session-persistence-jsonl/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-persistence-jsonl`. | `named-package-member` |
| [`packages/host/directory-picker-native/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-native/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/host/directory-picker-native`. | `named-directory-member` |
| [`packages/host/directory-picker-native/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-native/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/host/directory-picker-native`. | `named-directory-member` |
| [`packages/host/directory-picker-native/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-native/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/host/directory-picker-native`. Contains the exact code literal `packages/host/directory-picker-native` named by the note. | `exact-code-occurrence, named-directory-member` |
| [`packages/host/directory-picker-native/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-native/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/host/directory-picker-native`. | `named-directory-member` |
| [`packages/host/directory-picker-native/src/win32-dialog-bindings.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-native/src/win32-dialog-bindings.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/host/directory-picker-native`. Defines `WM_CLOSE`, a construct named by the note. | `named-directory-member, symbol-definition` |
| [`packages/host/directory-picker-native`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-native) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`native/landlock-run/packages/entry`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/packages/entry) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session/session-persistence-jsonl`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `WM_CLOSE` | `const` | [`packages/host/directory-picker-native/src/win32-dialog-bindings.ts:55`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-native/src/win32-dialog-bindings.ts#L55) | `const WM_CLOSE = 0x10` |
| `koffi` | `const` | [`packages/session/session-persistence-jsonl/src/win32.ts:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/win32.ts#L44) | `const koffi = (await import('koffi')).default` |

### Tests and executable evidence

- [`packages/session/session-persistence-jsonl/tests/win32.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/tests/win32.spec.ts) — A test under the owning area exercises or imports `koffi`.
- [`packages/host/directory-picker-native/tests/win32-dialog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-native/tests/win32-dialog.spec.ts) — A test under the owning area exercises or imports `WM_CLOSE`.
- [`packages/host/directory-picker-native/tests/win32-dialog-bindings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-native/tests/win32-dialog-bindings.spec.ts) — A test under the owning area exercises or imports `WM_CLOSE`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — Contains the exact code literal `dsh-session-persistence-jsonl` named by the note.
- [`packages/host/directory-picker-native/tests/built-worker.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-native/tests/built-worker.e2e.ts) — Contains the exact code literal `lib/worker.cjs` named by the note.

## How to read the implementation

1. Start with [`native/landlock-run/packages/entry/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/packages/entry/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/recovery`, `domain/build-release`, `domain/filesystem`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`
- Aliases: `WM_CLOSE`, `koffi`, `FolderBrowserDialog`, `ENOENT`, `SetProcessDPIAware`, `packages/host/directory-picker-native`, `IFileOpenDialog`, `FOS_PICKFOLDERS | FOS_FORCEFILESYSTEM | FOS_NOCHANGEDIR`, `win32.ts`, `EnumThreadWindows`, `SetThreadDpiAwarenessContext`, `win32-dialog-logic.ts`, `win32-dialog.ts`, `win32-dialog-bindings.ts`
- Regex: `(?i)(WM_CLOSE|koffi|FolderBrowserDialog|ENOENT|SetProcessDPIAware|packages/host/directory\-picker\-native|IFileOpenDialog|FOS_PICKFOLDERS[- ]\|[- ]FOS_FORCEFILESYSTEM[- ]\|[- ]FOS_NOCHANGEDIR)`

```bash
rg -n --pcre2 "(?i)(WM_CLOSE|koffi|FolderBrowserDialog|ENOENT|SetProcessDPIAware|packages/host/directory\\-picker\\-native|IFileOpenDialog|FOS_PICKFOLDERS[- ]\\|[- ]FOS_FORCEFILESYSTEM[- ]\\|[- ]FOS_NOCHANGEDIR)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0474. Drop the Windows PowerShell picker fallback](0474-drop-the-windows-powershell-picker-fallback.md): The source note links to this decision directly.
- **`shares-code-with`** — [0048. Zstandard JSONL session logs](0048-zstandard-jsonl-session-logs.md): Shares source implementation: `packages/session/session-persistence-jsonl/src/index.ts`, `packages/session/session-persistence-jsonl/src/invariant.ts`.
- **`shares-code-with`** — [0027. Windows-native durable JSONL publication](0027-windows-native-durable-jsonl-publication.md): Shares source implementation: `packages/session/session-persistence-jsonl/src/index.ts`, `packages/session/session-persistence-jsonl/src/invariant.ts`.
- **`shares-code-with`** — [0201. Cross-workspace session resume](0201-cross-workspace-session-resume.md): Shares source implementation: `packages/session/session-persistence-jsonl/src/index.ts`, `packages/session/session-persistence-jsonl/src/invariant.ts`.
- **`shares-code-with`** — [0070. A capability-discriminated directory-picker seam for the web-GUI host](0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md): Shares source implementation: `packages/host/directory-picker-native/src/index.ts`.
- **`shares-code-with`** — [0065. Make packed chunk rows the default JSONL layout](0065-make-packed-chunk-rows-the-default-jsonl-layout.md): Shares source implementation: `packages/session/session-persistence-jsonl/src/index.ts`, `packages/session/session-persistence-jsonl/src/invariant.ts`.
- **`shares-code-with`** — [0013. Shared persistence write coordinator](0013-shared-persistence-write-coordinator.md): Shares source implementation: `packages/session/session-persistence-jsonl/src/index.ts`, `packages/session/session-persistence-jsonl/src/invariant.ts`.
- **`shares-code-with`** — [0009. Session persistence as an abstract service over the existing `SessionEvent`](0009-session-persistence-as-an-abstract-service-over-the-existing-sessioneven.md): Shares source implementation: `packages/session/session-persistence-jsonl/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0245-win32-folder-picker-moves-to-koffi-in-a-child-process.md`.
