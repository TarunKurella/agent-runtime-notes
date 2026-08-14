---
id: "dsh-note-0321"
title: "Bound overwrite contextual-diff bases at the provider"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-30-bounded-overwrite-diff-basis.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/streaming"
aliases:
  - "probe"
  - "before"
  - "after"
  - "dsh-fs-local"
  - "FsWriteOutcome.before"
  - "LocalFileSystem.Config.diffBasisMaxBytes"
  - "diffBasisMaxBytes"
  - "tool-fs"
  - "tool-fs.readStreamMinSize"
  - "Bound overwrite contextual-diff bases at the provider"
  - "bug fix"
  - "boundary"
  - "cancellation timeout"
  - "discovery routing"
search_regex: "(?i)(probe|before|after|dsh\\-fs\\-local|FsWriteOutcome\\.before|LocalFileSystem\\.Config\\.diffBasisMaxBytes|diffBasisMaxBytes|tool\\-fs)"
---

# 0321. Bound overwrite contextual-diff bases at the provider — implementation context

## Open this when

dsh-fs-local returned the complete prior file in FsWriteOutcome.before so consumers could build a contextual overwrite diff. That presentation-only pre-read was unbounded: a large overwrite could allocate the entire prior file, and checking an earlier path stat alone could not enforce a limit because an external process could replace or grow the file between the stat and the read. A large replacement also made the contextual hunk approach the replacement size even when the prior file was small. This closes the deferred bound recorded by result-time applied-hunk diffs.

## Source decision

LocalFileSystem.Config.diffBasisMaxBytes is a positive safe-integer deployment setting no greater than the runtime's Buffer-allocation and string-decoding limits, with a 10 MiB default. An overwrite supplies before only when the UTF-8 replacement is strictly below that limit and the prior file opened for the basis also ends below it. The prior read opens a descriptor, checks that descriptor, and reads at most the configured byte count in cancellation-aware chunks; reaching the boundary returns null.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-30-bounded-overwrite-diff-basis.md](../02-notes/implemented/bug-fix/2026-07-30-bounded-overwrite-diff-basis.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-30-bounded-overwrite-diff-basis.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-30-bounded-overwrite-diff-basis.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/fs/fs-local/src/fsio.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/fs-local`. Defines `probe`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/tool-fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`packages/fs/fs-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-local`. Defines `before`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/tool-fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`packages/fs/fs-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/fs/tool-fs`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/fs/fs-local`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/fs/tool-fs/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/README.md) | package contract and examples | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`packages/fs/fs-local/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/README.md) | package contract and examples | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/fs/tool-fs/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/package.json) | composition and configuration | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`packages/fs/fs-local/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/package.json) | composition and configuration | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-fs-local` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `probe` | `function` | [`packages/fs/fs-local/src/fsio.ts:230`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L230) | `export async function probe(absolutePath: string): Promise<PathInfo \| null> {` |
| `before` | `const` | [`packages/fs/fs-local/src/index.ts:197`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts#L197) | `const before = diffable` |
| `after` | `const` | [`packages/fs/fs-local/src/index.ts:208`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts#L208) | `const after = await probe(target.targetKey)` |
| `after` | `const` | [`packages/fs/fs-local/src/index.ts:246`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts#L246) | `const after = await probe(target.targetKey)` |

### Tests and executable evidence

- [`packages/fs/tool-fs/tests/read-image.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/read-image.spec.ts) — A test under the owning area exercises or imports `tool-fs`.
- [`packages/fs/fs-local/tests/filesystem.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/tests/filesystem.spec.ts) — A test under the owning area exercises or imports `diffBasisMaxBytes`.

## How to read the implementation

1. Start with [`packages/fs/fs-local/src/fsio.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/streaming`
- Aliases: `probe`, `before`, `after`, `dsh-fs-local`, `FsWriteOutcome.before`, `LocalFileSystem.Config.diffBasisMaxBytes`, `diffBasisMaxBytes`, `tool-fs`, `tool-fs.readStreamMinSize`, `Bound overwrite contextual-diff bases at the provider`, `bug fix`, `boundary`, `cancellation timeout`, `discovery routing`
- Regex: `(?i)(probe|before|after|dsh\-fs\-local|FsWriteOutcome\.before|LocalFileSystem\.Config\.diffBasisMaxBytes|diffBasisMaxBytes|tool\-fs)`

```bash
rg -n --pcre2 "(?i)(probe|before|after|dsh\\-fs\\-local|FsWriteOutcome\\.before|LocalFileSystem\\.Config\\.diffBasisMaxBytes|diffBasisMaxBytes|tool\\-fs)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0548. Result-time applied-hunk diffs for file mutations](0548-result-time-applied-hunk-diffs-for-file-mutations.md): The source note links to this decision directly.
- **`shares-code-with`** — [0451. Split the filesystem seam --- provider text mutations plus the `dsh-fs-observation-policy` plugin](0451-split-the-filesystem-seam-provider-text-mutations-plus-the-dsh-fs-observ.md): Shares source implementation: `packages/fs/fs-local/src/fsio.ts`, `packages/fs/fs-local/src/index.ts`.
- **`shares-code-with`** — [0022. Resolve filesystem paths against the caller's session cwd](0022-resolve-filesystem-paths-against-the-caller-s-session-cwd.md): Shares source implementation: `packages/fs/fs-local/src/fsio.ts`, `packages/fs/fs-local/src/index.ts`.
- **`shares-code-with`** — [0358. The minimal preset owns the complete RL agent composition](0358-the-minimal-preset-owns-the-complete-rl-agent-composition.md): Shares source implementation: `packages/fs/fs-local`, `packages/fs/fs-local/README.md`.
- **`shares-code-with`** — [0132. Filesystem tool schemas --- model-facing read/write/edit shapes](0132-filesystem-tool-schemas-model-facing-read-write-edit-shapes.md): Shares source implementation: `packages/fs/fs-local/src/fsio.ts`, `packages/fs/fs-local/src/index.ts`.
- **`shares-code-with`** — [0550. Windows write-permission semantics --- inherited DACLs, not mode bits](0550-windows-write-permission-semantics-inherited-dacls-not-mode-bits.md): Shares source implementation: `packages/fs/fs-local`, `packages/fs/fs-local/README.md`.
- **`shares-code-with`** — [0626. TUI diff context lines stay neutral](0626-tui-diff-context-lines-stay-neutral.md): Shares source implementation: `packages/fs/tool-fs`, `packages/fs/tool-fs/README.md`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/fs/fs-local/src/fsio.ts`, `packages/fs/fs-local/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0321-bound-overwrite-contextual-diff-bases-at-the-provider.md`.
