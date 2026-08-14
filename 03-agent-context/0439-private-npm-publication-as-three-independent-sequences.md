---
id: "dsh-note-0439"
title: "Private npm publication as three independent sequences"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-10-npm-release-sequences.md"
implementation_evidence: "high"
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
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/protocols"
  - "domain/security"
  - "domain/shell-terminal"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "dependencies"
  - "publishOrder"
  - "json"
  - "publish"
  - "directory"
  - "patch"
  - "latest"
  - "files"
  - "integrity"
  - "ReleaseMember"
  - "ReleaseFamily"
  - "tarball"
  - "family"
  - "major"
search_regex: "(?i)(dependencies|publishOrder|json|publish|directory|patch|latest|files)"
---

# 0439. Private npm publication as three independent sequences — implementation context

## Open this when

This repository held three unrelated groups of publishable packages and no channel that sent any of them to a registry. packages// and apps/ form the runtime surface of @deepseek-ai/dsh; vendor/ holds nine rescoped Cordis framework packages, each carrying its upstream version; native/landlock-run/packages/ holds Linux platform packages with their own workflow. The three differ in version baseline, change rate, and build requirements: dsh moves with the product, vendor moves only when upstream is re-synced or a local modification changes, and native needs a musl toolchain and one build per architecture.

## Source decision

packages/, vendor/, and native/ each have one bump sequence and one publication, sharing no version, no trigger, and no waiting. Releasing dsh does not republish vendor; releasing vendor does not republish native. All three publish to the @deepseek-ai scope on npmjs.com, and access is per sequence rather than per scope: the vendored framework and the native packages are public, the dsh family is restricted (rationale). No publish path passes --access, because one flag cannot serve sequences that disagree and would override the manifest that owns the level.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-10-npm-release-sequences.md](../02-notes/implemented/process/2026-08-10-npm-release-sequences.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-10-npm-release-sequences.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-10-npm-release-sequences.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`scripts/publish-npm-baseline.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publish-npm-baseline.ts) | repository automation | The source note names this file directly. Defines `integrity`, a construct named by the note. | `exact-code-occurrence, named-file, symbol-definition` |
| [`scripts/check-workspace-constraints.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/check-workspace-constraints.ts) | repository automation | The source note names this file directly. Contains the exact code literal `scripts/check-workspace-constraints.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`apps/cli/src/plugin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts) | runtime implementation | Core file in the package named by the note: `apps/cli`. Defines `dependencies`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/hmr`. Defines `dependencies`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`vendor/group/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/group/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/group`. | `named-package-member` |
| [`vendor/timer/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/timer/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/timer`. | `named-package-member` |
| [`vendor/cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `vendor/cordis`. Core file in the package named by the note: `vendor/cordis`. | `named-directory-member, named-package-member` |
| [`vendor/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/loader`. | `named-package-member` |
| [`vendor/include/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/include`. | `named-package-member` |
| [`vendor/cosmokit/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cosmokit/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/cosmokit`. | `named-package-member` |
| [`vendor/cosmokit/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cosmokit/src/types.ts) | public types and contract | Core file in the package named by the note: `vendor/cosmokit`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `dependencies` | `const` | [`apps/cli/src/plugin.ts:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts#L62) | `const dependencies = Object.keys(after.dependencies ?? {})` |
| `publishOrder` | `const` | [`native/landlock-run/scripts/pack-release.mjs:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/scripts/pack-release.mjs#L52) | `const publishOrder = [];` |
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `publish` | `const` | [`packages/extensions/ui-cordis/src/client/inventory.ts:64`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/inventory.ts#L64) | `const publish = (next: CordisInventorySnapshot): void => {` |
| `directory` | `const` | [`packages/fs/fs-local/src/fsio.ts:542`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L542) | `const directory = dirname(absolutePath)` |
| `patch` | `const` | [`packages/fs/tool-fs/src/diff.ts:34`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/diff.ts#L34) | `const patch = structuredPatch('', '', before, after, undefined, undefined, { context: DIFF_CONTEXT })` |
| `latest` | `const` | [`packages/session/session-title/src/index.ts:400`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts#L400) | `const latest = messages.at(-1)` |
| `files` | `const` | [`packages/typert/generator/src/analyzer.ts:220`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L220) | `const files = new Map<string, ts.SourceFile \| undefined>()` |
| `integrity` | `const` | [`scripts/publish-npm-baseline.ts:664`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publish-npm-baseline.ts#L664) | `const integrity = this.remoteIntegrity(pkg.name)` |
| `ReleaseMember` | `interface` | [`scripts/release/families.ts:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/release/families.ts#L23) | `export interface ReleaseMember {` |
| `ReleaseFamily` | `class` | [`scripts/release/families.ts:69`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/release/families.ts#L69) | `export abstract class ReleaseFamily {` |
| `tarball` | `const` | [`scripts/release/pack.ts:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/release/pack.ts#L31) | `const tarball = join(destination, filename)` |
| `family` | `const` | [`scripts/release/pack.ts:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/release/pack.ts#L45) | `const family = releaseFamily(values.family)` |
| `major` | `const` | [`scripts/run-gates.ts:347`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts#L347) | `const major = Number.parseInt(process.versions.node.split('.')[0] ?? '', 10)` |
| `dependencies` | `const` | [`vendor/hmr/src/index.ts:38`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L38) | `const dependencies = new Set<string>()` |
| `dependencies` | `const` | [`vendor/hmr/src/index.ts:433`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L433) | `const dependencies = [...await loadDependencies(job, this.declined)]` |

### Tests and executable evidence

- [`apps/web/tests/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/README.md) — A test under the owning area exercises or imports `access`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `patch`. A test under the owning area exercises or imports `verify`.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `patch`.
- [`apps/web/tests/default-model.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/default-model.e2e.ts) — A test under the owning area exercises or imports `patch`.
- [`apps/web/tests/schedule-after.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/schedule-after.e2e.ts) — A test under the owning area exercises or imports `latest`.
- [`apps/web/tests/remote-welcome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/remote-welcome.e2e.ts) — A test under the owning area exercises or imports `access`.
- [`apps/web/tests/models-settings.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/models-settings.e2e.ts) — A test under the owning area exercises or imports `patch`.
- [`apps/web/tests/settings-chrome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/settings-chrome.e2e.ts) — A test under the owning area exercises or imports `access`.

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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/process`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/protocols`, `domain/security`, `domain/shell-terminal`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `dependencies`, `publishOrder`, `json`, `publish`, `directory`, `patch`, `latest`, `files`, `integrity`, `ReleaseMember`, `ReleaseFamily`, `tarball`, `family`, `major`
- Regex: `(?i)(dependencies|publishOrder|json|publish|directory|patch|latest|files)`

```bash
rg -n --pcre2 "(?i)(dependencies|publishOrder|json|publish|directory|patch|latest|files)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0444. npm access per release sequence: the vendored framework and the native packages publish publicly](0444-npm-access-per-release-sequence-the-vendored-framework-and-the-native-pa.md): The source note links to this decision directly.
- **`source-link`** — [0527. Artifact-first NPM baseline publication](0527-artifact-first-npm-baseline-publication.md): The source note links to this decision directly.
- **`shares-code-with`** — [0440. Rescope vendored Cordis into @deepseek-ai](0440-rescope-vendored-cordis-into-deepseek-ai.md): Shares source implementation: `vendor/README.md`, `vendor/cordis/src/index.ts`.
- **`shares-code-with`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): Shares source implementation: `apps/cli/src/plugin.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0546. Extract example apps into packages](0546-extract-example-apps-into-packages.md): Shares source implementation: `vendor/hmr/src/index.ts`, `vendor/loader/src/index.ts`.
- **`shares-code-with`** — [0514. Cordis Host/Client Dynamic Plugin Runtime](0514-cordis-host-client-dynamic-plugin-runtime.md): Shares source implementation: `vendor/cordis/src/index.ts`, `vendor/timer/src/index.ts`.
- **`shares-code-with`** — [0093. A session's agent is composed from a preset cordis.yml](0093-a-session-s-agent-is-composed-from-a-preset-cordis-yml.md): Shares source implementation: `vendor/cordis/src/index.ts`, `vendor/include/src/index.ts`.
- **`shares-code-with`** — [0478. Copy-only preset authoring, and the way into a preset's files](0478-copy-only-preset-authoring-and-the-way-into-a-preset-s-files.md): Shares source implementation: `apps/cli/src/plugin.ts`, `vendor/cordis/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0439-private-npm-publication-as-three-independent-sequences.md`.
