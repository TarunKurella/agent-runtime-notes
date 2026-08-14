---
id: "dsh-note-0550"
title: "Windows write-permission semantics --- inherited DACLs, not mode bits"
status: "archived"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/architecture/2026-07-05-windows-fs-permissions.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/trust"
  - "domain/filesystem"
  - "domain/security"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/policy"
aliases:
  - "writeFileAtomic"
  - "stat"
  - "@deepseek-ai/dsh-fs-local"
  - "stat().mode"
  - "Get-Acl"
  - "%TEMP%"
  - "Windows write-permission semantics --- inherited DACLs, not mode bits"
  - "architecture"
  - "boundary"
  - "evidence"
  - "ownership"
  - "trust"
  - "filesystem"
  - "security"
search_regex: "(?i)(writeFileAtomic|stat|@deepseek\\-ai/dsh\\-fs\\-local|stat\\(\\)\\.mode|Get\\-Acl|%TEMP%|architecture|boundary)"
---

# 0550. Windows write-permission semantics --- inherited DACLs, not mode bits — implementation context

## Open this when

writeFileAtomic in @deepseek-ai/dsh-fs-local protects write-in-progress content with POSIX mode bits: the staging directory is created 0o700, the temp file is opened 0o600, and new files default to 0o600. On POSIX this keeps temporary content owner-only regardless of the parent directory's permissions. Windows has no working equivalent behind the same API. Node's chmod there drives only the read-only attribute (every mode this package passes carries owner-write, so the calls are benign no-ops), and stat().mode reports synthetic 0o666/0o444 bits.

## Source decision

New Windows files use directory inheritance rather than synthetic mode bits: the staging directory is created inside the target's parent directory (dirname(absolutePath)), so it and the temp file inherit the destination directory's DACL. Replacement files follow the stricter DACL preservation contract. Tests assert mode bits on POSIX only. Native Windows coverage pins the package-owned replacement behavior; new-file inheritance remains an operating-system contract rather than a machine-specific ACL allowlist.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/architecture/2026-07-05-windows-fs-permissions.md](../02-notes/archived/architecture/2026-07-05-windows-fs-permissions.md)
- Pinned source: [.agents/notes/archived/architecture/2026-07-05-windows-fs-permissions.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/architecture/2026-07-05-windows-fs-permissions.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/fs/fs-local/src/fsio.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/fs-local`. Defines `writeFileAtomic`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/fs-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/fs/fs-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/fs/fs-local`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/shell/pwsh-local/src/resolve.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/resolve.ts) | runtime implementation | Defines `stat`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/fs-local/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/README.md) | package contract and examples | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/fs/fs-local/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/package.json) | composition and configuration | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `writeFileAtomic` | `function` | [`packages/fs/fs-local/src/fsio.ts:533`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L533) | `export async function writeFileAtomic(` |
| `stat` | `const` | [`packages/shell/pwsh-local/src/resolve.ts:48`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/resolve.ts#L48) | `const stat = lstatSync(candidate)` |

### Tests and executable evidence

- [`packages/fs/fs-local/tests/fsio.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/tests/fsio.spec.ts) — A test under the owning area exercises or imports `writeFileAtomic`. A test under the owning area exercises or imports `chmod`.

## How to read the implementation

1. Start with [`packages/fs/fs-local/src/fsio.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/evidence`, `concern/ownership`, `concern/trust`, `domain/filesystem`, `domain/security`, `domain/testing`, `lifecycle/archived`, `mechanism/policy`
- Aliases: `writeFileAtomic`, `stat`, `@deepseek-ai/dsh-fs-local`, `stat().mode`, `Get-Acl`, `%TEMP%`, `Windows write-permission semantics --- inherited DACLs, not mode bits`, `architecture`, `boundary`, `evidence`, `ownership`, `trust`, `filesystem`, `security`
- Regex: `(?i)(writeFileAtomic|stat|@deepseek\-ai/dsh\-fs\-local|stat\(\)\.mode|Get\-Acl|%TEMP%|architecture|boundary)`

```bash
rg -n --pcre2 "(?i)(writeFileAtomic|stat|@deepseek\\-ai/dsh\\-fs\\-local|stat\\(\\)\\.mode|Get\\-Acl|%TEMP%|architecture|boundary)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0300. Preserve Windows DACLs during atomic file replacement](0300-preserve-windows-dacls-during-atomic-file-replacement.md): Shares source implementation: `packages/fs/fs-local`, `packages/fs/fs-local/README.md`.
- **`shares-code-with`** — [0358. The minimal preset owns the complete RL agent composition](0358-the-minimal-preset-owns-the-complete-rl-agent-composition.md): Shares source implementation: `packages/fs/fs-local`, `packages/fs/fs-local/README.md`.
- **`shares-code-with`** — [0321. Bound overwrite contextual-diff bases at the provider](0321-bound-overwrite-contextual-diff-bases-at-the-provider.md): Shares source implementation: `packages/fs/fs-local`, `packages/fs/fs-local/README.md`.
- **`shares-code-with`** — [0022. Resolve filesystem paths against the caller's session cwd](0022-resolve-filesystem-paths-against-the-caller-s-session-cwd.md): Shares source implementation: `packages/fs/fs-local/src/fsio.ts`, `packages/fs/fs-local/src/index.ts`.
- **`shares-code-with`** — [0451. Split the filesystem seam --- provider text mutations plus the `dsh-fs-observation-policy` plugin](0451-split-the-filesystem-seam-provider-text-mutations-plus-the-dsh-fs-observ.md): Shares source implementation: `packages/fs/fs-local/src/fsio.ts`, `packages/fs/fs-local/src/index.ts`.
- **`shares-code-with`** — [0132. Filesystem tool schemas --- model-facing read/write/edit shapes](0132-filesystem-tool-schemas-model-facing-read-write-edit-shapes.md): Shares source implementation: `packages/fs/fs-local/src/fsio.ts`, `packages/fs/fs-local/src/index.ts`.
- **`shares-code-with`** — [0201. Cross-workspace session resume](0201-cross-workspace-session-resume.md): Shares source implementation: `packages/fs/fs-local`, `packages/fs/fs-local/src/index.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/fs/fs-local/src/fsio.ts`, `packages/fs/fs-local/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0550-windows-write-permission-semantics-inherited-dacls-not-mode-bits.md`.
