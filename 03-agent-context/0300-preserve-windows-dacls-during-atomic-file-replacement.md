---
id: "dsh-note-0300"
title: "Preserve Windows DACLs during atomic file replacement"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-19-windows-atomic-write-dacl-preservation.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/trust"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/security"
  - "domain/session-state"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
aliases:
  - "GetFileSecurityW"
  - "ReplaceFileW"
  - "dsh-fs-local"
  - "Get-Acl"
  - "Preserve Windows DACLs during atomic file replacement"
  - "bug fix"
  - "boundary"
  - "evidence"
  - "ownership"
  - "trust"
  - "build release"
  - "filesystem"
  - "security"
  - "session state"
search_regex: "(?i)(GetFileSecurityW|ReplaceFileW|dsh\\-fs\\-local|Get\\-Acl|Preserve[- ]Windows[- ]DACLs[- ]during[- ]atomic[- ]file[- ]replacement|bug[- ]fix|boundary|evidence)"
---

# 0300. Preserve Windows DACLs during atomic file replacement — implementation context

## Open this when

Atomic writes protect POSIX staging directories with 0o700 and temp files with 0o600, but Windows mode bits expose only a synthetic read-only view of the actual DACL. Creating staging under the target's parent and relying on inheritance is sufficient for a new file, but not for replacing an existing file whose explicit or protected DACL is narrower than its parent: content is written under the broader parent DACL, and rename carries that staging descriptor onto the replacement.

## Source decision

dsh-fs-local reads an existing target's DACL with GetFileSecurityW, applies it to the empty temp file with inheritance protected before writing content, and publishes the closed temp with ReplaceFileW. The protected staging descriptor prevents the temp directory's inherited entries from broadening access; ReplaceFileW preserves the original target access policy and other replacement metadata. Its ACL merge may reserialize auto-inheritance state or duplicate equivalent ACEs, so self-relative descriptor buffers are not a stable equality contract.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-19-windows-atomic-write-dacl-preservation.md](../02-notes/implemented/bug-fix/2026-07-19-windows-atomic-write-dacl-preservation.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-19-windows-atomic-write-dacl-preservation.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-19-windows-atomic-write-dacl-preservation.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/fs/fs-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/fs/fs-local/src/win32.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/win32.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/fs-local`. Defines `GetFileSecurityW`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/fs-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/fs/fs-local`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/fs/fs-local/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/README.md) | package contract and examples | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/fs/fs-local/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/package.json) | composition and configuration | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-fs-local` named by the note. | `exact-code-occurrence` |
| [`examples/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/package.json) | composition and configuration | Contains the exact code literal `dsh-fs-local` named by the note. | `exact-code-occurrence` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Contains the exact code literal `dsh-fs-local` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) | package contract and examples | Contains the exact code literal `dsh-fs-local` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.zh.md) | package contract and examples | Contains the exact code literal `dsh-fs-local` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-tool-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-tool-catalog.ts) | repository automation | Contains the exact code literal `dsh-fs-local` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `GetFileSecurityW` | `type` | [`packages/fs/fs-local/src/win32.ts:9`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/win32.ts#L9) | `type GetFileSecurityW = (` |
| `ReplaceFileW` | `type` | [`packages/fs/fs-local/src/win32.ts:17`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/win32.ts#L17) | `type ReplaceFileW = (` |

### Tests and executable evidence

- [`packages/fs/fs-local/tests/fsio.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/tests/fsio.spec.ts) — A test under the owning area exercises or imports `chmod`.
- [`packages/fs/fs-local/tests/win32.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/tests/win32.spec.ts) — A test under the owning area exercises or imports `GetFileSecurityW`. A test under the owning area exercises or imports `ReplaceFileW`.

## How to read the implementation

1. Start with [`packages/fs/fs-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/evidence`, `concern/ownership`, `concern/trust`, `domain/build-release`, `domain/filesystem`, `domain/security`, `domain/session-state`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`
- Aliases: `GetFileSecurityW`, `ReplaceFileW`, `dsh-fs-local`, `Get-Acl`, `Preserve Windows DACLs during atomic file replacement`, `bug fix`, `boundary`, `evidence`, `ownership`, `trust`, `build release`, `filesystem`, `security`, `session state`
- Regex: `(?i)(GetFileSecurityW|ReplaceFileW|dsh\-fs\-local|Get\-Acl|Preserve[- ]Windows[- ]DACLs[- ]during[- ]atomic[- ]file[- ]replacement|bug[- ]fix|boundary|evidence)`

```bash
rg -n --pcre2 "(?i)(GetFileSecurityW|ReplaceFileW|dsh\\-fs\\-local|Get\\-Acl|Preserve[- ]Windows[- ]DACLs[- ]during[- ]atomic[- ]file[- ]replacement|bug[- ]fix|boundary|evidence)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0550. Windows write-permission semantics --- inherited DACLs, not mode bits](0550-windows-write-permission-semantics-inherited-dacls-not-mode-bits.md): Shares source implementation: `packages/fs/fs-local`, `packages/fs/fs-local/README.md`.
- **`shares-code-with`** — [0358. The minimal preset owns the complete RL agent composition](0358-the-minimal-preset-owns-the-complete-rl-agent-composition.md): Shares source implementation: `packages/fs/fs-local`, `packages/fs/fs-local/README.md`.
- **`shares-code-with`** — [0321. Bound overwrite contextual-diff bases at the provider](0321-bound-overwrite-contextual-diff-bases-at-the-provider.md): Shares source implementation: `packages/fs/fs-local`, `packages/fs/fs-local/README.md`.
- **`shares-code-with`** — [0201. Cross-workspace session resume](0201-cross-workspace-session-resume.md): Shares source implementation: `packages/fs/fs-local`, `packages/fs/fs-local/src/index.ts`.
- **`shares-code-with`** — [0293. Minimal profiles use the bare two-tool runtime](0293-minimal-profiles-use-the-bare-two-tool-runtime.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs-local/src/invariant.ts`.
- **`shares-code-with`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs-local/src/win32.ts`.
- **`shares-code-with`** — [0356. Filesystem absence is an observation and guarded creation never replaces](0356-filesystem-absence-is-an-observation-and-guarded-creation-never-replaces.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs-local/src/invariant.ts`.
- **`shares-code-with`** — [0178. Web UI permission presets and approval answering](0178-web-ui-permission-presets-and-approval-answering.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs-local/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0300-preserve-windows-dacls-during-atomic-file-replacement.md`.
