---
id: "dsh-note-0403"
title: "Solution root over two aggregate programs"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-22-tsconfig-solution-root-two-aggregates.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/protocols"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
aliases:
  - "json"
  - "build"
  - "paths"
  - "files"
  - "exports"
  - "typecheck"
  - "Context"
  - "include"
  - "tsconfig.client.json"
  - "tsconfig.json"
  - "tsconfig.build.json"
  - "packages/goal/command-goal"
  - "tsc -b tsconfig.json"
  - "tsconfig.vitest.json"
search_regex: "(?i)(json|build|paths|files|exports|typecheck|Context|include)"
---

# 0403. Solution root over two aggregate programs — implementation context

## Open this when

The GUI split introduced a second aggregate program (tsconfig.client.json, layering RFC) while the root tsconfig.json kept doubling as the host aggregate, and tsconfig.build.json remained a third, hand-maintained full emit graph. That triple bookkeeping produced four concrete asymmetries: The typecheck and build references lists drifted apart (packages/goal/command-goal was in the typecheck graph but missing from the build graph). The lefthook pre-push hook ran tsc -b tsconfig.json only, so client-side type breakage passed the local checkpoint and surfaced in CI.

## Source decision

One solution root, two check units, one shared base pair, no separate build or vitest config: The load-bearing principle: cordis Context declaration-merge collisions exist only inside a ts.Program, never in module resolution. A solution file forms no program, so referencing both aggregates from one root cannot collide the merges; vite-tsconfig-paths reads only paths and include and discards types, so one facade may span both sides.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-22-tsconfig-solution-root-two-aggregates.md](../02-notes/implemented/process/2026-07-22-tsconfig-solution-root-two-aggregates.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-22-tsconfig-solution-root-two-aggregates.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-22-tsconfig-solution-root-two-aggregates.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/development.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/development.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `api/remotes` named by the note. | `exact-code-occurrence, named-file` |
| [`scripts/ts-project.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ts-project.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`vendor/include/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/include`. | `named-package-member` |
| [`packages/client/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/client`. | `named-directory-member` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/client`. Defines `json`, a construct named by the note. | `named-directory-member, symbol-definition` |
| [`packages/goal/command-goal/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/command-goal/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/goal/command-goal`. | `named-directory-member` |
| [`packages/goal/command-goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/command-goal/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/goal/command-goal`. | `named-directory-member` |
| [`packages/goal/command-goal/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/command-goal/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/goal/command-goal`. Contains the exact code literal `packages/goal/command-goal` named by the note. | `exact-code-occurrence, named-directory-member` |
| [`packages/goal/command-goal/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/command-goal/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/goal/command-goal`. | `named-directory-member` |
| [`packages/client/ui-primitives/src/DiffBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DiffBlock.tsx) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client`. | `named-directory-member` |
| [`packages/client/ui-trajectory/src/client/layout.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client`. | `named-directory-member` |
| [`packages/client/ui-deliverables/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-deliverables/src/client/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/client`. | `named-directory-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `build` | `const` | [`packages/client/ui-slots/src/index.ts:980`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L980) | `const build = (name: string, seen: Set<string>): LiveSlotNode \| undefined => {` |
| `paths` | `const` | [`packages/shell/tool-bash/src/render.ts:82`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/render.ts#L82) | `const paths = [read.stdoutSpillPath, read.stderrSpillPath].filter((path): path is string => path !== undefined)` |
| `files` | `const` | [`packages/typert/generator/src/analyzer.ts:220`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L220) | `const files = new Map<string, ts.SourceFile \| undefined>()` |
| `exports` | `const` | [`packages/typert/generator/src/emitter.ts:558`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/emitter.ts#L558) | `const exports = this.schemas.map((model): SchemaExport => ({` |
| `typecheck` | `const` | [`scripts/run-gates.ts:287`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts#L287) | `const typecheck = flagEnabled('DSH_NODE_COMPAT_SKIP_TYPECHECK')` |
| `Context` | `interface` | [`vendor/hmr/src/index.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L16) | `interface Context {` |
| `include` | `const` | [`vendor/hmr/src/index.ts:250`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L250) | `const include = entry.subtree as Include \| undefined` |

### Tests and executable evidence

- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — Contains the exact code literal `packages/client` named by the note.

## How to read the implementation

1. Start with [`docs/development.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/development.md) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/protocols`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`
- Aliases: `json`, `build`, `paths`, `files`, `exports`, `typecheck`, `Context`, `include`, `tsconfig.client.json`, `tsconfig.json`, `tsconfig.build.json`, `packages/goal/command-goal`, `tsc -b tsconfig.json`, `tsconfig.vitest.json`
- Regex: `(?i)(json|build|paths|files|exports|typecheck|Context|include)`

```bash
rg -n --pcre2 "(?i)(json|build|paths|files|exports|typecheck|Context|include)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): The source note links to this decision directly.
- **`source-link`** — [0380. TSC-first build and one compiler ownership](0380-tsc-first-build-and-one-compiler-ownership.md): The source note links to this decision directly.
- **`source-link`** — [0427. Ordered Build for API Remotes Generated Contracts](0427-ordered-build-for-api-remotes-generated-contracts.md): The source note links to this decision directly.
- **`shares-code-with`** — [0161. Model-facing same-session goal tools](0161-model-facing-same-session-goal-tools.md): Shares source implementation: `packages/goal/command-goal/README.md`, `packages/goal/command-goal/src/index.ts`.
- **`shares-code-with`** — [0443. Face-named client test files](0443-face-named-client-test-files.md): Shares source implementation: `packages/client/README.md`, `packages/client/modules/src/index.ts`.
- **`shares-code-with`** — [0160. Human `/goal` command](0160-human-goal-command.md): Shares source implementation: `packages/goal/command-goal/src/index.ts`, `packages/goal/command-goal/src/invariant.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `vendor/include/src/index.ts`.
- **`shares-code-with`** — [0093. A session's agent is composed from a preset cordis.yml](0093-a-session-s-agent-is-composed-from-a-preset-cordis-yml.md): Shares source implementation: `vendor/include/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0403-solution-root-over-two-aggregate-programs.md`.
