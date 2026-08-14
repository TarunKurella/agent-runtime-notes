---
id: "dsh-note-0443"
title: "Face-named client test files"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-12-face-named-client-test-files.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/filesystem"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
aliases:
  - "json"
  - "files"
  - "Context"
  - "include"
  - "packages/client/*/tests/"
  - "tsconfig.client.json"
  - "tsconfig.host.json"
  - "packages/client/**"
  - "packages/client/connection/tsconfig.host.json"
  - "packages/client"
  - "*.client.spec.ts"
  - "*.client.spec.tsx"
  - "*.client.ts"
  - "*.client.tsx"
search_regex: "(?i)(json|files|Context|include|packages/client/\\*/tests/|tsconfig\\.client\\.json|tsconfig\\.host\\.json|packages/client/\\*\\*)"
---

# 0443. Face-named client test files — implementation context

## Open this when

packages/client//tests/ holds tests for both compile faces. Most cover a Client package's browser half and belong to tsconfig.client.json; a few cover the Host half of a split package --- the carrier's node-half specs --- and can only type-check in tsconfig.host.json, because a Host-face spec reaching Host source needs the Host projects those files live in. Nothing in a filename said which face a test covered, so the two aggregates could not partition the directory by pattern.

## Source decision

A test file under packages/client names the face it covers: The suffixes are mutually exclusive --- neither is a suffix of the other --- so each aggregate keeps one broad test glob and excludes the other face: tsconfig.client.json includes packages/client//tests//.{ts,tsx} and excludes packages/client//tests//.host.spec.ts. tsconfig.host.json reaches the same directory through its repository-wide packages///tests//.ts and excludes packages/client//src/ plus the four .client. patterns. This rests on exclude filtering the result of include: when both match, the file stays out.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-12-face-named-client-test-files.md](../02-notes/implemented/process/2026-08-12-face-named-client-test-files.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-12-face-named-client-test-files.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-12-face-named-client-test-files.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/rescope-vendor.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/rescope-vendor.ts) | repository automation | The source note names this file directly. Contains the exact code literal `scripts/rescope-vendor.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/client/connection/tsconfig.host.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/tsconfig.host.json) | composition and configuration | The source note names this file directly. | `named-file` |
| [`vendor/include/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/include`. | `named-package-member` |
| [`packages/client/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/client`. | `named-directory-member` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/client`. Defines `json`, a construct named by the note. | `named-directory-member, symbol-definition` |
| [`packages/client/runtime/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/client`. | `named-directory-member` |
| [`packages/client/ui-layout/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/client`. | `named-directory-member` |
| [`packages/client/ui-commands/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/client`. | `named-directory-member` |
| [`packages/client/ui-trajectory/src/client/layout.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client`. | `named-directory-member` |
| [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client`. | `named-directory-member` |
| [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client`. | `named-directory-member` |
| [`packages/client`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client) | package or module directory | The source note names this implementation area directly. | `named-directory` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `files` | `const` | [`packages/typert/generator/src/analyzer.ts:220`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L220) | `const files = new Map<string, ts.SourceFile \| undefined>()` |
| `Context` | `interface` | [`vendor/hmr/src/index.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L16) | `interface Context {` |
| `include` | `const` | [`vendor/hmr/src/index.ts:250`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L250) | `const include = entry.subtree as Include \| undefined` |

### Tests and executable evidence

- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — Contains the exact code literal `packages/client` named by the note.

## How to read the implementation

1. Start with [`scripts/rescope-vendor.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/rescope-vendor.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/discovery-routing`, `concern/evidence`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/filesystem`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`
- Aliases: `json`, `files`, `Context`, `include`, `packages/client/*/tests/`, `tsconfig.client.json`, `tsconfig.host.json`, `packages/client/**`, `packages/client/connection/tsconfig.host.json`, `packages/client`, `*.client.spec.ts`, `*.client.spec.tsx`, `*.client.ts`, `*.client.tsx`
- Regex: `(?i)(json|files|Context|include|packages/client/\*/tests/|tsconfig\.client\.json|tsconfig\.host\.json|packages/client/\*\*)`

```bash
rg -n --pcre2 "(?i)(json|files|Context|include|packages/client/\\*/tests/|tsconfig\\.client\\.json|tsconfig\\.host\\.json|packages/client/\\*\\*)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0403. Solution root over two aggregate programs](0403-solution-root-over-two-aggregate-programs.md): Shares source implementation: `packages/client/README.md`, `packages/client/modules/src/index.ts`.
- **`shares-code-with`** — [0441. Python public publication workflow](0441-python-public-publication-workflow.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`.
- **`shares-code-with`** — [0411. Provision CI pnpm via pnpm/action-setup](0411-provision-ci-pnpm-via-pnpm-action-setup.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`.
- **`shares-code-with`** — [0417. Oxlint as the repository linter](0417-oxlint-as-the-repository-linter.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-layout/src/client/index.ts`.
- **`shares-code-with`** — [0377. Mechanical quality gates over prose guidelines](0377-mechanical-quality-gates-over-prose-guidelines.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`.
- **`shares-code-with`** — [0640. doc-sync through the gate scheduler](0640-doc-sync-through-the-gate-scheduler.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`.
- **`shares-code-with`** — [0424. Exact uncovered locations on coverage failure](0424-exact-uncovered-locations-on-coverage-failure.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/layout.ts`.
- **`shares-code-with`** — [0392. Package Model Experience contract](0392-package-model-experience-contract.md): Shares source implementation: `packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`, `scripts/rescope-vendor.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0443-face-named-client-test-files.md`.
