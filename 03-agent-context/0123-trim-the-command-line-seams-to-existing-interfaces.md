---
id: "dsh-note-0123"
title: "Trim the command-line seams to existing interfaces"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-11-cmdline-seam-trim.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/streaming"
aliases:
  - "internals"
  - "path"
  - "mode"
  - "key"
  - "Entry.enableRuntime"
  - "enableRuntime"
  - "enableRow"
  - "dsh-cmdline"
  - "--dev"
  - "EntryConfigResolver"
  - "headless-runner"
  - "headlessIo"
  - "ctx.appExit"
  - "appExit"
search_regex: "(?i)(internals|path|mode|Entry\\.enableRuntime|enableRuntime|enableRow|dsh\\-cmdline|\\-\\-dev)"
---

# 0123. Trim the command-line seams to existing interfaces — implementation context

## Open this when

The app-owned command line (note) shipped with three seams that were wider than their consumers needed: a vendored in-memory row-activation state machine (Entry.enableRuntime plus enableRow exported from dsh-cmdline, a command-line package owning a Loader concept) whose only purpose was the --dev conditional reload row, a vendored EntryConfigResolver protocol symbol whose only implementer was Include, and a launcher that still recognized the headless-runner row to pick SIGTERM exit codes, gate user-patch watching, and provide a headlessIo seam duplicating ctx.appExit.

## Source decision

Express all three with interfaces that already exist: No conditional dev row. The reload chain stops being conditional: dsh-web-app mounts the client-hmr row unconditionally and --dev is deleted, along with the web runtime's mode config, the mode-forked prompt contract, and the DSH_WEB_MODE bash variable. Without a rebuild watcher (pnpm run dev:web) rewriting client bundles, the chain polls unchanged files and stays idle, so the always-on row costs one stat-poll interval and an SSE route. Entry.enableRuntime, its two state fields, and enableRow are deleted with nothing replacing them. Tree-carrier config.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-11-cmdline-seam-trim.md](../02-notes/implemented/architecture/2026-08-11-cmdline-seam-trim.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-11-cmdline-seam-trim.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-11-cmdline-seam-trim.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/hmr`. Defines `path`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/boot/cmdline/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline/src/index.ts) | package entry point | Core file in the package named by the note: `packages/boot/cmdline`. | `named-package-member` |
| [`packages/bundle/web-app/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/src/index.ts) | package entry point | Core file in the package named by the note: `packages/bundle/web-app`. Defines `internals`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/hmr/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/hmr`. | `named-package-member` |
| [`packages/boot/cmdline/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/boot/cmdline`. | `named-package-member` |
| [`packages/bundle/web-app/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/bundle/web-app`. | `named-package-member` |
| [`packages/client/hmr`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/boot/cmdline`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/bundle/web-app`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/cordis/src/reflect.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/reflect.ts) | runtime implementation | Defines `key`, a construct named by the note. | `symbol-definition` |
| [`packages/e2b/fs-e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts) | package entry point | Defines `mode`, a construct named by the note. | `symbol-definition` |
| [`packages/client/hmr/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/README.md) | package contract and examples | Core file in the package named by the note: `packages/client/hmr`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `internals` | `const` | [`packages/bundle/web-app/src/index.ts:127`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/src/index.ts#L127) | `export const internals: { resolveDistIndex: () => string } = { resolveDistIndex }` |
| `path` | `const` | [`packages/client/hmr/src/index.ts:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts#L121) | `const path = ctx.clientModules.clientPath(row.id)` |
| `mode` | `const` | [`packages/e2b/fs-e2b/src/index.ts:535`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L535) | `const mode = existing === undefined ? 0o600 : existing.mode & 0o777` |
| `key` | `const` | [`vendor/cordis/src/reflect.ts:154`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/reflect.ts#L154) | `const key = target[symbols.isolate][prop]` |

### Tests and executable evidence

- [`packages/boot/cmdline/tests/cmdline.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline/tests/cmdline.spec.ts) — A test under the owning area exercises or imports `appExit`. A test under the owning area exercises or imports `internals`.
- [`packages/bundle/web-app/tests/startup.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/tests/startup.spec.ts) — A test under the owning area exercises or imports `dsh-cmdline`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — Contains the exact code literal `dsh-cmdline` named by the note.

## How to read the implementation

1. Start with [`packages/client/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/streaming`
- Aliases: `internals`, `path`, `mode`, `key`, `Entry.enableRuntime`, `enableRuntime`, `enableRow`, `dsh-cmdline`, `--dev`, `EntryConfigResolver`, `headless-runner`, `headlessIo`, `ctx.appExit`, `appExit`
- Regex: `(?i)(internals|path|mode|Entry\.enableRuntime|enableRuntime|enableRow|dsh\-cmdline|\-\-dev)`

```bash
rg -n --pcre2 "(?i)(internals|path|mode|Entry\\.enableRuntime|enableRuntime|enableRow|dsh\\-cmdline|\\-\\-dev)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0105. Apps own their command line through `ctx.cmdlineArgs`](0105-apps-own-their-command-line-through-ctx-cmdlineargs.md): The source note links to this decision directly.
- **`shares-code-with`** — [0115. headless is a direct core entry point](0115-headless-is-a-direct-core-entry-point.md): Shares source implementation: `packages/bundle/web-app`, `packages/bundle/web-app/src/index.ts`.
- **`shares-code-with`** — [0274. inline-code file mentions open the file they name](0274-inline-code-file-mentions-open-the-file-they-name.md): Shares source implementation: `packages/bundle/web-app`, `packages/bundle/web-app/src/index.ts`.
- **`shares-code-with`** — [0313. Web agents receive explicit runtime context](0313-web-agents-receive-explicit-runtime-context.md): Shares source implementation: `packages/bundle/web-app`, `packages/bundle/web-app/src/index.ts`.
- **`shares-code-with`** — [0054. Client plugin loading --- plain packages, dsh.client plugins, and the two-phase boot](0054-client-plugin-loading-plain-packages-dsh-client-plugins-and-the-two-phas.md): Shares source implementation: `packages/client/hmr/src/index.ts`, `packages/client/hmr/src/invariant.ts`.
- **`shares-code-with`** — [0487. parseCmdline runs the program's own commander action](0487-parsecmdline-runs-the-program-s-own-commander-action.md): Shares source implementation: `packages/boot/cmdline`, `packages/boot/cmdline/src/index.ts`.
- **`shares-code-with`** — [0060. dsh web config-tree boot and the web transport layering](0060-dsh-web-config-tree-boot-and-the-web-transport-layering.md): Shares source implementation: `packages/client/hmr/src/index.ts`, `packages/client/hmr/src/invariant.ts`.
- **`shares-code-with`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares source implementation: `vendor/cordis/src/reflect.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0123-trim-the-command-line-seams-to-existing-interfaces.md`.
