---
id: "dsh-note-0091"
title: "Packaged ripgrep spawn for glob/grep"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-01-packaged-ripgrep-search.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "systemPrompt"
  - "signal"
  - "Config"
  - "runRipgrep"
  - "singleQuote"
  - "PATH"
  - "@deepseek-ai/dsh-tool-fs-search"
  - "@vscode/ripgrep"
  - "ctx.subprocess"
  - "rgPath"
  - "--no-config"
  - "graceMs"
  - "exec.signal"
  - "--omit=optional"
search_regex: "(?i)(systemPrompt|signal|Config|runRipgrep|singleQuote|PATH|@deepseek\\-ai/dsh\\-tool\\-fs\\-search|@vscode/ripgrep)"
---

# 0091. Packaged ripgrep spawn for glob/grep — implementation context

## Open this when

The glob/grep tools ran through the bash executor seam, which made a system rg install a host dependency. On Windows and container images there is no rg on PATH by default, so the tools silently vanished there; a deployment could only discover that from the load-time probe warning. The bash seam also forced the whole model-visible argument surface through one shell-quoting helper, because a shell sat between the tool and ripgrep --- the bash-backed note recorded that coupling as the v1 trade-off and named direct spawn as the reasonable follow-up if the shell-string domain ever proved too sensitive.

## Source decision

@deepseek-ai/dsh-tool-fs-search now runs the PACKAGED ripgrep binary (@vscode/ripgrep, an npm dependency whose optional platform packages ship the binary) through the ctx.subprocess seam: runRipgrep() spawns rgPath with a plain argv vector prefixed by --no-config, collect-mode stdout/stderr, graceMs, and exec.signal forwarded. rgPath resolves lazily at the first call (memoized per process): @vscode/ripgrep resolves its platform package at module evaluation, so a static import would turn a missing or corrupt platform package (--omit=optional, partial install) into a Loader-composition failure --- the load-time.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-01-packaged-ripgrep-search.md](../02-notes/implemented/architecture/2026-08-01-packaged-ripgrep-search.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-01-packaged-ripgrep-search.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-01-packaged-ripgrep-search.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. Defines `signal`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/code-mode.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `signal`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/tool-fs-search/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/tool-fs-search`. Defines `Config`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/guard/timeout-policy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/guard/timeout-policy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/guard/timeout-policy`. | `named-package-member` |
| [`packages/fs/tool-fs-search/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/tool-fs-search`. | `named-package-member` |
| [`packages/subprocess/subprocess/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subprocess/subprocess`. | `named-package-member` |
| [`packages/subprocess/subprocess/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/subprocess/subprocess`. | `named-package-member` |
| [`packages/fs/tool-fs-search/src/search-core.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/search-core.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/tool-fs-search`. Defines `runRipgrep`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/guard/timeout-policy/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/guard/timeout-policy/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/guard/timeout-policy`. | `named-package-member` |
| [`packages/subprocess/subprocess/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subprocess/subprocess`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `systemPrompt` | `const` | [`packages/boot/app-boot/src/index.ts:822`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L822) | `const systemPrompt = ctx.get('systemPrompt')` |
| `signal` | `const` | [`packages/core/tools/src/code-mode.ts:401`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L401) | `const signal = new Promise<void>((resolve) => { wake = resolve })` |
| `Config` | `interface` | [`packages/core/tools/src/index.ts:654`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L654) | `export interface Config {` |
| `signal` | `const` | [`packages/core/tools/src/index.ts:1372`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1372) | `const signal = exec.signal` |
| `signal` | `const` | [`packages/core/tools/src/index.ts:1538`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1538) | `const signal = fused.signal` |
| `Config` | `interface` | [`packages/fs/tool-fs-search/src/index.ts:73`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/index.ts#L73) | `export interface Config {` |
| `Config` | `const` | [`packages/fs/tool-fs-search/src/index.ts:97`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/index.ts#L97) | `export const Config: z<Config> = z.object({` |
| `runRipgrep` | `function` | [`packages/fs/tool-fs-search/src/search-core.ts:211`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/search-core.ts#L211) | `export async function runRipgrep(` |
| `singleQuote` | `const` | [`scripts/publish-npm-baseline.ts:1000`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publish-npm-baseline.ts#L1000) | `const singleQuote = String.fromCodePoint(39)` |

### Tests and executable evidence

- [`packages/fs/tool-fs-search/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/tests/tools.spec.ts) — A test under the owning area exercises or imports `runRipgrep`. A test under the owning area exercises or imports `rgPath`.
- [`packages/fs/tool-fs-search/tests/rg-path.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/tests/rg-path.spec.ts) — A test under the owning area exercises or imports `runRipgrep`. A test under the owning area exercises or imports `SEARCH_FAILED`.
- [`packages/core/tools/tests/gen-tool-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/gen-tool-catalog.spec.ts) — A test under the owning area exercises or imports `grep`. A test under the owning area exercises or imports `PATH`.
- [`packages/subprocess/subprocess/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/tests/service.spec.ts) — A test under the owning area exercises or imports `graceMs`.
- [`packages/fs/tool-fs-search/tests/integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/tests/integration.spec.ts) — A test under the owning area exercises or imports `SEARCH_FAILED`.

## How to read the implementation

1. Start with [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `systemPrompt`, `signal`, `Config`, `runRipgrep`, `singleQuote`, `PATH`, `@deepseek-ai/dsh-tool-fs-search`, `@vscode/ripgrep`, `ctx.subprocess`, `rgPath`, `--no-config`, `graceMs`, `exec.signal`, `--omit=optional`
- Regex: `(?i)(systemPrompt|signal|Config|runRipgrep|singleQuote|PATH|@deepseek\-ai/dsh\-tool\-fs\-search|@vscode/ripgrep)`

```bash
rg -n --pcre2 "(?i)(systemPrompt|signal|Config|runRipgrep|singleQuote|PATH|@deepseek\\-ai/dsh\\-tool\\-fs\\-search|@vscode/ripgrep)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): The source note links to this decision directly.
- **`source-link`** — [0229. Even out the shipped tool rosters](0229-even-out-the-shipped-tool-rosters.md): The source note links to this decision directly.
- **`source-link`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): The source note links to this decision directly.
- **`shares-code-with`** — [0159. Fresh-agent Ralph workflow tool](0159-fresh-agent-ralph-workflow-tool.md): Shares source implementation: `packages/core/tools/src/code-mode.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0144. The approval seam --- one-shot permission decisions over a waterfall of answerers](0144-the-approval-seam-one-shot-permission-decisions-over-a-waterfall-of-answ.md): Shares source implementation: `packages/core/tools/src/code-mode.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0165. Typed tool returns in Code Mode](0165-typed-tool-returns-in-code-mode.md): Shares source implementation: `packages/core/tools/src/code-mode.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0228. Code Mode language dispatch and the Python SDK renderer](0228-code-mode-language-dispatch-and-the-python-sdk-renderer.md): Shares source implementation: `packages/core/tools/src/code-mode.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0032. Tool output spill policy](0032-tool-output-spill-policy.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0091-packaged-ripgrep-spawn-for-glob-grep.md`.
