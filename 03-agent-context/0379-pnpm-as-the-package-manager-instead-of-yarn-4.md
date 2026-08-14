---
id: "dsh-note-0379"
title: "pnpm as the package manager instead of Yarn 4"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-06-16-pnpm-over-yarn.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/performance"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "json"
  - "workspaces"
  - "packages"
  - "vendor"
  - "lefthook"
  - "node-modules"
  - "node_modules"
  - "yarn.config.cjs"
  - "@yarnpkg/types"
  - "packageManager"
  - "package.json"
  - ".yarnrc.yml"
  - "pnpm-workspace.yaml"
  - "vendor/*"
search_regex: "(?i)(json|workspaces|packages|vendor|lefthook|node\\-modules|node_modules|yarn\\.config\\.cjs)"
---

# 0379. pnpm as the package manager instead of Yarn 4 — implementation context

## Open this when

The repo shipped on Yarn 4 with the node-modules linker --- a deliberately conservative choice that behaves like npm's flat layout while giving us Yarn's workspaces and yarn constraints. It worked. But Yarn 4's Plug'n'Play heritage makes the node-modules linker the off-the-beaten-path mode, and the broader JS ecosystem --- tooling defaults, CI actions, Corepack examples, contributor familiarity --- increasingly centers on pnpm.

## Source decision

Adopt pnpm 11.7.0, pinned via the packageManager field and installed through Corepack (same mechanism Yarn used): Workspaces move from the package.json workspaces array + .yarnrc.yml to pnpm-workspace.yaml (vendor/, packages/ --- the same globs; examples/ stay non-workspace, matching the prior setup and tsdown's explicit globs). Strict symlinked linker (pnpm's default) replaces Yarn's hoisted node-modules linker.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-06-16-pnpm-over-yarn.md](../02-notes/implemented/process/2026-06-16-pnpm-over-yarn.md)
- Pinned source: [.agents/notes/implemented/process/2026-06-16-pnpm-over-yarn.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-06-16-pnpm-over-yarn.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/cordis/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `vendor/cordis`. | `named-file, named-package-member` |
| [`scripts/check-workspace-constraints.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/check-workspace-constraints.ts) | repository automation | The source note names this file directly. Contains the exact code literal `scripts/check-workspace-constraints.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`vendor/cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |
| [`vendor/cordis`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis) | package or module directory | The note names this package or capability. | `named-package` |
| [`scripts/install-lefthook.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.mjs) | repository automation | Defines `lefthook`, a construct named by the note. | `symbol-definition` |
| [`scripts/attribute-chunk-bytes.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/attribute-chunk-bytes.mjs) | repository automation | Defines `vendor`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `workspaces`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/analyzer.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts) | runtime implementation | Defines `packages`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/package.json) | composition and configuration | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |
| [`package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/package.json) | composition and configuration | Contains the exact code literal `scripts/check-workspace-constraints.ts` named by the note. | `exact-code-occurrence` |
| [`scripts/rescope-vendor.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/rescope-vendor.ts) | repository automation | Contains the exact code literal `scripts/check-workspace-constraints.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `workspaces` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1513`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1513) | `const workspaces = ctx.workspaceRegistry.list()` |
| `packages` | `const` | [`packages/typert/generator/src/analyzer.ts:385`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L385) | `const packages = new Map<string, { root: string; faces: Set<TypertFace> }>()` |
| `vendor` | `let` | [`scripts/attribute-chunk-bytes.mjs:95`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/attribute-chunk-bytes.mjs#L95) | `let vendor = 0, ws = 0, other = 0` |
| `lefthook` | `const` | [`scripts/install-lefthook.mjs:698`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.mjs#L698) | `const lefthook = join(root, 'node_modules', '.bin', isWindows ? 'lefthook.cmd' : 'lefthook')` |

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.

## How to read the implementation

1. Start with [`vendor/cordis/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/README.md) because it has the strongest evidence link to the note.
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

- Tags: `class/process`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/performance`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `json`, `workspaces`, `packages`, `vendor`, `lefthook`, `node-modules`, `node_modules`, `yarn.config.cjs`, `@yarnpkg/types`, `packageManager`, `package.json`, `.yarnrc.yml`, `pnpm-workspace.yaml`, `vendor/*`
- Regex: `(?i)(json|workspaces|packages|vendor|lefthook|node\-modules|node_modules|yarn\.config\.cjs)`

```bash
rg -n --pcre2 "(?i)(json|workspaces|packages|vendor|lefthook|node\\-modules|node_modules|yarn\\.config\\.cjs)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0631. tsdown for JS bundling instead of dumble](0631-tsdown-for-js-bundling-instead-of-dumble.md): The source note links to this decision directly.
- **`source-link`** — [0377. Mechanical quality gates over prose guidelines](0377-mechanical-quality-gates-over-prose-guidelines.md): The source note links to this decision directly.
- **`shares-code-with`** — [0444. npm access per release sequence: the vendored framework and the native packages publish publicly](0444-npm-access-per-release-sequence-the-vendored-framework-and-the-native-pa.md): Shares source implementation: `vendor/cordis`, `vendor/cordis/README.md`.
- **`shares-code-with`** — [0478. Copy-only preset authoring, and the way into a preset's files](0478-copy-only-preset-authoring-and-the-way-into-a-preset-s-files.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `vendor/cordis`.
- **`shares-code-with`** — [0380. TSC-first build and one compiler ownership](0380-tsc-first-build-and-one-compiler-ownership.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/typert/generator/src/analyzer.ts`.
- **`shares-code-with`** — [0439. Private npm publication as three independent sequences](0439-private-npm-publication-as-three-independent-sequences.md): Shares source implementation: `scripts/check-workspace-constraints.ts`, `vendor/cordis/src/index.ts`.
- **`shares-code-with`** — [0130. Multiplex concurrent ACP sessions over one connection](0130-multiplex-concurrent-acp-sessions-over-one-connection.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `scripts/install-lefthook.mjs`.
- **`shares-code-with`** — [0355. Broken presets are roster rows, not gaps](0355-broken-presets-are-roster-rows-not-gaps.md): Shares source implementation: `vendor/cordis`, `vendor/cordis/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0379-pnpm-as-the-package-manager-instead-of-yarn-4.md`.
