---
id: "dsh-note-0626"
title: "TUI diff context lines stay neutral"
status: "archived"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/bug-fix/2026-07-31-tui-diff-context-line-accounting.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/performance"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
aliases:
  - "FileDiff"
  - "diff"
  - "FileDiff.oldText"
  - "oldText"
  - "FileDiff.newText"
  - "newText"
  - "dsh-tool-fs"
  - "+A -R"
  - "advanced-cards"
  - "TUI diff context lines stay neutral"
  - "bug fix"
  - "boundary"
  - "compatibility"
  - "evidence"
search_regex: "(?i)(FileDiff|diff|FileDiff\\.oldText|oldText|FileDiff\\.newText|newText|dsh\\-tool\\-fs|\\+A[- ]\\-R)"
---

# 0626. TUI diff context lines stay neutral — implementation context

## Open this when

Result-time filesystem diffs carry the applied change with three surrounding context lines in each FileDiff.oldText and FileDiff.newText. The TUI rendered every old-side row as removed and every new-side row as added, including the identical context present on both sides. A one-line edit therefore appeared as seven removals plus seven additions, and the footer repeated those inflated totals.

## Source decision

The TUI compares each FileDiff whose old and new text are both available. Added and removed rows retain their green + and red - markers; equal context rows use the recessed body tone with a neutral two-space prefix. The footer sums only the rows classified as added or removed. maxDiffEditLength bounds the exact comparison by its combined added and removed line count; the default is 1000. Exceeding the bound renders the complete old side as removed and the complete new side as added, marks the footer approximate, and caches that result so redraws do not repeat the comparison.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/bug-fix/2026-07-31-tui-diff-context-line-accounting.md](../02-notes/archived/bug-fix/2026-07-31-tui-diff-context-line-accounting.md)
- Pinned source: [.agents/notes/archived/bug-fix/2026-07-31-tui-diff-context-line-accounting.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/bug-fix/2026-07-31-tui-diff-context-line-accounting.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/fs/tool-fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`packages/fs/tool-fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`packages/fs/tool-fs`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/loader/src/config/entry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/entry.ts) | runtime implementation | Defines `diff`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/presentation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts) | runtime implementation | Defines `FileDiff`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/README.md) | package contract and examples | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`packages/fs/tool-fs/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/package.json) | composition and configuration | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-tool-fs` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) | package contract and examples | Contains the exact code literal `dsh-tool-fs` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) | package contract and examples | Contains the exact code literal `dsh-tool-fs` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.zh.md) | package contract and examples | Contains the exact code literal `dsh-tool-fs` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.zh.md) | package contract and examples | Contains the exact code literal `dsh-tool-fs` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `FileDiff` | `interface` | [`packages/core/tools/src/presentation.ts:34`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts#L34) | `export interface FileDiff {` |
| `diff` | `const` | [`vendor/loader/src/config/entry.ts:157`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/entry.ts#L157) | `const diff = Object` |

### Tests and executable evidence

- [`packages/fs/tool-fs/tests/diff.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/diff.spec.ts) — A test under the owning area exercises or imports `oldText`. A test under the owning area exercises or imports `newText`.
- [`packages/fs/tool-fs/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/tools.spec.ts) — A test under the owning area exercises or imports `oldText`. A test under the owning area exercises or imports `newText`.

## How to read the implementation

1. Start with [`packages/fs/tool-fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/lifecycle`, `concern/performance`, `concern/recovery`, `concern/simplification`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/event-sourcing`
- Aliases: `FileDiff`, `diff`, `FileDiff.oldText`, `oldText`, `FileDiff.newText`, `newText`, `dsh-tool-fs`, `+A -R`, `advanced-cards`, `TUI diff context lines stay neutral`, `bug fix`, `boundary`, `compatibility`, `evidence`
- Regex: `(?i)(FileDiff|diff|FileDiff\.oldText|oldText|FileDiff\.newText|newText|dsh\-tool\-fs|\+A[- ]\-R)`

```bash
rg -n --pcre2 "(?i)(FileDiff|diff|FileDiff\\.oldText|oldText|FileDiff\\.newText|newText|dsh\\-tool\\-fs|\\+A[- ]\\-R)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0321. Bound overwrite contextual-diff bases at the provider](0321-bound-overwrite-contextual-diff-bases-at-the-provider.md): Shares source implementation: `packages/fs/tool-fs`, `packages/fs/tool-fs/README.md`.
- **`shares-code-with`** — [0548. Result-time applied-hunk diffs for file mutations](0548-result-time-applied-hunk-diffs-for-file-mutations.md): Shares source implementation: `packages/fs/tool-fs`, `packages/fs/tool-fs/src/index.ts`.
- **`shares-code-with`** — [0484. One editor family in general-purpose presets](0484-one-editor-family-in-general-purpose-presets.md): Shares source implementation: `packages/fs/tool-fs`, `packages/fs/tool-fs/src/index.ts`.
- **`shares-code-with`** — [0284. A minimal read_image tool over existing seams](0284-a-minimal-read-image-tool-over-existing-seams.md): Shares source implementation: `packages/fs/tool-fs`, `packages/fs/tool-fs/src/index.ts`.
- **`shares-code-with`** — [0436. verify-md-links validates fragment anchors, closing the last dead-link class](0436-verify-md-links-validates-fragment-anchors-closing-the-last-dead-link-cl.md): Shares source implementation: `packages/fs/tool-fs`, `packages/fs/tool-fs/src/index.ts`.
- **`shares-code-with`** — [0451. Split the filesystem seam --- provider text mutations plus the `dsh-fs-observation-policy` plugin](0451-split-the-filesystem-seam-provider-text-mutations-plus-the-dsh-fs-observ.md): Shares source implementation: `packages/fs/tool-fs/src/index.ts`, `packages/fs/tool-fs/src/invariant.ts`.
- **`shares-code-with`** — [0023. Tagged render-intent union for tool-call presentation](0023-tagged-render-intent-union-for-tool-call-presentation.md): Shares source implementation: `packages/fs/tool-fs/src/index.ts`, `packages/fs/tool-fs/src/invariant.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/fs/tool-fs/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0626-tui-diff-context-lines-stay-neutral.md`.
