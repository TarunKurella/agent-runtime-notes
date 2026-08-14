---
id: "dsh-note-0570"
title: "dsh tells the agent where its own source lives"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-21-dsh-system-prompt-source-path.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "boot"
  - "addHarnessSourceSection"
  - "systemPrompt"
  - "dsh"
  - "argv"
  - "url"
  - "apps/cli/src/tui.ts"
  - "fileURLToPath(new URL('../../..', import.meta.url))"
  - "fileURLToPath"
  - "apps/cli/{src,lib}"
  - "dsh-app-boot"
  - "-99"
  - "-100"
  - "apps/cli"
search_regex: "(?i)(boot|addHarnessSourceSection|systemPrompt|argv|apps/cli/src/tui\\.ts|fileURLToPath\\(new[- ]URL\\('\\.\\./\\.\\./\\.\\.',[- ]import\\.meta\\.url\\)\\)|fileURLToPath|apps/cli/\\{src,lib\\})"
---

# 0570. dsh tells the agent where its own source lives — implementation context

## Open this when

The dsh CLI is the self-referential surface: its cordis toolset lets the agent inspect and modify the very harness runtime it runs in. But the agent had no way to learn where that source lives on disk. dsh is normally symlinked onto PATH and launched from an arbitrary working directory --- the project under work --- so neither the cwd nor argv reliably points at the harness checkout. Without the path, "read your own source" is guesswork.

## Source decision

The dsh launcher (apps/cli/src/tui.ts) computes the harness checkout root from its own module URL --- fileURLToPath(new URL('../../..', import.meta.url)), three hops up from apps/cli/{src,lib} --- so it resolves to the real source location however dsh is launched (a PATH symlink, an arbitrary cwd). After boot() settles the tree, the launcher calls the new addHarnessSourceSection(ctx, sourceRoot) helper from dsh-app-boot, which registers a global harness:source prompt section reading Your own source code is the checkout at ; you can read it there to learn how dsh works and how to extend it.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-21-dsh-system-prompt-source-path.md](../02-notes/archived/feature/2026-07-21-dsh-system-prompt-source-path.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-21-dsh-system-prompt-source-path.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-21-dsh-system-prompt-source-path.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Core file in the package named by the note: `packages/boot/app-boot`. Defines `boot`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/boot/app-boot/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/boot/app-boot`. | `named-package-member` |
| [`packages/examples/acp-demo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/src/index.ts) | package entry point | Core file in the package named by the note: `packages/examples/acp-demo`. | `named-package-member` |
| [`packages/core/system-prompt/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/system-prompt`. | `named-package-member` |
| [`packages/examples/acp-demo/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/examples/acp-demo`. | `named-package-member` |
| [`packages/core/system-prompt/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/system-prompt`. | `named-package-member` |
| [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `apps/cli`. Core file in the package named by the note: `apps/cli`. | `named-directory-member, named-package-member` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `apps/cli`. Core file in the package named by the note: `apps/cli`. | `exact-code-occurrence, named-directory-member, named-package-member` |
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The source note names this implementation area directly. The note names this package or capability. | `named-directory, named-package` |
| [`vendor/cordis`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/boot/app-boot`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `boot` | `function` | [`packages/boot/app-boot/src/index.ts:757`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L757) | `export async function boot(` |
| `addHarnessSourceSection` | `function` | [`packages/boot/app-boot/src/index.ts:821`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L821) | `export function addHarnessSourceSection(ctx: Context, sourceRoot: string): (() => void) \| undefined {` |
| `systemPrompt` | `const` | [`packages/boot/app-boot/src/index.ts:822`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L822) | `const systemPrompt = ctx.get('systemPrompt')` |
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |
| `argv` | `const` | [`packages/e2b/subprocess-e2b/src/process.ts:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/process.ts#L121) | `const argv = spec.argv.map(quoteE2BShellArg).join(' ')` |
| `url` | `const` | [`vendor/hmr/src/index.ts:257`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L257) | `const url = pathToFileURL(filename).href` |

### Tests and executable evidence

- [`apps/cli/tests/windows-shell.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/windows-shell.spec.ts) — A test under the owning area exercises or imports `dsh-app-boot`. Contains the exact code literal `dsh-app-boot` named by the note.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `dsh-app-boot`. A test under the owning area exercises or imports `systemPrompt`.
- [`apps/cli/tests/memory-mcp-configs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/memory-mcp-configs.spec.ts) — A test under the owning area exercises or imports `dsh-app-boot`.
- [`packages/boot/app-boot/tests/profile.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/profile.spec.ts) — A test under the owning area exercises or imports `dsh-app-boot`.
- [`packages/boot/app-boot/tests/app-boot.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/app-boot.spec.ts) — A test under the owning area exercises or imports `addHarnessSourceSection`.
- [`apps/cli/tests/fixtures/dsh-badge/snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/dsh-badge/snapshot.ts) — A test under the owning area exercises or imports `dsh-app-boot`.
- [`packages/boot/app-boot/tests/user-patches.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/user-patches.spec.ts) — A test under the owning area exercises or imports `dsh-app-boot`.
- [`packages/examples/acp-demo/tests/load-path.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/tests/load-path.e2e.ts) — A test under the owning area exercises or imports `dsh-acp-demo`.

## How to read the implementation

1. Start with [`vendor/cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/index.ts) because it has the strongest evidence link to the note.
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
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/performance`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/policy`, `mechanism/registry`
- Aliases: `boot`, `addHarnessSourceSection`, `systemPrompt`, `dsh`, `argv`, `url`, `apps/cli/src/tui.ts`, `fileURLToPath(new URL('../../..', import.meta.url))`, `fileURLToPath`, `apps/cli/{src,lib}`, `dsh-app-boot`, `-99`, `-100`, `apps/cli`
- Regex: `(?i)(boot|addHarnessSourceSection|systemPrompt|argv|apps/cli/src/tui\.ts|fileURLToPath\(new[- ]URL\('\.\./\.\./\.\.',[- ]import\.meta\.url\)\)|fileURLToPath|apps/cli/\{src,lib\})`

```bash
rg -n --pcre2 "(?i)(boot|addHarnessSourceSection|systemPrompt|argv|apps/cli/src/tui\\.ts|fileURLToPath\\(new[- ]URL\\('\\.\\./\\.\\./\\.\\.',[- ]import\\.meta\\.url\\)\\)|fileURLToPath|apps/cli/\\{src,lib\\})" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0556. Native TypeScript source launch for dsh](0556-native-typescript-source-launch-for-dsh.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0553. Parse `dsh` argv through one Commander adapter](0553-parse-dsh-argv-through-one-commander-adapter.md): Shares source implementation: `apps/cli/package.json`, `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0166. The dsh CLI and personal config overlays from the Harness home](0166-the-dsh-cli-and-personal-config-overlays-from-the-harness-home.md): Shares source implementation: `apps/cli/README.md`, `apps/cli/package.json`.
- **`shares-code-with`** — [0597. `dsh meta` boots the TUI over the harness checkout](0597-dsh-meta-boots-the-tui-over-the-harness-checkout.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0676. Explicit-config dsh entrypoint](0676-explicit-config-dsh-entrypoint.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0579. Product-level TUI session resume](0579-product-level-tui-session-resume.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0527. Artifact-first NPM baseline publication](0527-artifact-first-npm-baseline-publication.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0546. Extract example apps into packages](0546-extract-example-apps-into-packages.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/boot/app-boot/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0570-dsh-tells-the-agent-where-its-own-source-lives.md`.
