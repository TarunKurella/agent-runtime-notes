---
id: "dsh-note-0171"
title: "Default local instruction overlay"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-21-local-instruction-overlay.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/performance"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
aliases:
  - "probeScopeInstruction"
  - "reconcileInstructionContext"
  - "Config"
  - "AGENTS.local.md"
  - "CLAUDE.local.md"
  - ".local."
  - "instructionFileCandidates"
  - "localInstructionFileCandidates"
  - "['AGENTS.local.md', 'CLAUDE.local.md']"
  - "cordis.yml"
  - "$DSH_HOME/AGENTS.md"
  - "$DSH_HOME"
  - "AGENTS.md"
  - "dsh-session"
search_regex: "(?i)(probeScopeInstruction|reconcileInstructionContext|Config|AGENTS\\.local\\.md|CLAUDE\\.local\\.md|\\.local\\.|instructionFileCandidates|localInstructionFileCandidates)"
---

# 0171. Default local instruction overlay — implementation context

## Open this when

Personal, git-ignored guidance (AGENTS.local.md / CLAUDE.local.md) is a Claude Code convention for per-developer overrides that are deliberately not committed. The agent-instructions plugin loaded only one candidate per directory, so a .local. name could only be reached by adding it to instructionFileCandidates, where --- because a directory has one winner --- it would shadow the committed base file instead of supplementing it. That inverts the additive "base plus personal overlay" model the names evoke, and it was off by default.

## Source decision

The plugin loads a second, independent candidate list per project directory. localInstructionFileCandidates defaults to ['AGENTS.local.md', 'CLAUDE.local.md'] and is resolved with the same same-directory validation as instructionFileCandidates. In every project directory from the root to the session cwd, the plugin loads the base candidates and then, additively, the local candidates; the local files are ordered after the base files so their guidance takes precedence within the byte budget. Both lists load in full under per-directory content dedup. An empty localInstructionFileCandidates disables the overlay.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-21-local-instruction-overlay.md](../02-notes/implemented/feature/2026-07-21-local-instruction-overlay.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-21-local-instruction-overlay.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-21-local-instruction-overlay.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/session) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `Config`, a construct named by the note. | `symbol-definition` |
| [`packages/context/agent-instructions/src/state.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/state.ts) | runtime implementation | Defines `reconcileInstructionContext`, a construct named by the note. | `symbol-definition` |
| [`packages/context/agent-instructions/src/files.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/files.ts) | runtime implementation | Defines `probeScopeInstruction`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`CLAUDE.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/CLAUDE.md) | package contract and examples | Contains the exact code literal `dsh-session` named by the note. | `exact-code-occurrence` |
| [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) | package contract and examples | Contains the exact code literal `dsh-session` named by the note. | `exact-code-occurrence` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-session` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `probeScopeInstruction` | `function` | [`packages/context/agent-instructions/src/files.ts:460`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/files.ts#L460) | `export async function probeScopeInstruction(` |
| `reconcileInstructionContext` | `function` | [`packages/context/agent-instructions/src/state.ts:246`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/state.ts#L246) | `export async function reconcileInstructionContext(` |
| `Config` | `interface` | [`vendor/hmr/src/index.ts:553`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L553) | `export interface Config extends ChokidarOptions {` |

### Tests and executable evidence

- [`packages/context/agent-instructions/tests/agent-instructions.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/tests/agent-instructions.spec.ts) — A test under the owning area exercises or imports `reconcileInstructionContext`.
- [`scripts/client-bundle-purity.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-purity.spec.ts) — Contains the exact code literal `dsh-session` named by the note.

## How to read the implementation

1. Start with [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/performance`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/ui-interaction`, `lifecycle/implemented`
- Aliases: `probeScopeInstruction`, `reconcileInstructionContext`, `Config`, `AGENTS.local.md`, `CLAUDE.local.md`, `.local.`, `instructionFileCandidates`, `localInstructionFileCandidates`, `['AGENTS.local.md', 'CLAUDE.local.md']`, `cordis.yml`, `$DSH_HOME/AGENTS.md`, `$DSH_HOME`, `AGENTS.md`, `dsh-session`
- Regex: `(?i)(probeScopeInstruction|reconcileInstructionContext|Config|AGENTS\.local\.md|CLAUDE\.local\.md|\.local\.|instructionFileCandidates|localInstructionFileCandidates)`

```bash
rg -n --pcre2 "(?i)(probeScopeInstruction|reconcileInstructionContext|Config|AGENTS\\.local\\.md|CLAUDE\\.local\\.md|\\.local\\.|instructionFileCandidates|localInstructionFileCandidates)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0136. Workspace context instruction files](0136-workspace-context-instruction-files.md): The source note links to this decision directly.
- **`source-link`** — [0170. Load all instruction candidates with per-directory dedup](0170-load-all-instruction-candidates-with-per-directory-dedup.md): The source note links to this decision directly.
- **`shares-code-with`** — [0102. Reusable Session preparation before publication](0102-reusable-session-preparation-before-publication.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0447. Fold trace-only session facts into load-bearing events](0447-fold-trace-only-session-facts-into-load-bearing-events.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0466. Remove synthetic turns for log-only events](0466-remove-synthetic-turns-for-log-only-events.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0311. Load sessions persisted before message identity](0311-load-sessions-persisted-before-message-identity.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0154. Session query relationship tracing](0154-session-query-relationship-tracing.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0003. Event-sourced sessions with derived message history](0003-event-sourced-sessions-with-derived-message-history.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0171-default-local-instruction-overlay.md`.
