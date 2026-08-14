---
id: "dsh-note-0356"
title: "Filesystem absence is an observation and guarded creation never replaces"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-09-filesystem-absence-observation.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
aliases:
  - "view"
  - "present"
  - "insert"
  - "stat"
  - "absent"
  - "FS_NOT_FOUND"
  - "fs/observed"
  - "replaceIfVersion"
  - "createIfAbsent"
  - "dsh-fs"
  - "{ kind: 'present', version: FsVersion } | { kind: 'absent' }"
  - "str_replace_editor"
  - "str_replace"
  - "dsh-fs-observation-policy"
search_regex: "(?i)(view|present|insert|stat|absent|FS_NOT_FOUND|fs/observed|replaceIfVersion)"
---

# 0356. Filesystem absence is an observation and guarded creation never replaces — implementation context

## Open this when

The event-gated filesystem policy originally records only successful reads and mutations as a target version. If a session reads a file and an external command deletes it, the first guarded mutation correctly fails stale, but the prescribed reread returns FS_NOT_FOUND before emitting fs/observed. The old positive version therefore remains forever: write keeps choosing replaceIfVersion, the provider keeps rejecting the missing target, and the model-facing "re-read the file, then retry" instruction becomes an unrecoverable loop. Treating a failed read as permission to create also exposes a second boundary.

## Source decision

dsh-fs owns an explicit observation union: { kind: 'present', version: FsVersion } | { kind: 'absent' }. The fs/observed event carries that union. Successful reads and mutations emit present; a metadata miss from read or the str_replace_editor view, str_replace, or insert command emits absent synchronously before returning FS_NOT_FOUND. Other read failures do not manufacture absence. dsh-fs-observation-policy stores three logical states per owner and target without injecting or calling ctx.fs: missing map entry is unseen, absent is confirmed absence, and present(version) is a replacement/edit basis.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-09-filesystem-absence-observation.md](../02-notes/implemented/bug-fix/2026-08-09-filesystem-absence-observation.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-09-filesystem-absence-observation.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-09-filesystem-absence-observation.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/fs/fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs`. | `named-package-member` |
| [`packages/fs/fs/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/fs/fs`. | `named-package-member` |
| [`packages/fs/fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs`. | `named-package-member` |
| [`packages/e2b/fs-e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts) | package entry point | Core file in the package named by the note: `packages/e2b/fs-e2b`. | `named-package-member` |
| [`packages/fs/fs-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/e2b/fs-e2b/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/e2b/fs-e2b`. | `named-package-member` |
| [`packages/fs/fs-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/fs/fs-observation-policy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-observation-policy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-observation-policy`. | `named-package-member` |
| [`packages/fs/fs-observation-policy/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-observation-policy/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/fs/fs-observation-policy`. | `named-package-member` |
| [`packages/fs/fs-observation-policy/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-observation-policy/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs-observation-policy`. | `named-package-member` |
| [`packages/fs/fs`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/e2b/fs-e2b`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `view` | `const` | [`packages/goal/goal/src/index.ts:535`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L535) | `const view = this.view(cache)` |
| `present` | `function` | [`packages/goal/tool-goal/src/index.ts:182`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/src/index.ts#L182) | `function present(title: string, kind: 'read' \| 'other', rawInput?: unknown): GenericCallView {` |
| `insert` | `const` | [`packages/session-query/session-query-sqlite/src/index.ts:584`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/src/index.ts#L584) | `const insert = db.prepare(\`` |
| `stat` | `const` | [`packages/shell/pwsh-local/src/resolve.ts:48`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/resolve.ts#L48) | `const stat = lstatSync(candidate)` |
| `absent` | `const` | [`scripts/verify-translation-pairing.ts:117`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-translation-pairing.ts#L117) | `const absent = request.anchors.filter((anchor) => {` |

### Tests and executable evidence

- [`packages/fs/fs/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/tests/service.spec.ts) — A test under the owning area exercises or imports `FS_NOT_FOUND`. A test under the owning area exercises or imports `dsh-fs-observation-policy`.
- [`packages/fs/fs-local/tests/fsio.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/tests/fsio.spec.ts) — A test under the owning area exercises or imports `FS_NOT_FOUND`. A test under the owning area exercises or imports `dsh-fs-observation-policy`.
- [`packages/e2b/fs-e2b/tests/filesystem.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/tests/filesystem.spec.ts) — A test under the owning area exercises or imports `FS_NOT_FOUND`. A test under the owning area exercises or imports `replaceIfVersion`.
- [`packages/fs/fs-local/tests/filesystem.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/tests/filesystem.spec.ts) — A test under the owning area exercises or imports `FS_NOT_FOUND`. A test under the owning area exercises or imports `replaceIfVersion`.
- [`packages/fs/fs-observation-policy/tests/policy.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-observation-policy/tests/policy.spec.ts) — A test under the owning area exercises or imports `FS_NOT_FOUND`. A test under the owning area exercises or imports `replaceIfVersion`.

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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`
- Aliases: `view`, `present`, `insert`, `stat`, `absent`, `FS_NOT_FOUND`, `fs/observed`, `replaceIfVersion`, `createIfAbsent`, `dsh-fs`, `{ kind: 'present', version: FsVersion } | { kind: 'absent' }`, `str_replace_editor`, `str_replace`, `dsh-fs-observation-policy`
- Regex: `(?i)(view|present|insert|stat|absent|FS_NOT_FOUND|fs/observed|replaceIfVersion)`

```bash
rg -n --pcre2 "(?i)(view|present|insert|stat|absent|FS_NOT_FOUND|fs/observed|replaceIfVersion)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0019. Make `dsh-fs-observation-policy` an event-gate plugin, not a method interface](0019-make-dsh-fs-observation-policy-an-event-gate-plugin-not-a-method-interfa.md): The source note links to this decision directly.
- **`source-link`** — [0246. Guarded-mutation errors append the recovery instruction at the model boundary](0246-guarded-mutation-errors-append-the-recovery-instruction-at-the-model-bou.md): The source note links to this decision directly.
- **`shares-code-with`** — [0132. Filesystem tool schemas --- model-facing read/write/edit shapes](0132-filesystem-tool-schemas-model-facing-read-write-edit-shapes.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs-local/src/invariant.ts`.
- **`shares-code-with`** — [0451. Split the filesystem seam --- provider text mutations plus the `dsh-fs-observation-policy` plugin](0451-split-the-filesystem-seam-provider-text-mutations-plus-the-dsh-fs-observ.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs-local/src/invariant.ts`.
- **`shares-code-with`** — [0010. Filesystem capability seam --- ctx.fs, local backend, and model-facing filesystem tools](0010-filesystem-capability-seam-ctx-fs-local-backend-and-model-facing-filesys.md): Shares source implementation: `packages/fs/fs/src/index.ts`, `packages/fs/fs/src/invariant.ts`.
- **`shares-code-with`** — [0658. Prune write-only fields and a dead routing knob from the fs seam](0658-prune-write-only-fields-and-a-dead-routing-knob-from-the-fs-seam.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs/src/index.ts`.
- **`shares-code-with`** — [0046. Meaningful package invariant contracts](0046-meaningful-package-invariant-contracts.md): Shares source implementation: `packages/fs/fs/src/index.ts`, `packages/fs/fs/src/invariant.ts`.
- **`shares-code-with`** — [0549. Add direct directory listing to the filesystem seam](0549-add-direct-directory-listing-to-the-filesystem-seam.md): Shares source implementation: `packages/e2b/fs-e2b/src/index.ts`, `packages/fs/fs/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0356-filesystem-absence-is-an-observation-and-guarded-creation-never-replaces.md`.
