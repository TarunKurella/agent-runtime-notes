---
id: "dsh-note-0329"
title: "fail-loud releases the terminal before exiting"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-31-fail-loud-releases-the-terminal.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "shutdown"
  - "FAIL_LOUD_RELEASE_TIMEOUT_MS"
  - "installFailLoud"
  - "boot"
  - "dsh"
  - "release"
  - "current"
  - "providers"
  - "reset"
  - "ui-tui"
  - "ProcessTerminal.start"
  - "ESC [ c"
  - "llm-pi-ai"
  - "process.exit"
search_regex: "(?i)(shutdown|FAIL_LOUD_RELEASE_TIMEOUT_MS|installFailLoud|boot|release|current|providers|reset)"
---

# 0329. fail-loud releases the terminal before exiting — implementation context

## Open this when

A dsh launch whose config failed validation printed its diagnostic and returned the user to a broken shell. Typing was invisible, and the next command was mangled by stray text: The Loader mounts entries concurrently, so entry failure order is not startup order. ui-tui activates and calls pi-tui's ProcessTerminal.start(), which puts stdin in raw mode, enables bracketed paste, and writes the Kitty keyboard-protocol probe --- a sequence ending in a Device Attributes query (ESC [ c). A sibling entry (here llm-pi-ai) then rejects on its own config.

## Source decision

installFailLoud takes an optional release teardown, awaited between the diagnostic and the exit: The diagnostic is written before the release, so a hanging or failing disposer cannot swallow the reason. A latch, not an uninstall, keeps the first rejection the reported one. Removing the listener during teardown would let a second concurrent rejection become uncaught, and Node would kill the process mid-teardown --- stranding exactly the terminal state this restores. Later rejections, including the release's own, fall through to the pending exit.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-31-fail-loud-releases-the-terminal.md](../02-notes/implemented/bug-fix/2026-07-31-fail-loud-releases-the-terminal.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-31-fail-loud-releases-the-terminal.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-31-fail-loud-releases-the-terminal.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/cli/src/profile-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts) | runtime implementation | Core file in the package named by the note: `apps/cli`. Defines `shutdown`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-pi-ai/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-pi-ai`. Defines `current`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-pi-ai/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm-pi-ai`. | `named-package-member` |
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/llm-pi-ai`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Defines `installFailLoud`, a construct named by the note. Defines `boot`, a construct named by the note. | `symbol-definition` |
| [`packages/lsp/lsp-stdio/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/index.ts) | package entry point | Defines `providers`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `dsh`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/code-mode.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts) | runtime implementation | Defines `release`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/tool-bash-persistent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash-persistent/src/index.ts) | package entry point | Defines `reset`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) | package contract and examples | Core file in the package named by the note: `apps/cli`. | `named-package-member` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Core file in the package named by the note: `apps/cli`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `shutdown` | `const` | [`apps/cli/src/profile-boot.ts:210`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L210) | `const shutdown = createProcessShutdown(async () => { await app.current?.fiber.dispose() })` |
| `FAIL_LOUD_RELEASE_TIMEOUT_MS` | `const` | [`packages/boot/app-boot/src/index.ts:578`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L578) | `export const FAIL_LOUD_RELEASE_TIMEOUT_MS = 2_000` |
| `installFailLoud` | `function` | [`packages/boot/app-boot/src/index.ts:609`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L609) | `export function installFailLoud(` |
| `boot` | `function` | [`packages/boot/app-boot/src/index.ts:757`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L757) | `export async function boot(` |
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |
| `release` | `const` | [`packages/core/tools/src/code-mode.ts:382`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L382) | `const release = wake` |
| `current` | `let` | [`packages/llm/llm-pi-ai/src/index.ts:151`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts#L151) | `let current: () => Config = () => config` |
| `providers` | `const` | [`packages/lsp/lsp-stdio/src/index.ts:141`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/index.ts#L141) | `const providers = await (async () => {` |
| `reset` | `const` | [`packages/shell/tool-bash-persistent/src/index.ts:220`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash-persistent/src/index.ts#L220) | `const reset = async (owner: Agent, reason: string): Promise<void> => {` |

### Tests and executable evidence

- [`packages/boot/app-boot/tests/app-boot.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/app-boot.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `installFailLoud`.
- [`apps/cli/tests/args.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/args.spec.ts) — A test under the owning area exercises or imports `providers`.
- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — A test under the owning area exercises or imports `llm-pi-ai`.
- [`apps/cli/tests/process-shutdown.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/process-shutdown.spec.ts) — A test under the owning area exercises or imports `shutdown`.
- [`apps/cli/tests/headless-shutdown.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/headless-shutdown.e2e.ts) — A test under the owning area exercises or imports `shutdown`.
- [`apps/cli/tests/fixtures/never-dispose.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/never-dispose.mjs) — A test under the owning area exercises or imports `shutdown`.
- [`apps/cli/tests/fixtures/invalid-provider.cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/invalid-provider.cordis.yml) — A test under the owning area exercises or imports `llm-pi-ai`. A test under the owning area exercises or imports `providers`.
- Source verification intent: packages/boot/app-boot/tests/app-boot.spec.ts covers the release contract: the hook is awaited before the exit commits, a rejecting hook still exits 1, a never-settling hook exits after FAIL_LOUD_RELEASE_TIMEOUT_MS, and a burst of rejections reports only the first while the release still completes. Those fake-process tests cannot observe the two failure modes that matter most --- process exit code with a real event loop, and terminal state after exit --- so the regression lives in apps/cli/tests/tui-keyless-smoke.e2e.ts.

## How to read the implementation

1. Start with [`apps/cli/src/profile-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `shutdown`, `FAIL_LOUD_RELEASE_TIMEOUT_MS`, `installFailLoud`, `boot`, `dsh`, `release`, `current`, `providers`, `reset`, `ui-tui`, `ProcessTerminal.start`, `ESC [ c`, `llm-pi-ai`, `process.exit`
- Regex: `(?i)(shutdown|FAIL_LOUD_RELEASE_TIMEOUT_MS|installFailLoud|boot|release|current|providers|reset)`

```bash
rg -n --pcre2 "(?i)(shutdown|FAIL_LOUD_RELEASE_TIMEOUT_MS|installFailLoud|boot|release|current|providers|reset)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0339. HMR's initial scan deadlocked a failing boot into a silent exit 13](0339-hmr-s-initial-scan-deadlocked-a-failing-boot-into-a-silent-exit-13.md): The source note links to this decision directly.
- **`shares-code-with`** — [0527. Artifact-first NPM baseline publication](0527-artifact-first-npm-baseline-publication.md): Shares source implementation: `apps/cli`, `packages/client/modules/src/index.ts`.
- **`shares-code-with`** — [0098. Interrogating a draft provider endpoint](0098-interrogating-a-draft-provider-endpoint.md): Shares source implementation: `packages/llm/llm-pi-ai`, `packages/llm/llm-pi-ai/src/index.ts`.
- **`shares-code-with`** — [0623. TUI model-context resolution defers on the adapter-registration race](0623-tui-model-context-resolution-defers-on-the-adapter-registration-race.md): Shares source implementation: `packages/llm/llm-pi-ai`, `packages/llm/llm-pi-ai/src/index.ts`.
- **`shares-code-with`** — [0128. A pi-ai model declares its own input modalities, and undeclared means text](0128-a-pi-ai-model-declares-its-own-input-modalities-and-undeclared-means-tex.md): Shares source implementation: `packages/llm/llm-pi-ai/src/index.ts`, `packages/llm/llm-pi-ai/src/invariant.ts`.
- **`shares-code-with`** — [0087. the web configuration plane](0087-the-web-configuration-plane.md): Shares source implementation: `packages/llm/llm-pi-ai/src/index.ts`, `packages/llm/llm-pi-ai/src/invariant.ts`.
- **`shares-code-with`** — [0094. pi-ai routes are declared providers, not catalog lookups](0094-pi-ai-routes-are-declared-providers-not-catalog-lookups.md): Shares source implementation: `packages/llm/llm-pi-ai/src/index.ts`, `packages/llm/llm-pi-ai/src/invariant.ts`.
- **`shares-code-with`** — [0567. Dedicated full-screen TUI front door](0567-dedicated-full-screen-tui-front-door.md): Shares source implementation: `apps/cli`, `packages/client/modules/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0329-fail-loud-releases-the-terminal-before-exiting.md`.
