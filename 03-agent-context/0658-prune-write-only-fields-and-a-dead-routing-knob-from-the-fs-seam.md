---
id: "dsh-note-0658"
title: "Prune write-only fields and a dead routing knob from the fs seam"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-07-04-prune-write-only-fs-surface.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/simplification"
  - "concern/trust"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/security"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "targetKey"
  - "version"
  - "size"
  - "FsIoInternals"
  - "displayPath"
  - "readWholeText"
  - "replacements"
  - "FsTarget"
  - "FsWriteOutcome"
  - "FsEditRequest"
  - "FsEditOutcome"
  - "formatEditOutput"
  - "FileReadOutcome"
  - "formatReadOutput"
search_regex: "(?i)(targetKey|version|size|FsIoInternals|displayPath|readWholeText|replacements|FsTarget)"
---

# 0658. Prune write-only fields and a dead routing knob from the fs seam — implementation context

## Open this when

The fs seam split moved read routing and policy out of the backend into dsh-tool-fs and dsh-fs-policy. Four pieces of surface kept the pre-split shape --- populated on every call, read by nobody: STREAM_MIN_SIZE + FsIoInternals.streamMinSize in dsh-fs-local --- removed ahead of this change by the no-hardcoded-tunables audit, which made the routing bound dsh-tool-fs's readStreamMinSize config; recorded here as part of the full prune. Originally (packages/fs/fs-local/src/fsio.ts, re-exported from packages/fs/fs-local/src/index.ts): zero readers anywhere, including fs-local's own source and tests.

## Source decision

Delete the fs-local constant, its re-export, and the streamMinSize knob (the remaining FsIoInternals knobs are genuinely used by the atomic-write tests); drop inputPath from FsTarget; shrink FsEditOutcome to { version, before, after } and pass replaceAll to formatEditOutput from the parsed args; drop limit/version from FileReadOutcome. The filesystem.md pastes, packages/fs/fs/README.md, and the test fakes that had to fabricate the removed fields shrink with the types.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-07-04-prune-write-only-fs-surface.md](../02-notes/archived/simplification/2026-07-04-prune-write-only-fs-surface.md)
- Pinned source: [.agents/notes/archived/simplification/2026-07-04-prune-write-only-fs-surface.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-07-04-prune-write-only-fs-surface.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/fs/fs/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/fs/fs`. | `named-file, named-package-member` |
| [`packages/fs/fs/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts) | public types and contract | The source note names this file directly. Core file in the package named by the note: `packages/fs/fs`. | `named-file, named-package-member, symbol-definition` |
| [`packages/fs/tool-fs/src/edit.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/edit.ts) | runtime implementation | The source note names this file directly. Core file in the package named by the note: `packages/fs/tool-fs`. | `named-file, named-package-member, symbol-definition` |
| [`packages/fs/tool-fs/src/read.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts) | runtime implementation | The source note names this file directly. Core file in the package named by the note: `packages/fs/tool-fs`. | `named-file, named-package-member, symbol-definition` |
| [`packages/fs/fs-local/src/fsio.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts) | runtime implementation | The source note names this file directly. Core file in the package named by the note: `packages/fs/fs-local`. | `named-file, named-package-member, symbol-definition` |
| [`packages/fs/fs-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts) | package entry point | The source note names this file directly. Core file in the package named by the note: `packages/fs/fs-local`. | `named-file, named-package-member` |
| [`packages/fs/tool-fs/src/read-render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read-render.ts) | runtime implementation | The source note names this file directly. Core file in the package named by the note: `packages/fs/tool-fs`. | `named-file, named-package-member, symbol-definition` |
| [`packages/fs/fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs`. | `named-package-member` |
| [`packages/fs/fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs`. | `named-package-member` |
| [`packages/fs/tool-fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`packages/fs/tool-fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`packages/fs/fs-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `targetKey` | `const` | [`packages/e2b/fs-e2b/src/index.ts:182`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L182) | `const targetKey = await this.canonicalPath(sandbox, displayPath, opts?.signal)` |
| `version` | `const` | [`packages/e2b/fs-e2b/src/index.ts:389`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L389) | `const version = await this.writeAtomic(` |
| `size` | `const` | [`packages/e2b/subprocess-e2b/src/process.ts:673`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/process.ts#L673) | `const size = (reader as E2BOutputReader).size` |
| `FsIoInternals` | `interface` | [`packages/fs/fs-local/src/fsio.ts:82`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L82) | `export interface FsIoInternals {` |
| `displayPath` | `const` | [`packages/fs/fs-local/src/fsio.ts:148`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L148) | `const displayPath = resolve(cwd, path)` |
| `readWholeText` | `function` | [`packages/fs/fs-local/src/fsio.ts:375`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L375) | `export async function readWholeText(target: LocalTarget, signal?: AbortSignal): Promise<string> {` |
| `replacements` | `const` | [`packages/fs/fs-local/src/fsio.ts:771`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L771) | `const replacements = countOccurrences(content, oldNorm)` |
| `FsTarget` | `interface` | [`packages/fs/fs/src/types.ts:60`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts#L60) | `export interface FsTarget {` |
| `FsWriteOutcome` | `interface` | [`packages/fs/fs/src/types.ts:128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts#L128) | `export interface FsWriteOutcome {` |
| `FsEditRequest` | `interface` | [`packages/fs/fs/src/types.ts:147`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts#L147) | `export interface FsEditRequest {` |
| `FsEditOutcome` | `interface` | [`packages/fs/fs/src/types.ts:157`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts#L157) | `export interface FsEditOutcome {` |
| `formatEditOutput` | `function` | [`packages/fs/tool-fs/src/edit.ts:65`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/edit.ts#L65) | `export function formatEditOutput(displayPath: string, replaceAll: boolean): string {` |
| `FileReadOutcome` | `interface` | [`packages/fs/tool-fs/src/read-render.ts:47`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read-render.ts#L47) | `export interface FileReadOutcome {` |
| `formatReadOutput` | `function` | [`packages/fs/tool-fs/src/read-render.ts:152`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read-render.ts#L152) | `export function formatReadOutput(displayPath: string, outcome: FileReadOutcome): string {` |
| `STREAM_MIN_SIZE` | `const` | [`packages/fs/tool-fs/src/read.ts:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts#L22) | `export const STREAM_MIN_SIZE = 10 * 1024 * 1024` |
| `offset` | `const` | [`packages/fs/tool-fs/src/read.ts:58`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts#L58) | `const offset = args.offset === undefined ? 1 : parsePositiveInteger(args.offset, 'offset')` |

### Tests and executable evidence

- [`packages/fs/fs/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/tests/service.spec.ts) — A test under the owning area exercises or imports `targetKey`. A test under the owning area exercises or imports `displayPath`.
- [`packages/fs/fs/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/tests/invariant.spec.ts) — A test under the owning area exercises or imports `targetKey`. A test under the owning area exercises or imports `displayPath`.
- [`packages/fs/tool-fs/tests/diff.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/diff.spec.ts) — A test under the owning area exercises or imports `replace_all`.
- [`packages/fs/tool-fs/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/tools.spec.ts) — A test under the owning area exercises or imports `STREAM_MIN_SIZE`. A test under the owning area exercises or imports `readStreamMinSize`.
- [`packages/fs/fs-local/tests/fsio.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/tests/fsio.spec.ts) — A test under the owning area exercises or imports `readWholeText`. A test under the owning area exercises or imports `streamWholeText`.
- [`packages/fs/tool-fs/tests/error.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/error.spec.ts) — A test under the owning area exercises or imports `FS_EDIT_NOT_FOUND`. A test under the owning area exercises or imports `dsh-fs`.
- [`packages/e2b/fs-e2b/tests/filesystem.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/tests/filesystem.spec.ts) — A test under the owning area exercises or imports `targetKey`.
- [`packages/fs/tool-fs/tests/read-image.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/read-image.spec.ts) — A test under the owning area exercises or imports `displayPath`.
- Source verification intent: The removed surfaces are gone --- STREAM_MIN_SIZE/streamMinSize in dsh-fs-local, FsTarget.inputPath, FsEditOutcome.replacements/.replaceAll, and FileReadOutcome.limit/.version --- while the request-side replaceAll (FsEditRequest) and the version fields on the other outcome types are untouched; the test fakes shrank with the types. formatEditOutput's emitted text is unchanged for both replace_all branches, so no snapshot expected output churned.

## How to read the implementation

1. Start with [`packages/fs/fs/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/README.md) because it has the strongest evidence link to the note.
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
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/simplification`, `concern/trust`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/security`, `domain/storage`, `domain/testing`, `lifecycle/archived`, `mechanism/policy`, `mechanism/projection`
- Aliases: `targetKey`, `version`, `size`, `FsIoInternals`, `displayPath`, `readWholeText`, `replacements`, `FsTarget`, `FsWriteOutcome`, `FsEditRequest`, `FsEditOutcome`, `formatEditOutput`, `FileReadOutcome`, `formatReadOutput`
- Regex: `(?i)(targetKey|version|size|FsIoInternals|displayPath|readWholeText|replacements|FsTarget)`

```bash
rg -n --pcre2 "(?i)(targetKey|version|size|FsIoInternals|displayPath|readWholeText|replacements|FsTarget)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0019. Make `dsh-fs-observation-policy` an event-gate plugin, not a method interface](0019-make-dsh-fs-observation-policy-an-event-gate-plugin-not-a-method-interfa.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs/src/index.ts`.
- **`shares-code-with`** — [0451. Split the filesystem seam --- provider text mutations plus the `dsh-fs-observation-policy` plugin](0451-split-the-filesystem-seam-provider-text-mutations-plus-the-dsh-fs-observ.md): Shares source implementation: `packages/fs/fs-local/src/fsio.ts`, `packages/fs/fs-local/src/index.ts`.
- **`shares-code-with`** — [0132. Filesystem tool schemas --- model-facing read/write/edit shapes](0132-filesystem-tool-schemas-model-facing-read-write-edit-shapes.md): Shares source implementation: `packages/fs/fs-local/src/fsio.ts`, `packages/fs/fs-local/src/index.ts`.
- **`shares-code-with`** — [0528. Prune dead public API and result fields](0528-prune-dead-public-api-and-result-fields.md): Shares source implementation: `packages/fs/fs-local/src/fsio.ts`, `packages/fs/fs-local/src/index.ts`.
- **`shares-code-with`** — [0010. Filesystem capability seam --- ctx.fs, local backend, and model-facing filesystem tools](0010-filesystem-capability-seam-ctx-fs-local-backend-and-model-facing-filesys.md): Shares source implementation: `packages/fs/fs-local/src/fsio.ts`, `packages/fs/fs/src/index.ts`.
- **`shares-code-with`** — [0246. Guarded-mutation errors append the recovery instruction at the model boundary](0246-guarded-mutation-errors-append-the-recovery-instruction-at-the-model-bou.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs/src/index.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/fs/fs-local/src/fsio.ts`, `packages/fs/fs-local/src/index.ts`.
- **`shares-code-with`** — [0548. Result-time applied-hunk diffs for file mutations](0548-result-time-applied-hunk-diffs-for-file-mutations.md): Shares source implementation: `packages/fs/fs/src/index.ts`, `packages/fs/fs/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0658-prune-write-only-fields-and-a-dead-routing-knob-from-the-fs-seam.md`.
