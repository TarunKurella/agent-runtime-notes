---
id: "dsh-note-0246"
title: "Guarded-mutation errors append the recovery instruction at the model boundary"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-03-fs-tool-error-remedy.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/recovery"
  - "concern/trust"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "FsError"
  - "remediateFsError"
  - "cause"
  - "FS_STALE_VERSION"
  - "FS_NOT_OBSERVED"
  - "dsh-tool-fs"
  - "src/error.ts"
  - "write.ts"
  - "edit.ts"
  - "--- re-read the file, then retry"
  - "--- read the file, then retry"
  - "fs/edit-intent"
  - "dsh-fs"
  - "dsh-fs-local"
search_regex: "(?i)(FsError|remediateFsError|cause|FS_STALE_VERSION|FS_NOT_OBSERVED|dsh\\-tool\\-fs|src/error\\.ts|write\\.ts)"
---

# 0246. Guarded-mutation errors append the recovery instruction at the model boundary — implementation context

## Open this when

Guarded write and edit failures reach the model with messages that state the condition but not the only correct recovery: FS_STALE_VERSION ("file changed since it was read") and FS_NOT_OBSERVED ("edit requires reading … first"). The model must guess that the recovery is a re-read (or a first read) followed by a retry, and the retry/permission/UI layers that route on the structured code see the same message text.

## Source decision

dsh-tool-fs owns a model-facing error wrapper, remediateFsError in src/error.ts, applied in write.ts and edit.ts after the sandbox denial mapping. It appends the recovery instruction to the two guarded-mutation codes and passes everything else through untouched: FS_STALE_VERSION (including a missing edit target, which shares the stale code) gains --- re-read the file, then retry. FS_NOT_OBSERVED gains --- read the file, then retry. The structured FsError code is preserved so retry/permission/UI layers keep routing on it, and the original error chains as cause.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-03-fs-tool-error-remedy.md](../02-notes/implemented/feature/2026-08-03-fs-tool-error-remedy.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-03-fs-tool-error-remedy.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-03-fs-tool-error-remedy.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/fs/fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs`. | `named-package-member` |
| [`packages/fs/fs/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/fs/fs`. Defines `FsError`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs`. | `named-package-member` |
| [`packages/fs/tool-fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`packages/fs/tool-fs/src/error.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/error.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/tool-fs`. Defines `remediateFsError`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/fs-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/fs/tool-fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`packages/fs/fs-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/fs/fs-observation-policy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-observation-policy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-observation-policy`. | `named-package-member` |
| [`packages/fs/fs-observation-policy/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-observation-policy/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/fs/fs-observation-policy`. | `named-package-member` |
| [`packages/fs/fs-observation-policy/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-observation-policy/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs-observation-policy`. | `named-package-member` |
| [`packages/fs/fs`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `FsError` | `class` | [`packages/fs/fs/src/types.ts:196`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts#L196) | `export class FsError extends HarnessError {` |
| `remediateFsError` | `function` | [`packages/fs/tool-fs/src/error.ts:29`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/error.ts#L29) | `export function remediateFsError(error: unknown): unknown {` |
| `cause` | `const` | [`packages/llm/llm/src/error.ts:140`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts#L140) | `const cause = causeText === '' \|\| causeText === message ? '' : \`: ${causeText}\`` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/fs/fs/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/tests/service.spec.ts) — A test under the owning area exercises or imports `FsError`. A test under the owning area exercises or imports `dsh-fs-observation-policy`.
- [`packages/fs/tool-fs/tests/harness.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/harness.ts) — A test under the owning area exercises or imports `dsh-fs-observation-policy`.
- [`packages/fs/tool-fs/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/tools.spec.ts) — A test under the owning area exercises or imports `FS_STALE_VERSION`. A test under the owning area exercises or imports `FS_NOT_OBSERVED`.
- [`packages/fs/tool-fs/tests/error.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/error.spec.ts) — A test under the owning area exercises or imports `FS_STALE_VERSION`. A test under the owning area exercises or imports `FS_NOT_OBSERVED`.
- [`packages/fs/fs-local/tests/fsio.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/tests/fsio.spec.ts) — A test under the owning area exercises or imports `FS_NOT_OBSERVED`. A test under the owning area exercises or imports `FsError`.

## How to read the implementation

1. Start with [`packages/fs/fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/recovery`, `concern/trust`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `FsError`, `remediateFsError`, `cause`, `FS_STALE_VERSION`, `FS_NOT_OBSERVED`, `dsh-tool-fs`, `src/error.ts`, `write.ts`, `edit.ts`, `--- re-read the file, then retry`, `--- read the file, then retry`, `fs/edit-intent`, `dsh-fs`, `dsh-fs-local`
- Regex: `(?i)(FsError|remediateFsError|cause|FS_STALE_VERSION|FS_NOT_OBSERVED|dsh\-tool\-fs|src/error\.ts|write\.ts)`

```bash
rg -n --pcre2 "(?i)(FsError|remediateFsError|cause|FS_STALE_VERSION|FS_NOT_OBSERVED|dsh\\-tool\\-fs|src/error\\.ts|write\\.ts)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0010. Filesystem capability seam --- ctx.fs, local backend, and model-facing filesystem tools](0010-filesystem-capability-seam-ctx-fs-local-backend-and-model-facing-filesys.md): The source note links to this decision directly.
- **`source-link`** — [0356. Filesystem absence is an observation and guarded creation never replaces](0356-filesystem-absence-is-an-observation-and-guarded-creation-never-replaces.md): The source note links to this decision directly.
- **`shares-code-with`** — [0132. Filesystem tool schemas --- model-facing read/write/edit shapes](0132-filesystem-tool-schemas-model-facing-read-write-edit-shapes.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs-local/src/invariant.ts`.
- **`shares-code-with`** — [0451. Split the filesystem seam --- provider text mutations plus the `dsh-fs-observation-policy` plugin](0451-split-the-filesystem-seam-provider-text-mutations-plus-the-dsh-fs-observ.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs-local/src/invariant.ts`.
- **`shares-code-with`** — [0019. Make `dsh-fs-observation-policy` an event-gate plugin, not a method interface](0019-make-dsh-fs-observation-policy-an-event-gate-plugin-not-a-method-interfa.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs/src/index.ts`.
- **`shares-code-with`** — [0658. Prune write-only fields and a dead routing knob from the fs seam](0658-prune-write-only-fields-and-a-dead-routing-knob-from-the-fs-seam.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs/src/index.ts`.
- **`shares-code-with`** — [0548. Result-time applied-hunk diffs for file mutations](0548-result-time-applied-hunk-diffs-for-file-mutations.md): Shares source implementation: `packages/fs/fs/src/index.ts`, `packages/fs/fs/src/invariant.ts`.
- **`shares-code-with`** — [0022. Resolve filesystem paths against the caller's session cwd](0022-resolve-filesystem-paths-against-the-caller-s-session-cwd.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs-local/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0246-guarded-mutation-errors-append-the-recovery-instruction-at-the-model-bou.md`.
