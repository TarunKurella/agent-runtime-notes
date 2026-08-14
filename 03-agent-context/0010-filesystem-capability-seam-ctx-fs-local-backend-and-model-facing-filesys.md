---
id: "dsh-note-0010"
title: "Filesystem capability seam --- ctx.fs, local backend, and model-facing filesystem tools"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-06-17-filesystem-capability-seam.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "systemPrompt"
  - "absolutePath"
  - "targetKey"
  - "version"
  - "size"
  - "displayPath"
  - "FileSystem"
  - "FsTarget"
  - "FsError"
  - "cwd"
  - "partial"
  - "HarnessError"
  - "stat"
  - "streamText"
search_regex: "(?i)(systemPrompt|absolutePath|targetKey|version|size|displayPath|FileSystem|FsTarget)"
---

# 0010. Filesystem capability seam --- ctx.fs, local backend, and model-facing filesystem tools — implementation context

## Open this when

The harness has a concrete bash capability seam (dsh-shell / dsh-bash-local / dsh-tool-bash), but filesystem operations were about to land as model-facing tools without an equivalent seam. If read, write, and edit directly used node:fs, the model-facing tool package would own filesystem execution policy, local path resolution, atomic write behavior, text decoding, symlink behavior, and edit semantics all at once. That couples three concerns that change independently: The filesystem contract: what operations plugins can ask for. The backend: local disk now, sandboxed/remote/project-scoped filesystem later.

## Source decision

Filesystem access is a first-class capability seam following the capability-seam Agent Note: @deepseek-ai/dsh-fs (packages/fs/fs) owns the abstract ctx.fs service, the filesystem vocabulary types, and the fs/ policy event vocabulary. @deepseek-ai/dsh-fs-local (packages/fs/fs-local) provides the first implementation, backed by the local filesystem. @deepseek-ai/dsh-tool-fs (packages/fs/tool-fs) provides the model-facing read, write, and edit tools over ctx.fs, and is the executor that dispatches the fs/ events. The Consumer package depends only on the Service Definition package, never on dsh-fs-local.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-06-17-filesystem-capability-seam.md](../02-notes/implemented/architecture/2026-06-17-filesystem-capability-seam.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-06-17-filesystem-capability-seam.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-06-17-filesystem-capability-seam.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |
| [`packages/fs/fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/fs/fs`. Core file in the package named by the note: `packages/fs/fs`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/fs/fs/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/fs/fs`. Core file in the package named by the note: `packages/fs/fs`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/error.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm`. Defines `HarnessError`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`vendor/cordis/src/registry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts) | runtime implementation | Core file in the package named by the note: `vendor/cordis`. Defines `resolve`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/fs/fs`. Core file in the package named by the note: `packages/fs/fs`. | `named-directory-member, named-package-member` |
| [`packages/fs/fs-local/src/fsio.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/fs/fs-local`. Core file in the package named by the note: `packages/fs/fs-local`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/fs/tool-fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/fs/tool-fs`. Core file in the package named by the note: `packages/fs/tool-fs`. | `named-directory-member, named-package-member` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `systemPrompt` | `const` | [`packages/boot/app-boot/src/index.ts:822`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L822) | `const systemPrompt = ctx.get('systemPrompt')` |
| `absolutePath` | `const` | [`packages/context/agent-instructions/src/files.ts:471`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/files.ts#L471) | `const absolutePath = join(dir, candidateName)` |
| `targetKey` | `const` | [`packages/e2b/fs-e2b/src/index.ts:182`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L182) | `const targetKey = await this.canonicalPath(sandbox, displayPath, opts?.signal)` |
| `version` | `const` | [`packages/e2b/fs-e2b/src/index.ts:389`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L389) | `const version = await this.writeAtomic(` |
| `size` | `const` | [`packages/e2b/subprocess-e2b/src/process.ts:673`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/process.ts#L673) | `const size = (reader as E2BOutputReader).size` |
| `displayPath` | `const` | [`packages/fs/fs-local/src/fsio.ts:148`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L148) | `const displayPath = resolve(cwd, path)` |
| `FileSystem` | `class` | [`packages/fs/fs/src/index.ts:86`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/index.ts#L86) | `export abstract class FileSystem extends Service {` |
| `FsTarget` | `interface` | [`packages/fs/fs/src/types.ts:60`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts#L60) | `export interface FsTarget {` |
| `FsError` | `class` | [`packages/fs/fs/src/types.ts:196`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts#L196) | `export class FsError extends HarnessError {` |
| `cwd` | `const` | [`packages/fs/tool-fs/src/session-cwd.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts#L24) | `const cwd = exec.agent?.session.header.cwd` |
| `cwd` | `const` | [`packages/fs/tool-fs/src/session-cwd.ts:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts#L41) | `const cwd = policyWorkspaceRoot ?? sessionCwd(exec, requestedPath)` |
| `partial` | `const` | [`packages/llm/llm/src/assembler.ts:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/assembler.ts#L62) | `const partial = this.ensure(chunk.index, chunk.type === 'text-delta' ? 'text' : 'reasoning')` |
| `partial` | `const` | [`packages/llm/llm/src/assembler.ts:68`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/assembler.ts#L68) | `const partial = this.ensure(chunk.index, 'tool-call')` |
| `partial` | `const` | [`packages/llm/llm/src/assembler.ts:76`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/assembler.ts#L76) | `const partial = this.ensure(chunk.index, chunk.block.type)` |
| `partial` | `let` | [`packages/llm/llm/src/assembler.ts:97`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/assembler.ts#L97) | `let partial = this.partials.get(index)` |
| `partial` | `const` | [`packages/llm/llm/src/assembler.ts:123`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/assembler.ts#L123) | `const partial = this.partials.get(index)` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/fs/tool-fs/tests/harness.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/harness.ts) — A test under the owning area exercises or imports `dsh-fs-observation-policy`.
- [`packages/fs/fs/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/tests/service.spec.ts) — A test under the owning area exercises or imports `dsh-fs-observation-policy`. A test under the owning area exercises or imports `FileSystem`.
- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `HarnessError`.
- [`packages/fs/fs/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/tests/invariant.spec.ts) — A test under the owning area exercises or imports `targetKey`. A test under the owning area exercises or imports `displayPath`.
- [`packages/fs/tool-fs/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/tools.spec.ts) — A test under the owning area exercises or imports `dsh-fs-observation-policy`. A test under the owning area exercises or imports `FileSystem`.
- Source verification intent: Tests follow the package boundary, not only the user-visible tools: the service contract in dsh-fs; real filesystem behavior through the ctx.fs interface in dsh-fs-local (resolution, symlinks, streaming, binary/UTF-8 rejection, unconditional and version-guarded writes, literal-edit semantics, line-ending preservation, structured FsError codes); the consumer surface in dsh-tool-fs against the real local provider (mock only the model/clock, never the collaborator); and integration through ctx.tools.execute() with and without dsh-fs-observation-policy, world-verified by reading files back from disk rather than.

## How to read the implementation

1. Start with [`vendor/cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/registry`
- Aliases: `systemPrompt`, `absolutePath`, `targetKey`, `version`, `size`, `displayPath`, `FileSystem`, `FsTarget`, `FsError`, `cwd`, `partial`, `HarnessError`, `stat`, `streamText`
- Regex: `(?i)(systemPrompt|absolutePath|targetKey|version|size|displayPath|FileSystem|FsTarget)`

```bash
rg -n --pcre2 "(?i)(systemPrompt|absolutePath|targetKey|version|size|displayPath|FileSystem|FsTarget)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0549. Add direct directory listing to the filesystem seam](0549-add-direct-directory-listing-to-the-filesystem-seam.md): The source note links to this decision directly.
- **`source-link`** — [0007. Capability seams --- Service Definition / Service Provider / Consumer roles](0007-capability-seams-service-definition-service-provider-consumer-roles.md): The source note links to this decision directly.
- **`source-link`** — [0019. Make `dsh-fs-observation-policy` an event-gate plugin, not a method interface](0019-make-dsh-fs-observation-policy-an-event-gate-plugin-not-a-method-interfa.md): The source note links to this decision directly.
- **`source-link`** — [0072. Portable consumers over filesystem and subprocess execution worlds](0072-portable-consumers-over-filesystem-and-subprocess-execution-worlds.md): The source note links to this decision directly.
- **`shares-code-with`** — [0046. Meaningful package invariant contracts](0046-meaningful-package-invariant-contracts.md): Shares source implementation: `packages/fs/fs/src/index.ts`, `packages/fs/fs/src/invariant.ts`.
- **`shares-code-with`** — [0451. Split the filesystem seam --- provider text mutations plus the `dsh-fs-observation-policy` plugin](0451-split-the-filesystem-seam-provider-text-mutations-plus-the-dsh-fs-observ.md): Shares source implementation: `packages/fs/fs-local/src/fsio.ts`, `packages/fs/fs/src/index.ts`.
- **`shares-code-with`** — [0018. Web capability seam - stable tools over multiple providers](0018-web-capability-seam-stable-tools-over-multiple-providers.md): Shares source implementation: `packages/fs/tool-fs/src/index.ts`, `packages/llm/llm/src/error.ts`.
- **`shares-code-with`** — [0132. Filesystem tool schemas --- model-facing read/write/edit shapes](0132-filesystem-tool-schemas-model-facing-read-write-edit-shapes.md): Shares source implementation: `packages/fs/fs-local/src/fsio.ts`, `packages/fs/fs/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0010-filesystem-capability-seam-ctx-fs-local-backend-and-model-facing-filesys.md`.
