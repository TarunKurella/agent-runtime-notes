---
id: "dsh-note-0527"
title: "Artifact-first NPM baseline publication"
status: "proposed"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/process/2026-08-04-artifact-first-npm-baseline-publication.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/filesystem"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/proposed"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "dependencies"
  - "dsh"
  - "release"
  - "npm"
  - "bin"
  - "lib/"
  - "package.json#files"
  - "@deepseek-ai"
  - "@deepseek-ai/*"
  - "packages/*/*/package.json"
  - "apps/*/package.json"
  - "website/"
  - "<base>-<YYYYMMDDHHmmss>-<short-commit>"
  - "dev-<base>"
search_regex: "(?i)(dependencies|release|lib/|package\\.json\\#files|@deepseek\\-ai|@deepseek\\-ai/\\*|packages/\\*/\\*/package\\.json|apps/\\*/package\\.json)"
---

# 0527. Artifact-first NPM baseline publication — implementation context

## Open this when

Runnable source in the monorepo does not prove that published packages are runnable. Workspace links, TypeScript paths, tsx source loading, and residual lib/ files in the working tree can supply files or dependencies that are absent from a published tarball. Existing built-artifact tests still read lib/ directly from the working tree, so they do not verify what package.json#files selects or the layout produced by package-manager installation. An execution that succeeds in development mode can therefore be published without a required bundle chunk, declaration, configuration file, or asset.

## Source decision

The publication flow uses an immutable release bundle as its boundary. The pack phase builds every target package from one fixed Git commit, creates every tarball, validates tarball contents, and passes an installed-artifact integration test. The publish phase reads only those tarballs and their manifest and is forbidden from rebuilding or repacking. The target set contains only @deepseek-ai/ workspace packages discovered from packages///package.json and apps//package.json. The root project, website/, vendor, Python, and native workspaces are outside this NPM baseline.

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/process/2026-08-04-artifact-first-npm-baseline-publication.md](../02-notes/proposed/process/2026-08-04-artifact-first-npm-baseline-publication.md)
- Pinned source: [.agents/notes/proposed/process/2026-08-04-artifact-first-npm-baseline-publication.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/process/2026-08-04-artifact-first-npm-baseline-publication.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/cli/src/plugin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts) | runtime implementation | Core file in the package named by the note: `apps/cli`. Defines `dependencies`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The note names this package or capability. | `named-package` |
| [`scripts/publish-npm-baseline.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publish-npm-baseline.ts) | repository automation | Defines `bin`, a construct named by the note. | `symbol-definition` |
| [`scripts/gen-third-party-notices.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.ts) | repository automation | Defines `npm`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/code-mode.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts) | runtime implementation | Defines `release`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `dsh`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) | package contract and examples | Core file in the package named by the note: `apps/cli`. | `named-package-member` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Core file in the package named by the note: `apps/cli`. Contains the exact code literal `lib/bin.js` named by the note. | `exact-code-occurrence, named-package-member` |
| [`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.md) | package contract and examples | Contains the exact code literal `lib/bin.js` named by the note. | `exact-code-occurrence` |
| [`docs/testing.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.zh.md) | package contract and examples | Contains the exact code literal `lib/bin.js` named by the note. | `exact-code-occurrence` |
| [`scripts/release/families.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/release/families.ts) | repository automation | Contains the exact code literal `lib/bin.js` named by the note. | `exact-code-occurrence` |
| [`docs/cookbook/adding-a-package.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cookbook/adding-a-package.md) | package contract and examples | Contains the exact code literal `lib/bin.js` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `dependencies` | `const` | [`apps/cli/src/plugin.ts:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts#L62) | `const dependencies = Object.keys(after.dependencies ?? {})` |
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |
| `release` | `const` | [`packages/core/tools/src/code-mode.ts:382`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L382) | `const release = wake` |
| `npm` | `const` | [`scripts/gen-third-party-notices.ts:665`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.ts#L665) | `const npm = collectNpmDeps()` |
| `bin` | `const` | [`scripts/publish-npm-baseline.ts:457`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publish-npm-baseline.ts#L457) | `const bin = resolve(consumerRoot, 'node_modules/@deepseek-ai/dsh/lib/bin.js')` |

### Tests and executable evidence

- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — Contains the exact code literal `lib/bin.js` named by the note.
- [`scripts/release/families.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/release/families.spec.ts) — Contains the exact code literal `lib/bin.js` named by the note.
- Source verification intent: One pack entry discovers every target under packages// and apps/ from a fixed commit, derives and displays a version from the UTC second and short commit before waiting for Enter, generates the complete release bundle before any registry write, and prints one copyable publish command; release waits again after packing, while --yes skips both confirmations. Static manifest and tarball-content gates both reject published src and .d.ts.map while source manifests retain exports["./src/"].

## How to read the implementation

1. Start with [`apps/cli/src/plugin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/filesystem`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/proposed`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `dependencies`, `dsh`, `release`, `npm`, `bin`, `lib/`, `package.json#files`, `@deepseek-ai`, `@deepseek-ai/*`, `packages/*/*/package.json`, `apps/*/package.json`, `website/`, `<base>-<YYYYMMDDHHmmss>-<short-commit>`, `dev-<base>`
- Regex: `(?i)(dependencies|release|lib/|package\.json\#files|@deepseek\-ai|@deepseek\-ai/\*|packages/\*/\*/package\.json|apps/\*/package\.json)`

```bash
rg -n --pcre2 "(?i)(dependencies|release|lib/|package\\.json\\#files|@deepseek\\-ai|@deepseek\\-ai/\\*|packages/\\*/\\*/package\\.json|apps/\\*/package\\.json)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0579. Product-level TUI session resume](0579-product-level-tui-session-resume.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0604. Versioned TUI first-run welcome](0604-versioned-tui-first-run-welcome.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0676. Explicit-config dsh entrypoint](0676-explicit-config-dsh-entrypoint.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0402. Product-first root README](0402-product-first-root-readme.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0556. Native TypeScript source launch for dsh](0556-native-typescript-source-launch-for-dsh.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0485. Source run without a managed installer](0485-source-run-without-a-managed-installer.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0439. Private npm publication as three independent sequences](0439-private-npm-publication-as-three-independent-sequences.md): Shares source implementation: `apps/cli/src/plugin.ts`, `scripts/publish-npm-baseline.ts`.
- **`shares-code-with`** — [0473. Omit runtime invariants from shipped dsh config](0473-omit-runtime-invariants-from-shipped-dsh-config.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0527-artifact-first-npm-baseline-publication.md`.
