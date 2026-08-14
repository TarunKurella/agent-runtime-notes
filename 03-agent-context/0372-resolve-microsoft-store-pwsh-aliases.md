---
id: "dsh-note-0372"
title: "Resolve Microsoft Store pwsh aliases"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-12-resolve-store-pwsh-aliases.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/bug-fix"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/security"
  - "domain/shell-terminal"
  - "domain/testing"
  - "lifecycle/implemented"
aliases:
  - "candidateExists"
  - "resolvePwshPath"
  - "existsSync"
  - "%LOCALAPPDATA%\\Microsoft\\WindowsApps\\pwsh.exe"
  - "Resolve Microsoft Store pwsh aliases"
  - "bug fix"
  - "discovery routing"
  - "evidence"
  - "recovery"
  - "build release"
  - "filesystem"
  - "security"
  - "shell terminal"
  - "testing"
search_regex: "(?i)(candidateExists|resolvePwshPath|existsSync|%LOCALAPPDATA%\\\\Microsoft\\\\WindowsApps\\\\pwsh\\.exe|Resolve[- ]Microsoft[- ]Store[- ]pwsh[- ]aliases|bug[- ]fix|discovery[- ]routing|evidence)"
---

# 0372. Resolve Microsoft Store pwsh aliases — implementation context

## Open this when

resolvePwshPath documented that Microsoft Store installs resolve through PATH, but its existence probe was existsSync, which stats a candidate and therefore follows reparse points. The Store's %LOCALAPPDATA%\Microsoft\WindowsApps\pwsh.exe is an app execution alias whose target directory ACL refuses stat (EACCES), so existsSync missed it and resolution silently fell through to Windows PowerShell 5.1 on hosts whose only PowerShell 7 is a Store install.

## Source decision

candidateExists accepts a candidate that stats as a file or that lstat sees as a link-shaped reparse point, and resolvePwshPath uses it. Spawning the alias path works because CreateProcess resolves app execution aliases. A dangling link-shaped candidate is accepted so a broken pwsh fails loudly at spawn instead of silently downgrading to 5.1.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-12-resolve-store-pwsh-aliases.md](../02-notes/implemented/bug-fix/2026-08-12-resolve-store-pwsh-aliases.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-12-resolve-store-pwsh-aliases.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-12-resolve-store-pwsh-aliases.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/shell/pwsh-local/src/resolve.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/resolve.ts) | runtime implementation | Defines `resolvePwshPath`, a construct named by the note. Defines `candidateExists`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `candidateExists` | `function` | [`packages/shell/pwsh-local/src/resolve.ts:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/resolve.ts#L46) | `function candidateExists(candidate: string): boolean {` |
| `resolvePwshPath` | `function` | [`packages/shell/pwsh-local/src/resolve.ts:67`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/resolve.ts#L67) | `export function resolvePwshPath(` |

### Tests and executable evidence

- [`scripts/clean.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/clean.spec.ts) — A test under the owning area exercises or imports `existsSync`.
- [`scripts/change-scope.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/change-scope.spec.ts) — A test under the owning area exercises or imports `existsSync`.
- [`scripts/client-tsconfig.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-tsconfig.spec.ts) — A test under the owning area exercises or imports `existsSync`.
- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — A test under the owning area exercises or imports `existsSync`.
- [`scripts/project-doc-site.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.spec.ts) — A test under the owning area exercises or imports `existsSync`.
- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — A test under the owning area exercises or imports `existsSync`.
- [`packages/shell/pwsh-local/tests/executor.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/tests/executor.spec.ts) — A test under the owning area exercises or imports `resolvePwshPath`.

## How to read the implementation

1. Start with [`packages/shell/pwsh-local/src/resolve.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/resolve.ts) because it has the strongest evidence link to the note.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/discovery-routing`, `concern/evidence`, `concern/recovery`, `domain/build-release`, `domain/filesystem`, `domain/security`, `domain/shell-terminal`, `domain/testing`, `lifecycle/implemented`
- Aliases: `candidateExists`, `resolvePwshPath`, `existsSync`, `%LOCALAPPDATA%\Microsoft\WindowsApps\pwsh.exe`, `Resolve Microsoft Store pwsh aliases`, `bug fix`, `discovery routing`, `evidence`, `recovery`, `build release`, `filesystem`, `security`, `shell terminal`, `testing`
- Regex: `(?i)(candidateExists|resolvePwshPath|existsSync|%LOCALAPPDATA%\\Microsoft\\WindowsApps\\pwsh\.exe|Resolve[- ]Microsoft[- ]Store[- ]pwsh[- ]aliases|bug[- ]fix|discovery[- ]routing|evidence)`

```bash
rg -n --pcre2 "(?i)(candidateExists|resolvePwshPath|existsSync|%LOCALAPPDATA%\\\\Microsoft\\\\WindowsApps\\\\pwsh\\.exe|Resolve[- ]Microsoft[- ]Store[- ]pwsh[- ]aliases|bug[- ]fix|discovery[- ]routing|evidence)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0415. Make Lefthook installation worktree-local](0415-make-lefthook-installation-worktree-local.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/install-lefthook.spec.ts`.
- **`shares-code-with`** — [0499. Keep supported-platform tests semantic](0499-keep-supported-platform-tests-semantic.md): Shares source implementation: `scripts/client-tsconfig.spec.ts`, `scripts/install-lefthook.spec.ts`.
- **`shares-code-with`** — [0421. Coverage-exempt heavy suites](0421-coverage-exempt-heavy-suites.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/install-lefthook.spec.ts`.
- **`shares-code-with`** — [0373. Unlink fixture junctions before recursive deletion](0373-unlink-fixture-junctions-before-recursive-deletion.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/clean.spec.ts`.
- **`shares-code-with`** — [0646. Wine-run Windows blocking gates on Linux runners](0646-wine-run-windows-blocking-gates-on-linux-runners.md): Shares source implementation: `scripts/clean.spec.ts`, `scripts/oxlint-contract.spec.ts`.
- **`shares-code-with`** — [0411. Provision CI pnpm via pnpm/action-setup](0411-provision-ci-pnpm-via-pnpm-action-setup.md): Shares source implementation: `scripts/install-lefthook.spec.ts`, `scripts/oxlint-contract.spec.ts`.
- **`shares-code-with`** — [0425. The documentation site carries its own images](0425-the-documentation-site-carries-its-own-images.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/project-doc-site.spec.ts`.
- **`shares-code-with`** — [0441. Python public publication workflow](0441-python-public-publication-workflow.md): Shares source implementation: `scripts/clean.spec.ts`, `scripts/project-doc-site.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0372-resolve-microsoft-store-pwsh-aliases.md`.
