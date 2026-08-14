---
id: "dsh-note-0451"
title: "Split the filesystem seam --- provider text mutations plus the `dsh-fs-observation-policy` plugin"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-06-26-fsspec-style-fs-seam.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "exec"
  - "version"
  - "size"
  - "info"
  - "FileSystem"
  - "FsTargetKey"
  - "FsVersion"
  - "FsWriteIntent"
  - "FsErrorCode"
  - "FsError"
  - "offset"
  - "limit"
  - "partial"
  - "stat"
search_regex: "(?i)(exec|version|size|info|FileSystem|FsTargetKey|FsVersion|FsWriteIntent)"
---

# 0451. Split the filesystem seam --- provider text mutations plus the `dsh-fs-observation-policy` plugin — implementation context

## Open this when

The filesystem capability from filesystem-capability-seam currently makes one abstract FileSystem service own two different jobs: Provider operations --- resolving targets, stat/version metadata, text reads/streams, atomic writes, and guarded literal edits. Agent-facing policy --- line windows, literal edit semantics, and read-before-write/edit observed-state. That makes every future backend reimplement model-facing read semantics and observation policy. readPage returns numbered lines and view metadata; the base service stores per-owner file state and distinguishes full from partial reads.

## Source decision

Split the stack into four layers: dsh-tool-fs keeps the same model-facing read/write/edit schemas. It is the executor: it injects fs (not a policy service) and reaches ctx.fs directly, owns read windowing, and dispatches the fs/ events so dsh-fs-observation-policy can gate and record. This Agent Note decided the four-layer split, the provider contract, and the freshness policy.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-06-26-fsspec-style-fs-seam.md](../02-notes/implemented/simplification/2026-06-26-fsspec-style-fs-seam.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-06-26-fsspec-style-fs-seam.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-06-26-fsspec-style-fs-seam.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/fs/fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs`. Defines `FileSystem`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/fs/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/fs/fs`. Defines `FsErrorCode`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs`. | `named-package-member` |
| [`packages/fs/tool-fs/src/read.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/tool-fs`. Defines `offset`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/fs-local/src/fsio.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/fs-local`. Defines `info`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/tool-fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`packages/fs/fs-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-local`. Defines `info`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/tool-fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`packages/fs/fs-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/fs/tool-fs/src/read-target.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read-target.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`packages/fs/fs-observation-policy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-observation-policy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-observation-policy`. | `named-package-member` |
| [`packages/fs/fs-observation-policy/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-observation-policy/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/fs/fs-observation-policy`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `exec` | `const` | [`packages/core/tools/src/index.ts:1469`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1469) | `const exec = created.exec` |
| `version` | `const` | [`packages/e2b/fs-e2b/src/index.ts:389`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L389) | `const version = await this.writeAtomic(` |
| `size` | `const` | [`packages/e2b/subprocess-e2b/src/process.ts:673`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/process.ts#L673) | `const size = (reader as E2BOutputReader).size` |
| `info` | `const` | [`packages/fs/fs-local/src/fsio.ts:231`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L231) | `const info = await probeStats(absolutePath, path => stat(path, { bigint: true }))` |
| `info` | `const` | [`packages/fs/fs-local/src/fsio.ts:247`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L247) | `const info = await probeStats(absolutePath, path => lstat(path, { bigint: true }))` |
| `info` | `const` | [`packages/fs/fs-local/src/fsio.ts:402`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L402) | `const info = await statRegularFile(target, 'read', signal)` |
| `info` | `const` | [`packages/fs/fs-local/src/fsio.ts:708`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L708) | `const info = await handle.stat()` |
| `info` | `const` | [`packages/fs/fs-local/src/index.ts:128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts#L128) | `const info = await probe(target.targetKey)` |
| `FileSystem` | `class` | [`packages/fs/fs/src/index.ts:86`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/index.ts#L86) | `export abstract class FileSystem extends Service {` |
| `FsTargetKey` | `type` | [`packages/fs/fs/src/types.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts#L16) | `export type FsTargetKey = Branded<'FsTargetKey'>` |
| `FsTargetKey` | `function` | [`packages/fs/fs/src/types.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts#L24) | `export function FsTargetKey(key: string): FsTargetKey {` |
| `FsVersion` | `type` | [`packages/fs/fs/src/types.ts:35`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts#L35) | `export type FsVersion = Branded<'FsVersion'>` |
| `FsVersion` | `function` | [`packages/fs/fs/src/types.ts:43`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts#L43) | `export function FsVersion(v: string): FsVersion {` |
| `FsWriteIntent` | `type` | [`packages/fs/fs/src/types.ts:123`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts#L123) | `export type FsWriteIntent =` |
| `FsErrorCode` | `type` | [`packages/fs/fs/src/types.ts:175`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts#L175) | `export type FsErrorCode =` |
| `FsError` | `class` | [`packages/fs/fs/src/types.ts:196`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts#L196) | `export class FsError extends HarnessError {` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/fs/tool-fs/tests/harness.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/harness.ts) — A test under the owning area exercises or imports `dsh-fs-observation-policy`.
- [`packages/fs/fs/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/tests/service.spec.ts) — A test under the owning area exercises or imports `dsh-fs-observation-policy`. A test under the owning area exercises or imports `FileSystem`.
- [`packages/fs/fs/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/tests/invariant.spec.ts) — A test under the owning area exercises or imports `FsTargetKey`. A test under the owning area exercises or imports `FsVersion`.
- [`packages/fs/tool-fs/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/tools.spec.ts) — A test under the owning area exercises or imports `dsh-fs-observation-policy`. A test under the owning area exercises or imports `FileSystem`.
- [`packages/fs/fs-local/tests/fsio.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/tests/fsio.spec.ts) — A test under the owning area exercises or imports `dsh-fs-observation-policy`. A test under the owning area exercises or imports `FS_NOT_TEXT`.
- Source verification intent: dsh-fs exposes exactly resolve/stat/readText/streamText/writeText/editText (stat returning FsInfo | undefined, writeText taking FsWriteIntent), with the removed types/primitives gone; dsh-fs-local carries no line, view, or formatReadBody logic; model-facing schemas stayed byte-for-byte unchanged. Tests pin that a windowed read authorizes a later edit of an unchanged file, that an edit based on a stale read reports FS_STALE_VERSION before attempting literal matching, that version-CAS behavior is preserved, and that the observation contract holds (a read-tool read records observed-state; a direct ctx.fs read does.

## How to read the implementation

1. Start with [`packages/fs/fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `exec`, `version`, `size`, `info`, `FileSystem`, `FsTargetKey`, `FsVersion`, `FsWriteIntent`, `FsErrorCode`, `FsError`, `offset`, `limit`, `partial`, `stat`
- Regex: `(?i)(exec|version|size|info|FileSystem|FsTargetKey|FsVersion|FsWriteIntent)`

```bash
rg -n --pcre2 "(?i)(exec|version|size|info|FileSystem|FsTargetKey|FsVersion|FsWriteIntent)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0549. Add direct directory listing to the filesystem seam](0549-add-direct-directory-listing-to-the-filesystem-seam.md): The source note links to this decision directly.
- **`source-link`** — [0010. Filesystem capability seam --- ctx.fs, local backend, and model-facing filesystem tools](0010-filesystem-capability-seam-ctx-fs-local-backend-and-model-facing-filesys.md): The source note links to this decision directly.
- **`source-link`** — [0014. Branded IDs everywhere they belong](0014-branded-ids-everywhere-they-belong.md): The source note links to this decision directly.
- **`source-link`** — [0019. Make `dsh-fs-observation-policy` an event-gate plugin, not a method interface](0019-make-dsh-fs-observation-policy-an-event-gate-plugin-not-a-method-interfa.md): The source note links to this decision directly.
- **`shares-code-with`** — [0132. Filesystem tool schemas --- model-facing read/write/edit shapes](0132-filesystem-tool-schemas-model-facing-read-write-edit-shapes.md): Shares source implementation: `packages/fs/fs-local/src/fsio.ts`, `packages/fs/fs-local/src/index.ts`.
- **`shares-code-with`** — [0658. Prune write-only fields and a dead routing knob from the fs seam](0658-prune-write-only-fields-and-a-dead-routing-knob-from-the-fs-seam.md): Shares source implementation: `packages/fs/fs-local/src/fsio.ts`, `packages/fs/fs-local/src/index.ts`.
- **`shares-code-with`** — [0246. Guarded-mutation errors append the recovery instruction at the model boundary](0246-guarded-mutation-errors-append-the-recovery-instruction-at-the-model-bou.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs-local/src/invariant.ts`.
- **`shares-code-with`** — [0548. Result-time applied-hunk diffs for file mutations](0548-result-time-applied-hunk-diffs-for-file-mutations.md): Shares source implementation: `packages/fs/fs/src/index.ts`, `packages/fs/fs/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0451-split-the-filesystem-seam-provider-text-mutations-plus-the-dsh-fs-observ.md`.
