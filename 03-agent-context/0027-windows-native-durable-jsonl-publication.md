---
id: "dsh-note-0027"
title: "Windows-native durable JSONL publication"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-05-windows-jsonl-durable-publish.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/filesystem"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
aliases:
  - "link"
  - "ReplaceFileW"
  - "MoveFileExW"
  - "koffi"
  - "SessionPersistence"
  - "materialize"
  - "dsh-session-persistence-jsonl"
  - "syncDir"
  - ".dsh-mkdir-"
  - "MOVEFILE_REPLACE_EXISTING"
  - "MOVEFILE_COPY_ALLOWED"
  - "pnpm-workspace.yaml"
  - "CreateHardLinkW"
  - "Windows-native durable JSONL publication"
search_regex: "(?i)(link|ReplaceFileW|MoveFileExW|koffi|SessionPersistence|materialize|dsh\\-session\\-persistence\\-jsonl|syncDir)"
---

# 0027. Windows-native durable JSONL publication — implementation context

## Open this when

dsh-session-persistence-jsonl publishes a session log lazily on the first append. The POSIX protocol writes a temp file, fsyncs it, links it to the final name, fsyncs the parent directory, and then removes the temp link. The parent-directory fsync is part of the durability contract: a crash after the namespace change must not lose the committed final name while leaving callers believing the session log materialized. Windows has atomic namespace operations, but Node does not expose a POSIX-equivalent parent-directory fsync contract there.

## Source decision

The JSONL backend forks inside materialize() before any namespace mutation. Shared code computes the session directory, final log path, and encoded header plus initial event batch; POSIX and Windows then run separate publication protocols. POSIX keeps the existing protocol: create the root, project directory, and session directory with parent directory fsyncs, write and fsync a temp file, publish with link() so an existing final log is never overwritten, fsync the session directory, then remove the redundant temp hard link.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-05-windows-jsonl-durable-publish.md](../02-notes/implemented/architecture/2026-07-05-windows-jsonl-durable-publish.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-05-windows-jsonl-durable-publish.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-05-windows-jsonl-durable-publish.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/session/session-persistence-jsonl/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session/session-persistence-jsonl`. | `named-package-member` |
| [`packages/session/session-persistence-jsonl/src/win32.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/win32.ts) | runtime implementation | Core file in the package named by the note: `packages/session/session-persistence-jsonl`. Defines `MoveFileExW`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session/session-persistence-jsonl/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-persistence-jsonl`. | `named-package-member` |
| [`packages/session/session-persistence-jsonl`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/fs/fs-local/src/win32.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/win32.ts) | runtime implementation | Defines `ReplaceFileW`, a construct named by the note. | `symbol-definition` |
| [`packages/boot/app-boot/src/profile.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/profile.ts) | runtime implementation | Defines `link`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-persistence/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/index.ts) | package entry point | Defines `SessionPersistence`, a construct named by the note. | `symbol-definition` |
| [`packages/workflow/workflow-worker-thread/src/realm.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow-worker-thread/src/realm.ts) | runtime implementation | Defines `materialize`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-persistence-jsonl/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/README.md) | package contract and examples | Core file in the package named by the note: `packages/session/session-persistence-jsonl`. | `named-package-member` |
| [`packages/session/session-persistence-jsonl/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/package.json) | composition and configuration | Core file in the package named by the note: `packages/session/session-persistence-jsonl`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-session-persistence-jsonl` named by the note. | `exact-code-occurrence` |
| [`examples/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/package.json) | composition and configuration | Contains the exact code literal `dsh-session-persistence-jsonl` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `link` | `const` | [`packages/boot/app-boot/src/profile.ts:251`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/profile.ts#L251) | `const link = join(modulesDir, packageName)` |
| `ReplaceFileW` | `type` | [`packages/fs/fs-local/src/win32.ts:17`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/win32.ts#L17) | `type ReplaceFileW = (` |
| `MoveFileExW` | `type` | [`packages/session/session-persistence-jsonl/src/win32.ts:17`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/win32.ts#L17) | `type MoveFileExW = (existing: string, replacement: string, flags: number) => number` |
| `koffi` | `const` | [`packages/session/session-persistence-jsonl/src/win32.ts:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/win32.ts#L44) | `const koffi = (await import('koffi')).default` |
| `SessionPersistence` | `class` | [`packages/session/session-persistence/src/index.ts:84`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/index.ts#L84) | `export abstract class SessionPersistence extends Service {` |
| `materialize` | `function` | [`packages/workflow/workflow-worker-thread/src/realm.ts:78`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow-worker-thread/src/realm.ts#L78) | `function materialize(value: unknown, path: string, seen: Set<object>): unknown {` |

### Tests and executable evidence

- [`packages/fs/fs-local/tests/win32.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/tests/win32.spec.ts) — A test under the owning area exercises or imports `ReplaceFileW`.
- [`packages/session/session-persistence/tests/contract.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/tests/contract.ts) — A test under the owning area exercises or imports `SessionPersistence`.
- [`packages/session/session-persistence-jsonl/tests/jsonl.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/tests/jsonl.spec.ts) — A test under the owning area exercises or imports `materialize`.
- [`packages/session/session-persistence-jsonl/tests/win32.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/tests/win32.spec.ts) — A test under the owning area exercises or imports `MoveFileExW`. A test under the owning area exercises or imports `koffi`.
- [`packages/session/session-persistence/tests/persistence.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/tests/persistence.spec.ts) — A test under the owning area exercises or imports `SessionPersistence`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — Contains the exact code literal `dsh-session-persistence-jsonl` named by the note.

## How to read the implementation

1. Start with [`packages/session/session-persistence-jsonl/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/evidence`, `concern/recovery`, `concern/simplification`, `domain/filesystem`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`
- Aliases: `link`, `ReplaceFileW`, `MoveFileExW`, `koffi`, `SessionPersistence`, `materialize`, `dsh-session-persistence-jsonl`, `syncDir`, `.dsh-mkdir-`, `MOVEFILE_REPLACE_EXISTING`, `MOVEFILE_COPY_ALLOWED`, `pnpm-workspace.yaml`, `CreateHardLinkW`, `Windows-native durable JSONL publication`
- Regex: `(?i)(link|ReplaceFileW|MoveFileExW|koffi|SessionPersistence|materialize|dsh\-session\-persistence\-jsonl|syncDir)`

```bash
rg -n --pcre2 "(?i)(link|ReplaceFileW|MoveFileExW|koffi|SessionPersistence|materialize|dsh\\-session\\-persistence\\-jsonl|syncDir)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0048. Zstandard JSONL session logs](0048-zstandard-jsonl-session-logs.md): The source note links to this decision directly.
- **`shares-code-with`** — [0013. Shared persistence write coordinator](0013-shared-persistence-write-coordinator.md): Shares source implementation: `packages/session/session-persistence-jsonl/src/index.ts`, `packages/session/session-persistence-jsonl/src/invariant.ts`.
- **`shares-code-with`** — [0245. Win32 folder picker moves to koffi in a child process](0245-win32-folder-picker-moves-to-koffi-in-a-child-process.md): Shares source implementation: `packages/session/session-persistence-jsonl/src/index.ts`, `packages/session/session-persistence-jsonl/src/invariant.ts`.
- **`shares-code-with`** — [0065. Make packed chunk rows the default JSONL layout](0065-make-packed-chunk-rows-the-default-jsonl-layout.md): Shares source implementation: `packages/session/session-persistence-jsonl/src/index.ts`, `packages/session/session-persistence-jsonl/src/invariant.ts`.
- **`shares-code-with`** — [0009. Session persistence as an abstract service over the existing `SessionEvent`](0009-session-persistence-as-an-abstract-service-over-the-existing-sessioneven.md): Shares source implementation: `packages/session/session-persistence-jsonl/src/index.ts`, `packages/session/session-persistence/src/index.ts`.
- **`shares-code-with`** — [0201. Cross-workspace session resume](0201-cross-workspace-session-resume.md): Shares source implementation: `packages/session/session-persistence-jsonl/src/index.ts`, `packages/session/session-persistence-jsonl/src/invariant.ts`.
- **`shares-code-with`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares source implementation: `packages/fs/fs-local/src/win32.ts`.
- **`shares-code-with`** — [0512. Storage root placement and derived-medium recovery](0512-storage-root-placement-and-derived-medium-recovery.md): Shares source implementation: `packages/session/session-persistence-jsonl/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0027-windows-native-durable-jsonl-publication.md`.
