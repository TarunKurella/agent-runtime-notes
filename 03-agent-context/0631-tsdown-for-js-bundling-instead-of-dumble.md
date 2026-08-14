---
id: "dsh-note-0631"
title: "tsdown for JS bundling instead of dumble"
status: "archived"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/process/2026-06-11-tsdown-over-dumble.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/evidence"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/archived"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "exports"
  - "scripts/build.ts"
  - "tsdown.config.ts"
  - "workspace: ['vendor/*', 'packages/*/*']"
  - "lib/types/index.js"
  - "outDir: 'lib"
  - "fixedExtension: false"
  - ".js"
  - "lib/types"
  - "src/index.ts"
  - ".mjs"
  - ".cjs"
  - "outExtensions"
  - "tsc -b && tsdown"
search_regex: "(?i)(exports|scripts/build\\.ts|tsdown\\.config\\.ts|workspace:[- ]\\['vendor/\\*',[- ]'packages/\\*/\\*'\\]|lib/types/index\\.js|outDir:[- ]'lib|fixedExtension:[- ]false|lib/types)"
---

# 0631. tsdown for JS bundling instead of dumble — implementation context

## Open this when

The initial build used dumble, the cordiverse zero-config esbuild wrapper that upstream Cordis itself builds with --- maximum alignment with the vendored packages' conventions (it reads each package.json and infers entries/formats from the exports field). But dumble is a liability as a load-bearing tool in this repo: v0.2.x, ~530 npm downloads/week, effectively one maintainer, and we were invoking it through a custom orchestration script (scripts/build.ts) because it has no workspace mode.

## Source decision

Replace dumble with tsdown (rolldown-based, ~2.5M downloads/week, VoidZero-backed, actively released): Root tsdown.config.ts with workspace: ['vendor/', 'packages//'] (explicit globs keep bundling to vendored Cordis and the TypeScript package tree; workspace: true would also discover example manifests and non-bundled workspace members). Shared shape: entry lib/types/index.js, outDir: 'lib', ESM, platform: node, target: es2024, fixedExtension: false (keeps .js for "type": "module" packages), dts: false (tsc -b owns declarations), clean: false (lib/ also holds TSC's lib/types intermediate tree).

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/process/2026-06-11-tsdown-over-dumble.md](../02-notes/archived/process/2026-06-11-tsdown-over-dumble.md)
- Pinned source: [.agents/notes/archived/process/2026-06-11-tsdown-over-dumble.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/process/2026-06-11-tsdown-over-dumble.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`vendor/schemastery/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/schemastery/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/schemastery`. | `named-package-member` |
| [`vendor/logger-console/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/logger-console/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/logger-console`. | `named-package-member` |
| [`vendor/schemastery`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/vendor/schemastery) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/logger-console`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/vendor/logger-console) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/typert/generator/src/emitter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/emitter.ts) | runtime implementation | Defines `exports`, a construct named by the note. | `symbol-definition` |
| [`vendor/schemastery/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/schemastery/README.md) | package contract and examples | Core file in the package named by the note: `vendor/schemastery`. | `named-package-member` |
| [`vendor/logger-console/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/logger-console/README.md) | package contract and examples | Core file in the package named by the note: `vendor/logger-console`. | `named-package-member` |
| [`vendor/schemastery/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/schemastery/package.json) | composition and configuration | Core file in the package named by the note: `vendor/schemastery`. Contains the exact code literal `lib/index.mjs` named by the note. | `exact-code-occurrence, named-package-member` |
| [`vendor/logger-console/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/logger-console/package.json) | composition and configuration | Core file in the package named by the note: `vendor/logger-console`. Contains the exact code literal `lib/browser.js` named by the note. | `exact-code-occurrence, named-package-member` |
| [`CLAUDE.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/CLAUDE.md) | package contract and examples | Contains the exact code literal `lib/types` named by the note. | `exact-code-occurrence` |
| [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) | package contract and examples | Contains the exact code literal `lib/types` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `exports` | `const` | [`packages/typert/generator/src/emitter.ts:558`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/emitter.ts#L558) | `const exports = this.schemas.map((model): SchemaExport => ({` |

### Tests and executable evidence

- [`scripts/clean.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/clean.spec.ts) — Contains the exact code literal `lib/types/index.js` named by the note. Contains the exact code literal `lib/types` named by the note.
- [`scripts/publint-all.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.spec.ts) — Contains the exact code literal `lib/index.js` named by the note.
- [`scripts/release/families.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/release/families.spec.ts) — Contains the exact code literal `lib/index.js` named by the note.
- [`scripts/client-bundle-css.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-css.spec.ts) — Contains the exact code literal `lib/types/index.js` named by the note.
- [`scripts/package-invariants.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-invariants.spec.ts) — Contains the exact code literal `lib/types/index.js` named by the note. Contains the exact code literal `lib/index.js` named by the note.
- [`scripts/doc-typecheck-paths.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/doc-typecheck-paths.spec.ts) — Contains the exact code literal `lib/types` named by the note.
- [`scripts/publication-payload.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publication-payload.spec.ts) — Contains the exact code literal `src/index.ts` named by the note. Contains the exact code literal `lib/index.js` named by the note.
- [`scripts/client-bundle-purity.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-purity.spec.ts) — Contains the exact code literal `lib/types/index.js` named by the note.

## How to read the implementation

1. Start with [`vendor/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/README.md) because it has the strongest evidence link to the note.
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
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/evidence`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/testing`, `domain/web-retrieval`, `lifecycle/archived`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `exports`, `scripts/build.ts`, `tsdown.config.ts`, `workspace: ['vendor/*', 'packages/*/*']`, `lib/types/index.js`, `outDir: 'lib`, `fixedExtension: false`, `.js`, `lib/types`, `src/index.ts`, `.mjs`, `.cjs`, `outExtensions`, `tsc -b && tsdown`
- Regex: `(?i)(exports|scripts/build\.ts|tsdown\.config\.ts|workspace:[- ]\['vendor/\*',[- ]'packages/\*/\*'\]|lib/types/index\.js|outDir:[- ]'lib|fixedExtension:[- ]false|lib/types)`

```bash
rg -n --pcre2 "(?i)(exports|scripts/build\\.ts|tsdown\\.config\\.ts|workspace:[- ]\\['vendor/\\*',[- ]'packages/\\*/\\*'\\]|lib/types/index\\.js|outDir:[- ]'lib|fixedExtension:[- ]false|lib/types)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0567. Dedicated full-screen TUI front door](0567-dedicated-full-screen-tui-front-door.md): Shares source implementation: `vendor/schemastery/src/index.ts`.
- **`shares-code-with`** — [0467. One shared base config with per-surface overlays](0467-one-shared-base-config-with-per-surface-overlays.md): Shares source implementation: `vendor/README.md`.
- **`shares-code-with`** — [0231. Permission Settings default for new sessions](0231-permission-settings-default-for-new-sessions.md): Shares source implementation: `vendor/schemastery/src/index.ts`.
- **`shares-code-with`** — [0556. Native TypeScript source launch for dsh](0556-native-typescript-source-launch-for-dsh.md): Shares source implementation: `vendor/README.md`.
- **`shares-code-with`** — [0413. Dependabot version updates with a 30-day cooldown](0413-dependabot-version-updates-with-a-30-day-cooldown.md): Shares source implementation: `vendor/README.md`.
- **`shares-code-with`** — [0380. TSC-first build and one compiler ownership](0380-tsc-first-build-and-one-compiler-ownership.md): Shares source implementation: `packages/typert/generator/src/emitter.ts`.
- **`shares-code-with`** — [0339. HMR's initial scan deadlocked a failing boot into a silent exit 13](0339-hmr-s-initial-scan-deadlocked-a-failing-boot-into-a-silent-exit-13.md): Shares source implementation: `vendor/README.md`.
- **`shares-code-with`** — [0301. A config hot-reload must not kill or degrade a live app](0301-a-config-hot-reload-must-not-kill-or-degrade-a-live-app.md): Shares source implementation: `vendor/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0631-tsdown-for-js-bundling-instead-of-dumble.md`.
