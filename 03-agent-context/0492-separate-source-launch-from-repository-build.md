---
id: "dsh-note-0492"
title: "Separate source launch from repository build"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-08-12-separate-source-launch-from-build.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/simplification"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/generation"
aliases:
  - "dsh"
  - "client"
  - "node --import tsx/esm apps/cli/src/bin.ts"
  - "dsh.client"
  - "apps/cli/tests/source-launch.compat.spec.ts"
  - "packages/bundle/web-app/tests/web-app.spec.ts"
  - "packages/client/modules/tests/node-half.client.spec.ts"
  - "Separate source launch from repository build"
  - "simplification"
  - "discovery routing"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "performance"
search_regex: "(?i)(client|node[- ]\\-\\-import[- ]tsx/esm[- ]apps/cli/src/bin\\.ts|dsh\\.client|apps/cli/tests/source\\-launch\\.compat\\.spec\\.ts|packages/bundle/web\\-app/tests/web\\-app\\.spec\\.ts|packages/client/modules/tests/node\\-half\\.client\\.spec\\.ts|Separate[- ]source[- ]launch[- ]from[- ]repository[- ]build|simplification)"
---

# 0492. Separate source launch from repository build — implementation context

## Open this when

The TypeScript source launcher does not need a complete repository build for every invocation. The Web surface does need built frontend and client-plugin artifacts. Making one package script own both operations adds repository-wide build latency to repeated TUI, headless, and Web startup and obscures when browser artifacts are refreshed. Source modules reached through tsx and browser modules reached through built bundles have different freshness behavior. Separating their commands requires explicit ownership of artifact production and an accurate failure model for missing and stale output.

## Source decision

The root dsh script only runs node --import tsx/esm apps/cli/src/bin.ts. pnpm run build remains the separate operation that generates package and frontend artifacts. Source users run the build before the first production-like launch and whenever frontend or client-plugin artifacts need refreshing. Missing Typert host artifacts fail profile boot through module-resolution errors without a build instruction. Once those host artifacts exist, missing frontend and client-plugin artifacts fail at startup with diagnostics that direct the user to pnpm run build.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-08-12-separate-source-launch-from-build.md](../02-notes/implemented/simplification/2026-08-12-separate-source-launch-from-build.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-08-12-separate-source-launch-from-build.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-08-12-separate-source-launch-from-build.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/cli/src/bin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/bin.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/sdk/client/src/api.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/api.ts) | runtime implementation | Defines `client`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `dsh`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) | package contract and examples | Core file in the package named by the note: `apps/cli`. | `named-package-member` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Core file in the package named by the note: `apps/cli`. | `named-package-member` |
| [`scripts/run-gates.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts) | repository automation | Contains the exact code literal `apps/cli/tests/source-launch.compat.spec.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |
| `client` | `const` | [`packages/sdk/client/src/api.ts:148`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/api.ts#L148) | `const client = this.harness.client` |

### Tests and executable evidence

- [`apps/cli/tests/source-launch.compat.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/source-launch.compat.spec.ts) — The source note names this file directly.
- [`packages/bundle/web-app/tests/web-app.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/tests/web-app.spec.ts) — The source note names this file directly.
- [`packages/client/modules/tests/node-half.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/tests/node-half.client.spec.ts) — The source note names this file directly.
- [`packages/typert/generator/tests/fixtures/type-model/packages/client/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/client`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/client`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/client/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/client`.
- Source verification intent: apps/cli/tests/source-launch.compat.spec.ts pins the exact root package command and exercises the production source-launch vector. packages/bundle/web-app/tests/web-app.spec.ts and packages/client/modules/tests/node-half.client.spec.ts pin the missing-artifact diagnostics.

## How to read the implementation

1. Start with [`apps/cli/src/bin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/bin.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/generation`
- Aliases: `dsh`, `client`, `node --import tsx/esm apps/cli/src/bin.ts`, `dsh.client`, `apps/cli/tests/source-launch.compat.spec.ts`, `packages/bundle/web-app/tests/web-app.spec.ts`, `packages/client/modules/tests/node-half.client.spec.ts`, `Separate source launch from repository build`, `simplification`, `discovery routing`, `evidence`, `lifecycle`, `ownership`, `performance`
- Regex: `(?i)(client|node[- ]\-\-import[- ]tsx/esm[- ]apps/cli/src/bin\.ts|dsh\.client|apps/cli/tests/source\-launch\.compat\.spec\.ts|packages/bundle/web\-app/tests/web\-app\.spec\.ts|packages/client/modules/tests/node\-half\.client\.spec\.ts|Separate[- ]source[- ]launch[- ]from[- ]repository[- ]build|simplification)`

```bash
rg -n --pcre2 "(?i)(client|node[- ]\\-\\-import[- ]tsx/esm[- ]apps/cli/src/bin\\.ts|dsh\\.client|apps/cli/tests/source\\-launch\\.compat\\.spec\\.ts|packages/bundle/web\\-app/tests/web\\-app\\.spec\\.ts|packages/client/modules/tests/node\\-half\\.client\\.spec\\.ts|Separate[- ]source[- ]launch[- ]from[- ]repository[- ]build|simplification)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0074. dsh source launch through the tsx ESM hook](0074-dsh-source-launch-through-the-tsx-esm-hook.md): The source note links to this decision directly.
- **`source-link`** — [0166. The dsh CLI and personal config overlays from the Harness home](0166-the-dsh-cli-and-personal-config-overlays-from-the-harness-home.md): The source note links to this decision directly.
- **`source-link`** — [0485. Source run without a managed installer](0485-source-run-without-a-managed-installer.md): The source note links to this decision directly.
- **`shares-code-with`** — [0579. Product-level TUI session resume](0579-product-level-tui-session-resume.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0676. Explicit-config dsh entrypoint](0676-explicit-config-dsh-entrypoint.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0556. Native TypeScript source launch for dsh](0556-native-typescript-source-launch-for-dsh.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0527. Artifact-first NPM baseline publication](0527-artifact-first-npm-baseline-publication.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0604. Versioned TUI first-run welcome](0604-versioned-tui-first-run-welcome.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0492-separate-source-launch-from-repository-build.md`.
