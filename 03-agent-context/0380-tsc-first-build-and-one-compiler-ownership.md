---
id: "dsh-note-0380"
title: "TSC-first build and one compiler ownership"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-06-17-ts-build-config.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/projection"
aliases:
  - "json"
  - "dsh"
  - "build"
  - "types"
  - "exports"
  - "vendor"
  - "typecheck"
  - ".ts"
  - ".d.ts"
  - "packages/<group>/<pkg>"
  - "vendor/*"
  - ".js"
  - ".js.map"
  - ".d.ts.map"
search_regex: "(?i)(json|build|types|exports|vendor|typecheck|\\.d\\.ts|packages/<group>/<pkg>)"
---

# 0380. TSC-first build and one compiler ownership — implementation context

## Open this when

The current TypeScript build and typecheck setup had these issues: build used tsc to transform .ts to .d.ts files for packages under packages// and vendor/, and then used tsdown to transform .ts to bundled .js files. This made two tools do TypeScript transform. typecheck tended to validate packages, vendor source, examples, tests, and scripts through one root typecheck config. Build and typecheck use matching tsconfig boundaries and TypeScript resolution/transform behavior. Build generates .js, .d.ts, .js.map, and .d.ts.map through one compiler and config, so publish output and type validation stay consistent.

## Source decision

In-package relative imports use explicit .ts specifiers. pnpm run build orders Host lib, Client lib, and Web; each lib phase keeps tsc emission before tsdown bundling: Host tsc runs tsc -b against tsconfig.host.json, emitting per-module .js, .d.ts, .js.map, and .d.ts.map into lib/types for each package in the Host graph; Host tsdown then reads that JavaScript, produces published entries, and runs Host Typert.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-06-17-ts-build-config.md](../02-notes/implemented/process/2026-06-17-ts-build-config.md)
- Pinned source: [.agents/notes/implemented/process/2026-06-17-ts-build-config.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-06-17-ts-build-config.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/clean.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/clean.ts) | repository automation | The source note names this file directly. Contains the exact code literal `lib/types` named by the note. | `exact-code-occurrence, named-file` |
| [`scripts/verify-node-next-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-node-next-types.ts) | repository automation | The source note names this file directly. Contains the exact code literal `lib/types` named by the note. | `exact-code-occurrence, named-file` |
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The note names this package or capability. | `named-package` |
| [`scripts/run-gates.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts) | repository automation | Defines `typecheck`, a construct named by the note. | `symbol-definition` |
| [`scripts/attribute-chunk-bytes.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/attribute-chunk-bytes.mjs) | repository automation | Defines `vendor`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `json`, a construct named by the note. Defines `dsh`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-slots/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts) | package entry point | Defines `build`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/emitter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/emitter.ts) | runtime implementation | Defines `exports`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/analyzer.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts) | runtime implementation | Defines `types`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) | package contract and examples | Core file in the package named by the note: `apps/cli`. | `named-package-member` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Core file in the package named by the note: `apps/cli`. | `named-package-member` |
| [`CLAUDE.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/CLAUDE.md) | package contract and examples | Contains the exact code literal `lib/types` named by the note. Contains the exact code literal `api/remotes` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |
| `build` | `const` | [`packages/client/ui-slots/src/index.ts:980`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L980) | `const build = (name: string, seen: Set<string>): LiveSlotNode \| undefined => {` |
| `types` | `const` | [`packages/typert/generator/src/analyzer.ts:2657`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L2657) | `const types = manifest.types` |
| `exports` | `const` | [`packages/typert/generator/src/emitter.ts:558`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/emitter.ts#L558) | `const exports = this.schemas.map((model): SchemaExport => ({` |
| `vendor` | `let` | [`scripts/attribute-chunk-bytes.mjs:95`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/attribute-chunk-bytes.mjs#L95) | `let vendor = 0, ws = 0, other = 0` |
| `typecheck` | `const` | [`scripts/run-gates.ts:287`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts#L287) | `const typecheck = flagEnabled('DSH_NODE_COMPAT_SKIP_TYPECHECK')` |

### Tests and executable evidence

- [`scripts/clean.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/clean.spec.ts) — Contains the exact code literal `lib/types` named by the note.
- [`scripts/doc-typecheck-paths.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/doc-typecheck-paths.spec.ts) — Contains the exact code literal `lib/types` named by the note.

## How to read the implementation

1. Start with [`scripts/clean.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/clean.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/process`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/projection`
- Aliases: `json`, `dsh`, `build`, `types`, `exports`, `vendor`, `typecheck`, `.ts`, `.d.ts`, `packages/<group>/<pkg>`, `vendor/*`, `.js`, `.js.map`, `.d.ts.map`
- Regex: `(?i)(json|build|types|exports|vendor|typecheck|\.d\.ts|packages/<group>/<pkg>)`

```bash
rg -n --pcre2 "(?i)(json|build|types|exports|vendor|typecheck|\\.d\\.ts|packages/<group>/<pkg>)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0403. Solution root over two aggregate programs](0403-solution-root-over-two-aggregate-programs.md): The source note links to this decision directly.
- **`source-link`** — [0427. Ordered Build for API Remotes Generated Contracts](0427-ordered-build-for-api-remotes-generated-contracts.md): The source note links to this decision directly.
- **`shares-code-with`** — [0527. Artifact-first NPM baseline publication](0527-artifact-first-npm-baseline-publication.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0492. Separate source launch from repository build](0492-separate-source-launch-from-repository-build.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0579. Product-level TUI session resume](0579-product-level-tui-session-resume.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0676. Explicit-config dsh entrypoint](0676-explicit-config-dsh-entrypoint.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0604. Versioned TUI first-run welcome](0604-versioned-tui-first-run-welcome.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0379. pnpm as the package manager instead of Yarn 4](0379-pnpm-as-the-package-manager-instead-of-yarn-4.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/typert/generator/src/analyzer.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0380-tsc-first-build-and-one-compiler-ownership.md`.
