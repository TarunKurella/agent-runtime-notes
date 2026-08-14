---
id: "dsh-note-0019"
title: "Make `dsh-fs-observation-policy` an event-gate plugin, not a method interface"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-06-26-file-context-as-event-gate.md"
implementation_evidence: "medium"
target_anchor: "repository tests and release policy"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "expected"
  - "systemPrompt"
  - "exec"
  - "FsVersion"
  - "FsObservation"
  - "FsTarget"
  - "FsWriteIntent"
  - "applyEditTool"
  - "READ_MAX_LINE_LENGTH"
  - "READ_MAX_BYTES"
  - "FileTextLine"
  - "FileReadOutcome"
  - "buildWindow"
  - "formatReadOutput"
search_regex: "(?i)(expected|systemPrompt|exec|FsVersion|FsObservation|FsTarget|FsWriteIntent|applyEditTool)"
---

# 0019. Make `dsh-fs-observation-policy` an event-gate plugin, not a method interface — implementation context

## Open this when

The split-fs-seam Agent Note put ctx.fileContext between the model-facing tools and the ctx.fs provider: dsh-tool-fs injects fileContext and routes every read/write/edit through its methods. That makes fileContext in-path and mandatory. The tool cannot reach ctx.fs without it, the policy layer owns the fs I/O and the read windowing, and a deployment that does not want observed-state policy cannot simply drop the package --- dsh-tool-fs would fail to resolve ctx.fileContext. This couples three things that should be separable: What the tool does --- resolve a path, read a window, write/edit a file.

## Source decision

Invert the control flow. dsh-tool-fs becomes the executor and calls ctx.fs directly; dsh-fs-observation-policy becomes a gate + recorder plugin that participates through events, never through a method the tool calls and never by registering a ctx.fileContext service. The model is additive: bare ctx.fs performs atomic, unconstrained text I/O, while dsh-fs-observation-policy adds observed state, read-before-edit, and version guards. Removing the policy therefore leaves the tools usable but unconstrained.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-06-26-file-context-as-event-gate.md](../02-notes/implemented/architecture/2026-06-26-file-context-as-event-gate.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-06-26-file-context-as-event-gate.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-06-26-file-context-as-event-gate.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/fs/fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs`. | `named-package-member` |
| [`packages/fs/fs/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/fs/fs`. Defines `FsWriteIntent`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs`. | `named-package-member` |
| [`packages/fs/tool-fs/src/edit.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/edit.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/tool-fs`. Defines `applyEditTool`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/tool-fs/src/read.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/tool-fs`. Defines `STREAM_MIN_SIZE`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. Defines `exec`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/fs/tool-fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`packages/fs/tool-fs/src/write.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/write.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/tool-fs`. Defines `applyWriteTool`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/fs-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/core/tools/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/tools`. Defines `exec`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/tool-fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `expected` | `const` | [`packages/api/gateway/src/index.ts:594`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts#L594) | `const expected = new Set(descriptor.parameters.map(parameter => parameter.wire))` |
| `systemPrompt` | `const` | [`packages/boot/app-boot/src/index.ts:822`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L822) | `const systemPrompt = ctx.get('systemPrompt')` |
| `exec` | `const` | [`packages/core/tools/src/index.ts:1469`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1469) | `const exec = created.exec` |
| `exec` | `const` | [`packages/core/tools/src/invariant.ts:95`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts#L95) | `const exec = args[0] as ToolExecution` |
| `exec` | `const` | [`packages/core/tools/src/invariant.ts:101`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts#L101) | `const exec = args[0] as ToolExecution` |
| `exec` | `const` | [`packages/core/tools/src/invariant.ts:107`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts#L107) | `const exec = args[0] as ToolExecution` |
| `FsVersion` | `type` | [`packages/fs/fs/src/types.ts:35`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts#L35) | `export type FsVersion = Branded<'FsVersion'>` |
| `FsVersion` | `function` | [`packages/fs/fs/src/types.ts:43`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts#L43) | `export function FsVersion(v: string): FsVersion {` |
| `FsObservation` | `type` | [`packages/fs/fs/src/types.ts:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts#L52) | `export type FsObservation =` |
| `FsTarget` | `interface` | [`packages/fs/fs/src/types.ts:60`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts#L60) | `export interface FsTarget {` |
| `FsWriteIntent` | `type` | [`packages/fs/fs/src/types.ts:123`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts#L123) | `export type FsWriteIntent =` |
| `applyEditTool` | `function` | [`packages/fs/tool-fs/src/edit.ts:76`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/edit.ts#L76) | `export function applyEditTool(ctx: Context, sandbox: FsSandboxController): void {` |
| `READ_MAX_LINE_LENGTH` | `const` | [`packages/fs/tool-fs/src/read-render.ts:11`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read-render.ts#L11) | `export const READ_MAX_LINE_LENGTH = 2000` |
| `READ_MAX_BYTES` | `const` | [`packages/fs/tool-fs/src/read-render.ts:14`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read-render.ts#L14) | `export const READ_MAX_BYTES = 50 * 1024` |
| `FileTextLine` | `interface` | [`packages/fs/tool-fs/src/read-render.ts:29`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read-render.ts#L29) | `export interface FileTextLine {` |
| `FileReadOutcome` | `interface` | [`packages/fs/tool-fs/src/read-render.ts:47`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read-render.ts#L47) | `export interface FileReadOutcome {` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/fs/tool-fs/tests/harness.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/harness.ts) — A test under the owning area exercises or imports `dsh-fs-observation-policy`.
- [`packages/fs/fs/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/tests/service.spec.ts) — A test under the owning area exercises or imports `dsh-fs-observation-policy`. A test under the owning area exercises or imports `FsWriteIntent`.
- [`packages/fs/fs/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/tests/invariant.spec.ts) — A test under the owning area exercises or imports `FsTarget`. A test under the owning area exercises or imports `FsVersion`.
- [`packages/fs/tool-fs/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/tools.spec.ts) — A test under the owning area exercises or imports `dsh-fs-observation-policy`. A test under the owning area exercises or imports `createIfAbsent`.
- [`packages/fs/fs-local/tests/fsio.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/tests/fsio.spec.ts) — A test under the owning area exercises or imports `dsh-fs-observation-policy`. A test under the owning area exercises or imports `FS_NOT_OBSERVED`.
- Source verification intent: Tests pin both paths: without dsh-fs-observation-policy, the root tool plugin boots against dsh-fs-local, and read, create, overwrite, and unread edit succeed; with the policy, unread edit returns FS_NOT_OBSERVED and unread overwrite is gated by createIfAbsent. A later intent listener is not reached after the policy decides. Stale edits fail through provider CAS while the policy performs no stat; the tool budgets remain one stat for read and zero for write or edit on either path. The deletion recovery path is also assembled: stale mutation, missing reread, guarded recreation.

## How to read the implementation

1. Start with [`packages/fs/fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and release policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `expected`, `systemPrompt`, `exec`, `FsVersion`, `FsObservation`, `FsTarget`, `FsWriteIntent`, `applyEditTool`, `READ_MAX_LINE_LENGTH`, `READ_MAX_BYTES`, `FileTextLine`, `FileReadOutcome`, `buildWindow`, `formatReadOutput`
- Regex: `(?i)(expected|systemPrompt|exec|FsVersion|FsObservation|FsTarget|FsWriteIntent|applyEditTool)`

```bash
rg -n --pcre2 "(?i)(expected|systemPrompt|exec|FsVersion|FsObservation|FsTarget|FsWriteIntent|applyEditTool)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0356. Filesystem absence is an observation and guarded creation never replaces](0356-filesystem-absence-is-an-observation-and-guarded-creation-never-replaces.md): The source note links to this decision directly.
- **`source-link`** — [0451. Split the filesystem seam --- provider text mutations plus the `dsh-fs-observation-policy` plugin](0451-split-the-filesystem-seam-provider-text-mutations-plus-the-dsh-fs-observ.md): The source note links to this decision directly.
- **`shares-code-with`** — [0658. Prune write-only fields and a dead routing knob from the fs seam](0658-prune-write-only-fields-and-a-dead-routing-knob-from-the-fs-seam.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs/src/index.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/types.ts`.
- **`shares-code-with`** — [0132. Filesystem tool schemas --- model-facing read/write/edit shapes](0132-filesystem-tool-schemas-model-facing-read-write-edit-shapes.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs/src/index.ts`.
- **`shares-code-with`** — [0528. Prune dead public API and result fields](0528-prune-dead-public-api-and-result-fields.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/tool-fs/src/edit.ts`.
- **`shares-code-with`** — [0010. Filesystem capability seam --- ctx.fs, local backend, and model-facing filesystem tools](0010-filesystem-capability-seam-ctx-fs-local-backend-and-model-facing-filesys.md): Shares source implementation: `packages/fs/fs/src/index.ts`, `packages/fs/fs/src/invariant.ts`.
- **`shares-code-with`** — [0155. Cross-family file sandbox --- one policy home, a sandboxed fs provider, and fs escalation parity](0155-cross-family-file-sandbox-one-policy-home-a-sandboxed-fs-provider-and-fs.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/tool-fs/src/edit.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0019-make-dsh-fs-observation-policy-an-event-gate-plugin-not-a-method-interfa.md`.
