---
id: "dsh-note-0089"
title: "the code-runtime seam owns portable-identifier exclusions"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-31-code-runtime-portable-identifier-seam.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "stack"
  - "IDENTIFIER"
  - "args"
  - "RESERVED_BINDING_GLOBALS"
  - "RESERVED_ERROR_MEMBERS"
  - "DUNDER_MEMBER"
  - "PORTABLE_RESERVED_WORDS"
  - "CodeBindingErrorClass"
  - "global"
  - "dsh-code-runtime-worker-thread"
  - "set holding only ECMAScript keywords, and a"
  - "set of three JS"
  - "@deepseek-ai/dsh-code-runtime"
  - "__dsh_main__"
search_regex: "(?i)(stack|IDENTIFIER|args|RESERVED_BINDING_GLOBALS|RESERVED_ERROR_MEMBERS|DUNDER_MEMBER|PORTABLE_RESERVED_WORDS|CodeBindingErrorClass)"
---

# 0089. the code-runtime seam owns portable-identifier exclusions — implementation context

## Open this when

The code-runtime seam promises that a binding-namespace list valid on one backend is valid on every backend, so a Code Mode consumer can hand the same bindings to any registered runtime without knowing its language. The first backend, dsh-code-runtime-worker-thread, privately owned the identifier rules that enforce part of that promise: an IDENTIFIER regex that allowed the JS-only $, a RESERVED_WORDS set holding only ECMAScript keywords, and a RESERVED_ERROR_PROPERTIES set of three JS Error slots. Those rules described the worker's own language, not the seam's portability contract.

## Source decision

The Service Definition package (@deepseek-ai/dsh-code-runtime) exports the portable-identifier exclusion contract as four named constants, and every Service Provider imports them rather than re-declaring: PORTABLE_RESERVED_WORDS --- the union of ECMAScript and Python reserved words. A namespace global or error-class name matching any is refused on all backends, so lambda is refused even though it is a legal JS parameter name. Adding a language widens this union, which is a deliberate breaking review of existing binding names.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-31-code-runtime-portable-identifier-seam.md](../02-notes/implemented/architecture/2026-07-31-code-runtime-portable-identifier-seam.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-31-code-runtime-portable-identifier-seam.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-31-code-runtime-portable-identifier-seam.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/code-runtime/code-runtime/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime/src/index.ts) | package entry point | Core file in the package named by the note: `packages/code-runtime/code-runtime`. Defines `PORTABLE_RESERVED_WORDS`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/code-runtime/code-runtime/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/code-runtime/code-runtime`. Defines `CodeBindingErrorClass`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/code-runtime/code-runtime/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/code-runtime/code-runtime`. | `named-package-member` |
| [`packages/code-runtime/code-runtime-worker-thread/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/index.ts) | package entry point | Core file in the package named by the note: `packages/code-runtime/code-runtime-worker-thread`. Defines `IDENTIFIER`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/code-runtime/code-runtime-worker-thread/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/code-runtime/code-runtime-worker-thread`. | `named-package-member` |
| [`packages/code-runtime/code-runtime-worker-thread/src/bootstrap.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/bootstrap.ts) | runtime implementation | Core file in the package named by the note: `packages/code-runtime/code-runtime-worker-thread`. | `named-package-member` |
| [`packages/code-runtime/code-runtime`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/code-runtime/code-runtime-worker-thread`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Defines `stack`, a construct named by the note. | `symbol-definition` |
| [`packages/storage/storage-sqlite/src/unit.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-sqlite/src/unit.ts) | runtime implementation | Defines `global`, a construct named by the note. | `symbol-definition` |
| [`packages/code-runtime/code-runtime/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime/README.md) | package contract and examples | Core file in the package named by the note: `packages/code-runtime/code-runtime`. | `named-package-member` |
| [`packages/code-runtime/code-runtime/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime/package.json) | composition and configuration | Core file in the package named by the note: `packages/code-runtime/code-runtime`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `stack` | `const` | [`packages/boot/app-boot/src/index.ts:799`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L799) | `const stack = deepest instanceof Error && deepest !== cause ? \`\n${deepest.stack ?? deepest.message}\` : ''` |
| `IDENTIFIER` | `const` | [`packages/code-runtime/code-runtime-worker-thread/src/index.ts:73`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/index.ts#L73) | `const IDENTIFIER = /^[A-Za-z_][A-Za-z0-9_]*$/` |
| `args` | `const` | [`packages/code-runtime/code-runtime-worker-thread/src/index.ts:484`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/index.ts#L484) | `const args = decodeWorkerJson(message.args)` |
| `RESERVED_BINDING_GLOBALS` | `const` | [`packages/code-runtime/code-runtime/src/index.ts:40`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime/src/index.ts#L40) | `export const RESERVED_BINDING_GLOBALS: ReadonlySet<string> = new Set([` |
| `RESERVED_ERROR_MEMBERS` | `const` | [`packages/code-runtime/code-runtime/src/index.ts:55`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime/src/index.ts#L55) | `export const RESERVED_ERROR_MEMBERS: ReadonlySet<string> = new Set([` |
| `DUNDER_MEMBER` | `const` | [`packages/code-runtime/code-runtime/src/index.ts:64`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime/src/index.ts#L64) | `export const DUNDER_MEMBER = /^__.+__$/` |
| `PORTABLE_RESERVED_WORDS` | `const` | [`packages/code-runtime/code-runtime/src/index.ts:76`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime/src/index.ts#L76) | `export const PORTABLE_RESERVED_WORDS: ReadonlySet<string> = new Set([` |
| `CodeBindingErrorClass` | `interface` | [`packages/code-runtime/code-runtime/src/types.ts:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime/src/types.ts#L30) | `export interface CodeBindingErrorClass {` |
| `global` | `let` | [`packages/storage/storage-sqlite/src/unit.ts:77`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-sqlite/src/unit.ts#L77) | `let global: unknown = null` |

### Tests and executable evidence

- [`packages/code-runtime/code-runtime/tests/reserved.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime/tests/reserved.spec.ts) — A test under the owning area exercises or imports `lambda`. A test under the owning area exercises or imports `PORTABLE_RESERVED_WORDS`.
- [`packages/code-runtime/code-runtime-worker-thread/tests/runtime.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/tests/runtime.spec.ts) — A test under the owning area exercises or imports `dsh-code-runtime-worker-thread`. A test under the owning area exercises or imports `lambda`.
- [`packages/code-runtime/code-runtime-worker-thread/tests/built-lib.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/tests/built-lib.e2e.ts) — A test under the owning area exercises or imports `dsh-code-runtime-worker-thread`.

## How to read the implementation

1. Start with [`packages/code-runtime/code-runtime/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/ownership`, `concern/schema-types`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `stack`, `IDENTIFIER`, `args`, `RESERVED_BINDING_GLOBALS`, `RESERVED_ERROR_MEMBERS`, `DUNDER_MEMBER`, `PORTABLE_RESERVED_WORDS`, `CodeBindingErrorClass`, `global`, `dsh-code-runtime-worker-thread`, `set holding only ECMAScript keywords, and a`, `set of three JS`, `@deepseek-ai/dsh-code-runtime`, `__dsh_main__`
- Regex: `(?i)(stack|IDENTIFIER|args|RESERVED_BINDING_GLOBALS|RESERVED_ERROR_MEMBERS|DUNDER_MEMBER|PORTABLE_RESERVED_WORDS|CodeBindingErrorClass)`

```bash
rg -n --pcre2 "(?i)(stack|IDENTIFIER|args|RESERVED_BINDING_GLOBALS|RESERVED_ERROR_MEMBERS|DUNDER_MEMBER|PORTABLE_RESERVED_WORDS|CodeBindingErrorClass)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0228. Code Mode language dispatch and the Python SDK renderer](0228-code-mode-language-dispatch-and-the-python-sdk-renderer.md): The source note links to this decision directly.
- **`shares-code-with`** — [0329. fail-loud releases the terminal before exiting](0329-fail-loud-releases-the-terminal-before-exiting.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0553. Parse `dsh` argv through one Commander adapter](0553-parse-dsh-argv-through-one-commander-adapter.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0075. Regroup packages/ by measured clustering](0075-regroup-packages-by-measured-clustering.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0034. Single-file executable SDK runtime distribution (single-exe)](0034-single-file-executable-sdk-runtime-distribution-single-exe.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0145. Explicit model-facing tool order](0145-explicit-model-facing-tool-order.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0546. Extract example apps into packages](0546-extract-example-apps-into-packages.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0570. dsh tells the agent where its own source lives](0570-dsh-tells-the-agent-where-its-own-source-lives.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0089-the-code-runtime-seam-owns-portable-identifier-exclusions.md`.
