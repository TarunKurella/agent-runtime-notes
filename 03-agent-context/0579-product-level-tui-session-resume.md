---
id: "dsh-note-0579"
title: "Product-level TUI session resume"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-21-tui-resume-command.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
aliases:
  - "dsh"
  - "SessionId"
  - "/resume"
  - "session-query.readSession"
  - "TuiRuntime.handoffResume"
  - "handoffResume"
  - "process.execve"
  - "--resume"
  - "/goal resume"
  - "resumeCommand"
  - "session-query"
  - "Product-level TUI session resume"
  - "feature"
  - "boundary"
search_regex: "(?i)(SessionId|/resume|session\\-query\\.readSession|TuiRuntime\\.handoffResume|handoffResume|process\\.execve|\\-\\-resume|/goal[- ]resume)"
---

# 0579. Product-level TUI session resume — implementation context

## Open this when

The original /resume printed shell commands. It did not let a keyboard user inspect titles or outcomes, distinguish corruption from a missing adapter, or safely transfer the terminal. Leaving the TUI and manually launching a command also hid the required ordering: finish current work, flush it, release the UI and app, then restore the exact persisted identity without silently creating a replacement.

## Source decision

/resume uses the TUI's existing interactive overlay seam as a full-viewport picker rather than a centered dialog. The flat page keeps the search field, workspace scope line, candidates, and shortcut footer in stable screen regions; only the active row uses the accent role. Its search editor starts immediately after the search glyph and emits pi-tui's cursor marker, so terminal IME composition remains anchored in the field. Escape clears a non-empty query before a second Escape closes the picker. It orders candidates by last logged activity and searches log-backed title or id.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-21-tui-resume-command.md](../02-notes/archived/feature/2026-07-21-tui-resume-command.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-21-tui-resume-command.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-21-tui-resume-command.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/session-query/session-query/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session-query/session-query`. | `named-package-member` |
| [`packages/session-query/session-query/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/session-query/session-query`. | `named-package-member` |
| [`packages/session-query/session-query/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session-query/session-query`. | `named-package-member` |
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session-query/session-query`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Defines `SessionId`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `dsh`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) | package contract and examples | Core file in the package named by the note: `apps/cli`. | `named-package-member` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Core file in the package named by the note: `apps/cli`. | `named-package-member` |
| [`packages/session-query/session-query/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/README.md) | package contract and examples | Core file in the package named by the note: `packages/session-query/session-query`. | `named-package-member` |
| [`packages/session-query/session-query/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/package.json) | composition and configuration | Core file in the package named by the note: `packages/session-query/session-query`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |
| `SessionId` | `type` | [`packages/core/session/src/types.ts:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L22) | `export type SessionId = Branded<'SessionId'>` |

### Tests and executable evidence

- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `SessionId`.
- [`apps/cli/tests/fixtures/dsh-badge/snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/dsh-badge/snapshot.ts) — A test under the owning area exercises or imports `SessionId`.
- Source verification intent: TUI package tests cover keyboard navigation, title/id search, search-clear/cancel behavior, running-agent refusal, refusal of the current session and sessions already live in this runtime, route absence, corrupt rows, preflight revalidation, the no-host warning, and stop-before-handoff ordering. Session-query tests pin detached full-log validation. Agent-loop resume tests pin exact identity and history; title, todo, and goal replay suites pin restored projections and disarmed goal activation.

## How to read the implementation

1. Start with [`packages/session-query/session-query/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`
- Aliases: `dsh`, `SessionId`, `/resume`, `session-query.readSession`, `TuiRuntime.handoffResume`, `handoffResume`, `process.execve`, `--resume`, `/goal resume`, `resumeCommand`, `session-query`, `Product-level TUI session resume`, `feature`, `boundary`
- Regex: `(?i)(SessionId|/resume|session\-query\.readSession|TuiRuntime\.handoffResume|handoffResume|process\.execve|\-\-resume|/goal[- ]resume)`

```bash
rg -n --pcre2 "(?i)(SessionId|/resume|session\\-query\\.readSession|TuiRuntime\\.handoffResume|handoffResume|process\\.execve|\\-\\-resume|/goal[- ]resume)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0604. Versioned TUI first-run welcome](0604-versioned-tui-first-run-welcome.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0676. Explicit-config dsh entrypoint](0676-explicit-config-dsh-entrypoint.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0527. Artifact-first NPM baseline publication](0527-artifact-first-npm-baseline-publication.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0567. Dedicated full-screen TUI front door](0567-dedicated-full-screen-tui-front-door.md): Shares source implementation: `apps/cli`, `packages/client/modules/src/index.ts`.
- **`shares-code-with`** — [0152. SQLite FTS5 session search](0152-sqlite-fts5-session-search.md): Shares source implementation: `packages/session-query/session-query`, `packages/session-query/session-query/src/index.ts`.
- **`shares-code-with`** — [0062. Web command business surfaces and assembly (ui-commands / ui-skill / ui-subagent)](0062-web-command-business-surfaces-and-assembly-ui-commands-ui-skill-ui-subag.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0402. Product-first root README](0402-product-first-root-readme.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0597. `dsh meta` boots the TUI over the harness checkout](0597-dsh-meta-boots-the-tui-over-the-harness-checkout.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0579-product-level-tui-session-resume.md`.
