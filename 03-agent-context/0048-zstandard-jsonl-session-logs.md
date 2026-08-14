---
id: "dsh-note-0048"
title: "Zstandard JSONL session logs"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-19-zstandard-jsonl-session-logs.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/streaming"
aliases:
  - "SESSION_FORMAT_VERSION"
  - "SessionEvent"
  - "MoveFileExW"
  - "jsonl"
  - "assistant/chunk"
  - "dsh-session-persistence-jsonl"
  - ".jsonl.zstd"
  - ".jsonl"
  - "SessionLocation.kind"
  - "zstdCompress"
  - "zstdDecompress"
  - "ZSTD_c_checksumFlag"
  - "finishFlush: ZSTD_e_flush"
  - "persistenceCompression"
search_regex: "(?i)(SESSION_FORMAT_VERSION|SessionEvent|MoveFileExW|jsonl|assistant/chunk|dsh\\-session\\-persistence\\-jsonl|\\.jsonl\\.zstd|\\.jsonl)"
---

# 0048. Zstandard JSONL session logs — implementation context

## Open this when

The JSONL persistence backend keeps every SessionEvent verbatim, including high-volume assistant/chunk records. Raw text makes logs inspectable but spends storage and I/O on repeated JSON keys and model text. Compression must retain the existing append/fsync commit boundary, collision-safe first materialization, crash repair, and metadata-only listing; rewriting a whole compressed file after every turn would discard those properties. The encoding also has to remain explicit at the deployment boundary.

## Source decision

dsh-session-persistence-jsonl accepts compression?: 'zstd' | 'none' and explicitly resolves omission to 'zstd'. Zstandard artifacts end in .jsonl.zstd; 'none' retains the original newline-delimited UTF-8 .jsonl representation. SessionLocation.kind remains 'jsonl', because both encodings carry the same logical record format, and SESSION_FORMAT_VERSION remains 0 under the repository's pre-release reject-without-migration policy. Each persistence root belongs to one encoding.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-19-zstandard-jsonl-session-logs.md](../02-notes/implemented/architecture/2026-07-19-zstandard-jsonl-session-logs.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-19-zstandard-jsonl-session-logs.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-19-zstandard-jsonl-session-logs.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/session/session-persistence-jsonl/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session/session-persistence-jsonl`. | `named-package-member` |
| [`packages/session/session-persistence-jsonl/src/win32.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/win32.ts) | runtime implementation | Core file in the package named by the note: `packages/session/session-persistence-jsonl`. Defines `MoveFileExW`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session/session-persistence-jsonl/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-persistence-jsonl`. | `named-package-member` |
| [`packages/session/session-persistence-jsonl`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl) | package or module directory | The note names this package or capability. | `named-package` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Defines `jsonl`, a construct named by the note. Contains the exact code literal `assistant/chunk` named by the note. | `exact-code-occurrence, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Defines `SessionEvent`, a construct named by the note. Defines `SESSION_FORMAT_VERSION`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-persistence-jsonl/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/README.md) | package contract and examples | Core file in the package named by the note: `packages/session/session-persistence-jsonl`. | `named-package-member` |
| [`packages/session/session-persistence-jsonl/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/package.json) | composition and configuration | Core file in the package named by the note: `packages/session/session-persistence-jsonl`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-session-persistence-jsonl` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `assistant/chunk` named by the note. | `exact-code-occurrence` |
| [`examples/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/package.json) | composition and configuration | Contains the exact code literal `dsh-session-persistence-jsonl` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) | package contract and examples | Contains the exact code literal `assistant/chunk` named by the note. Contains the exact code literal `dsh-session-persistence-jsonl` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `SESSION_FORMAT_VERSION` | `const` | [`packages/core/session/src/types.ts:56`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L56) | `export const SESSION_FORMAT_VERSION = 0` |
| `SessionEvent` | `type` | [`packages/core/session/src/types.ts:404`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L404) | `export type SessionEvent<T extends SessionEventType = SessionEventType> = {` |
| `MoveFileExW` | `type` | [`packages/session/session-persistence-jsonl/src/win32.ts:17`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/win32.ts#L17) | `type MoveFileExW = (existing: string, replacement: string, flags: number) => number` |
| `jsonl` | `const` | [`scripts/gen-doc-graphs.ts:731`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts#L731) | `const jsonl = nodeId('bundle', 'jsonl')` |

### Tests and executable evidence

- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `SESSION_FORMAT_VERSION`.
- [`packages/session/session-persistence-jsonl/tests/zstd.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/tests/zstd.spec.ts) — A test under the owning area exercises or imports `zstd`. A test under the owning area exercises or imports `fsync`.
- [`packages/session/session-persistence-jsonl/tests/jsonl.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/tests/jsonl.spec.ts) — A test under the owning area exercises or imports `zstd`. A test under the owning area exercises or imports `fsync`.
- [`packages/session/session-persistence-jsonl/tests/win32.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/tests/win32.spec.ts) — A test under the owning area exercises or imports `MoveFileExW`.
- [`packages/session/session-persistence-jsonl/tests/zstd.compat.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/tests/zstd.compat.spec.ts) — A test under the owning area exercises or imports `zstd`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — Contains the exact code literal `dsh-session-persistence-jsonl` named by the note.
- [`scripts/session-fixture-layout.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/session-fixture-layout.spec.ts) — Contains the exact code literal `assistant/chunk` named by the note.

## How to read the implementation

1. Start with [`packages/session/session-persistence-jsonl/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/streaming`
- Aliases: `SESSION_FORMAT_VERSION`, `SessionEvent`, `MoveFileExW`, `jsonl`, `assistant/chunk`, `dsh-session-persistence-jsonl`, `.jsonl.zstd`, `.jsonl`, `SessionLocation.kind`, `zstdCompress`, `zstdDecompress`, `ZSTD_c_checksumFlag`, `finishFlush: ZSTD_e_flush`, `persistenceCompression`
- Regex: `(?i)(SESSION_FORMAT_VERSION|SessionEvent|MoveFileExW|jsonl|assistant/chunk|dsh\-session\-persistence\-jsonl|\.jsonl\.zstd|\.jsonl)`

```bash
rg -n --pcre2 "(?i)(SESSION_FORMAT_VERSION|SessionEvent|MoveFileExW|jsonl|assistant/chunk|dsh\\-session\\-persistence\\-jsonl|\\.jsonl\\.zstd|\\.jsonl)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0100. Large-session JSONL restore pipeline](0100-large-session-jsonl-restore-pipeline.md): The source note links to this decision directly.
- **`shares-code-with`** — [0027. Windows-native durable JSONL publication](0027-windows-native-durable-jsonl-publication.md): Shares source implementation: `packages/session/session-persistence-jsonl`, `packages/session/session-persistence-jsonl/README.md`.
- **`shares-code-with`** — [0065. Make packed chunk rows the default JSONL layout](0065-make-packed-chunk-rows-the-default-jsonl-layout.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/session/session-persistence-jsonl/src/index.ts`.
- **`shares-code-with`** — [0009. Session persistence as an abstract service over the existing `SessionEvent`](0009-session-persistence-as-an-abstract-service-over-the-existing-sessioneven.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/session/session-persistence-jsonl/src/index.ts`.
- **`shares-code-with`** — [0013. Shared persistence write coordinator](0013-shared-persistence-write-coordinator.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/session/session-persistence-jsonl/src/index.ts`.
- **`shares-code-with`** — [0245. Win32 folder picker moves to koffi in a child process](0245-win32-folder-picker-moves-to-koffi-in-a-child-process.md): Shares source implementation: `packages/session/session-persistence-jsonl/src/index.ts`, `packages/session/session-persistence-jsonl/src/invariant.ts`.
- **`shares-code-with`** — [0110. Bounded session persistence write batching](0110-bounded-session-persistence-write-batching.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/session/session-persistence-jsonl/src/index.ts`.
- **`shares-code-with`** — [0303. Bind JSONL session identity before mutation](0303-bind-jsonl-session-identity-before-mutation.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/session/session-persistence-jsonl/tests/zstd.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0048-zstandard-jsonl-session-logs.md`.
