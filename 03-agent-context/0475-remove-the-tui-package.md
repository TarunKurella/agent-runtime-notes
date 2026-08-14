---
id: "dsh-note-0475"
title: "Remove the TUI package"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-08-04-remove-tui-package.md"
implementation_evidence: "medium"
target_anchor: "repository tests and release policy"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/projection"
aliases:
  - "dsh"
  - "@deepseek-ai/dsh-tui"
  - "pi-tui"
  - "packages/ui/tui"
  - "dsh-user-questions"
  - "dsh-commands"
  - "cordis.yml"
  - "Remove the TUI package"
  - "simplification"
  - "boundary"
  - "evidence"
  - "human control"
  - "lifecycle"
  - "recovery"
search_regex: "(?i)(@deepseek\\-ai/dsh\\-tui|pi\\-tui|packages/ui/tui|dsh\\-user\\-questions|dsh\\-commands|cordis\\.yml|Remove[- ]the[- ]TUI[- ]package|simplification)"
---

# 0475. Remove the TUI package — implementation context

## Open this when

Removing the implicit dsh terminal application left @deepseek-ai/dsh-tui without a shipped composition. The package still carried a terminal renderer, interactive command and question adapters, extension overlays, snapshot fixtures, a patched pi-tui dependency, and SDK scaffolding that advertised TUI as a supported application interface. Keeping that surface required maintaining a product-sized frontend whose only remaining consumer was the project generator itself. The package also made the repository's supported application inventory misleading.

## Source decision

The packages/ui/tui package is deleted without a compatibility package or alias. Its source, package tests, terminal snapshots, dependency declarations, patched pi-tui artifact, workspace references, generated service catalog entry, and documentation are removed together. Generic host and agent-loop capabilities remain unchanged. The SDK project toolchain that remained as the TUI package's final consumer is deleted by the toolchain removal decision. Host applications may still mount the provider-neutral dsh-user-questions, dsh-commands, and presentation services directly.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-08-04-remove-tui-package.md](../02-notes/implemented/simplification/2026-08-04-remove-tui-package.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-08-04-remove-tui-package.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-08-04-remove-tui-package.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/interaction/commands/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/index.ts) | package entry point | Core file in the package named by the note: `packages/interaction/commands`. | `named-package-member` |
| [`packages/interaction/commands/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/interaction/commands`. | `named-package-member` |
| [`packages/interaction/commands/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/interaction/commands`. | `named-package-member` |
| [`packages/interaction/user-questions/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-questions/src/index.ts) | package entry point | Core file in the package named by the note: `packages/interaction/user-questions`. | `named-package-member` |
| [`packages/interaction/user-questions/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-questions/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/interaction/user-questions`. | `named-package-member` |
| [`packages/interaction/user-questions/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-questions/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/interaction/user-questions`. | `named-package-member` |
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/interaction/commands`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/interaction/user-questions`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-questions) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `dsh`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) | package contract and examples | Core file in the package named by the note: `apps/cli`. | `named-package-member` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Core file in the package named by the note: `apps/cli`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |

### Tests and executable evidence

- [`apps/web/tests/shipped-composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/shipped-composition.e2e.ts) — Contains the exact code literal `dsh-commands` named by the note.
- [`apps/web/tests/goal-command-presentation.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/goal-command-presentation.e2e.ts) — Contains the exact code literal `dsh-commands` named by the note.
- Source verification intent: Repository searches and generated catalogs contain no TUI package, dependency patch, service key, or package link. The ordinary source build, typecheck, lint, hygiene, documentation gates, and remaining assembled snapshot suites run without the deleted workspace.

## How to read the implementation

1. Start with [`packages/interaction/commands/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and release policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/projection`
- Aliases: `dsh`, `@deepseek-ai/dsh-tui`, `pi-tui`, `packages/ui/tui`, `dsh-user-questions`, `dsh-commands`, `cordis.yml`, `Remove the TUI package`, `simplification`, `boundary`, `evidence`, `human control`, `lifecycle`, `recovery`
- Regex: `(?i)(@deepseek\-ai/dsh\-tui|pi\-tui|packages/ui/tui|dsh\-user\-questions|dsh\-commands|cordis\.yml|Remove[- ]the[- ]TUI[- ]package|simplification)`

```bash
rg -n --pcre2 "(?i)(@deepseek\\-ai/dsh\\-tui|pi\\-tui|packages/ui/tui|dsh\\-user\\-questions|dsh\\-commands|cordis\\.yml|Remove[- ]the[- ]TUI[- ]package|simplification)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0676. Explicit-config dsh entrypoint](0676-explicit-config-dsh-entrypoint.md): The source note links to this decision directly.
- **`source-link`** — [0490. Remove the SDK project toolchain](0490-remove-the-sdk-project-toolchain.md): The source note links to this decision directly.
- **`shares-code-with`** — [0062. Web command business surfaces and assembly (ui-commands / ui-skill / ui-subagent)](0062-web-command-business-surfaces-and-assembly-ui-commands-ui-skill-ui-subag.md): Shares source implementation: `apps/cli`, `packages/interaction/commands`.
- **`shares-code-with`** — [0215. Queued manual compaction with one durable lock](0215-queued-manual-compaction-with-one-durable-lock.md): Shares source implementation: `packages/interaction/commands`, `packages/interaction/commands/src/index.ts`.
- **`shares-code-with`** — [0551. Effect-owned TUI interactive extensions](0551-effect-owned-tui-interactive-extensions.md): Shares source implementation: `packages/interaction/commands`, `packages/interaction/commands/src/index.ts`.
- **`shares-code-with`** — [0163. Plugin-owned human command registration](0163-plugin-owned-human-command-registration.md): Shares source implementation: `packages/interaction/commands`, `packages/interaction/commands/src/index.ts`.
- **`shares-code-with`** — [0160. Human `/goal` command](0160-human-goal-command.md): Shares source implementation: `packages/interaction/commands/src/index.ts`, `packages/interaction/commands/src/invariant.ts`.
- **`shares-code-with`** — [0202. `/feedback` command](0202-feedback-command.md): Shares source implementation: `packages/interaction/commands/src/index.ts`, `packages/interaction/commands/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0475-remove-the-tui-package.md`.
