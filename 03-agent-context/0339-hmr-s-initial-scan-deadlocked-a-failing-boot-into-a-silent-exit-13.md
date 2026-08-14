---
id: "dsh-note-0339"
title: "HMR's initial scan deadlocked a failing boot into a silent exit 13"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-03-hmr-initial-scan-boot-deadlock.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/performance"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "hmr"
  - "boot"
  - "dsh"
  - "refresh"
  - "change"
  - "providers"
  - "add"
  - "Include.refresh"
  - "this.content"
  - "EntryGroup.update"
  - "loader.create"
  - "vendor/README.md"
  - "include/src/index.ts"
  - "internal/update"
search_regex: "(?i)(boot|refresh|change|providers|Include\\.refresh|this\\.content|EntryGroup\\.update|loader\\.create)"
---

# 0339. HMR's initial scan deadlocked a failing boot into a silent exit 13 — implementation context

## Open this when

A dsh launch whose config-tree failed validation exited 13 (unsettled top-level await) with no diagnostic at all, and left the TUI's terminal state stranded on the shell --- the exact symptom the fail-loud release fixed, reintroduced through a different mechanism after the transactional config reload. Two defects compounded: Concurrent Include applies corrupt the transactional group update. The HMR main watcher's chokidar initial scan re-announces every existing file as add.

## Source decision

Both halves are fixed in the vendored packages (logged in vendor/README.md): include/src/index.ts funnels every child-tree mutation --- initial apply, refresh, and internal/update patch re-application --- through one per-Include promise queue. The group's transactional update is not reentrant, so serialization is a correctness requirement, not a throughput choice. refresh() also reads inside the queue so its changed-content check compares against the predecessor's committed state. hmr/src/index.ts passes ignoreInitial: true to the main watcher.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-03-hmr-initial-scan-boot-deadlock.md](../02-notes/implemented/bug-fix/2026-08-03-hmr-initial-scan-boot-deadlock.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-03-hmr-initial-scan-boot-deadlock.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-03-hmr-initial-scan-boot-deadlock.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/README.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `include/src/index.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/hmr`. | `named-package-member` |
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/hmr`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `change`, a construct named by the note. | `symbol-definition` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Defines `boot`, a construct named by the note. Defines `hmr`, a construct named by the note. | `symbol-definition` |
| [`packages/lsp/lsp-stdio/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/index.ts) | package entry point | Defines `providers`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `dsh`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/emitter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/emitter.ts) | runtime implementation | Defines `add`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-agent-preset/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-agent-preset/src/client/index.ts) | package entry point | Defines `refresh`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) | package contract and examples | Core file in the package named by the note: `apps/cli`. | `named-package-member` |
| [`vendor/hmr/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/README.md) | package contract and examples | Core file in the package named by the note: `vendor/hmr`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `hmr` | `const` | [`packages/boot/app-boot/src/index.ts:237`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L237) | `const hmr = ctx.get('hmr')` |
| `boot` | `function` | [`packages/boot/app-boot/src/index.ts:757`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L757) | `export async function boot(` |
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |
| `refresh` | `const` | [`packages/client/ui-agent-preset/src/client/index.ts:77`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-agent-preset/src/client/index.ts#L77) | `const refresh = (): void => {` |
| `change` | `const` | [`packages/goal/goal/src/fold.ts:315`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L315) | `const change = decodeGoalChange(event.data)` |
| `providers` | `const` | [`packages/lsp/lsp-stdio/src/index.ts:141`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/index.ts#L141) | `const providers = await (async () => {` |
| `add` | `const` | [`packages/typert/generator/src/emitter.ts:880`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/emitter.ts#L880) | `const add = (boundary: RemoteBoundaryModel): void => {` |

### Tests and executable evidence

- [`packages/boot/app-boot/tests/hmr-config.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/hmr-config.spec.ts) — The source note names this file directly.
- [`packages/boot/app-boot/tests/config-reload.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/config-reload.spec.ts) — The source note names this file directly.
- [`apps/cli/tests/args.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/args.spec.ts) — A test under the owning area exercises or imports `providers`.
- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — A test under the owning area exercises or imports `hmr`. A test under the owning area exercises or imports `refresh`.
- [`apps/cli/tests/fixtures/invalid-provider.cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/invalid-provider.cordis.yml) — A test under the owning area exercises or imports `providers`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — Contains the exact code literal `vendor/README.md` named by the note.
- Source verification intent: The dsh invalid-provider PTY case in apps/cli/tests/tui-keyless-smoke.e2e.ts pins the end-to-end contract: exit 1, the labelled dsh: plugin tree failed to load: diagnostic naming $.providers, and the bracketed-paste reset proving the tree was disposed. Before this fix the same case observed exit 13 with no diagnostic. Reload behavior stays covered by packages/boot/app-boot/tests/config-reload.spec.ts and packages/boot/app-boot/tests/hmr-config.spec.ts.

## How to read the implementation

1. Start with [`vendor/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/README.md) because it has the strongest evidence link to the note.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/performance`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/projection`, `mechanism/registry`
- Aliases: `hmr`, `boot`, `dsh`, `refresh`, `change`, `providers`, `add`, `Include.refresh`, `this.content`, `EntryGroup.update`, `loader.create`, `vendor/README.md`, `include/src/index.ts`, `internal/update`
- Regex: `(?i)(boot|refresh|change|providers|Include\.refresh|this\.content|EntryGroup\.update|loader\.create)`

```bash
rg -n --pcre2 "(?i)(boot|refresh|change|providers|Include\\.refresh|this\\.content|EntryGroup\\.update|loader\\.create)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0301. A config hot-reload must not kill or degrade a live app](0301-a-config-hot-reload-must-not-kill-or-degrade-a-live-app.md): The source note links to this decision directly.
- **`source-link`** — [0329. fail-loud releases the terminal before exiting](0329-fail-loud-releases-the-terminal-before-exiting.md): The source note links to this decision directly.
- **`shares-code-with`** — [0567. Dedicated full-screen TUI front door](0567-dedicated-full-screen-tui-front-door.md): Shares source implementation: `apps/cli`, `packages/client/modules/src/index.ts`.
- **`shares-code-with`** — [0556. Native TypeScript source launch for dsh](0556-native-typescript-source-launch-for-dsh.md): Shares source implementation: `apps/cli`, `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0579. Product-level TUI session resume](0579-product-level-tui-session-resume.md): Shares source implementation: `apps/cli`, `packages/client/modules/src/index.ts`.
- **`shares-code-with`** — [0527. Artifact-first NPM baseline publication](0527-artifact-first-npm-baseline-publication.md): Shares source implementation: `apps/cli`, `packages/client/modules/src/index.ts`.
- **`shares-code-with`** — [0647. the installer adopts an existing checkout into the managed layout](0647-the-installer-adopts-an-existing-checkout-into-the-managed-layout.md): Shares source implementation: `apps/cli`, `packages/client/modules/src/index.ts`.
- **`shares-code-with`** — [0439. Private npm publication as three independent sequences](0439-private-npm-publication-as-three-independent-sequences.md): Shares source implementation: `vendor/README.md`, `vendor/hmr/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0339-hmr-s-initial-scan-deadlocked-a-failing-boot-into-a-silent-exit-13.md`.
