---
id: "dsh-note-0170"
title: "Load all instruction candidates with per-directory dedup"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-21-instruction-load-all-dedup.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/simplification"
  - "concern/trust"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/implemented"
aliases:
  - "probeScopeInstruction"
  - "candidateScopeKey"
  - "decodeScopeKey"
  - "trimmedDigest"
  - "set"
  - "replace"
  - "instructionFileCandidates"
  - "AGENTS.md"
  - "CLAUDE.md"
  - "directory\\u0000candidateName"
  - "previousPath"
  - "context/message"
  - "dsh-session"
  - "Load all instruction candidates with per-directory dedup"
search_regex: "(?i)(probeScopeInstruction|candidateScopeKey|decodeScopeKey|trimmedDigest|replace|instructionFileCandidates|AGENTS\\.md|CLAUDE\\.md)"
---

# 0170. Load all instruction candidates with per-directory dedup — implementation context

## Open this when

The agent-instructions plugin resolved one winning file per candidate list per directory: the first existing name in instructionFileCandidates won the base slot, and the local overlay added one more winner. But AGENTS.md and CLAUDE.md routinely coexist in the same directory. In most repositories one is a symlink to the other, so they carry identical content; in repositories mid-migration they are two distinct real files that have drifted apart.

## Source decision

Every existing candidate in each list loads --- the base list first, then the local list --- in configured order. Within one directory, candidates whose content is byte-identical after trimming leading and trailing whitespace collapse to the earliest candidate in that order, and the kept file's original bytes are rendered. Dedup is per-directory rather than global, and symmetric across the base and local lists.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-21-instruction-load-all-dedup.md](../02-notes/implemented/feature/2026-07-21-instruction-load-all-dedup.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-21-instruction-load-all-dedup.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-21-instruction-load-all-dedup.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/session) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/loader/src/config/entry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/entry.ts) | runtime implementation | Defines `replace`, a construct named by the note. | `symbol-definition` |
| [`packages/context/agent-instructions/src/files.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/files.ts) | runtime implementation | Defines `probeScopeInstruction`, a construct named by the note. | `symbol-definition` |
| [`packages/context/agent-instructions/src/state.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/state.ts) | runtime implementation | Defines `trimmedDigest`, a construct named by the note. | `symbol-definition` |
| [`packages/context/agent-instructions/src/render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/render.ts) | runtime implementation | Defines `candidateScopeKey`, a construct named by the note. Defines `decodeScopeKey`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/client-runtime/src/sessions.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/sessions.ts) | runtime implementation | Defines `set`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`CLAUDE.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/CLAUDE.md) | package contract and examples | Contains the exact code literal `dsh-session` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `probeScopeInstruction` | `function` | [`packages/context/agent-instructions/src/files.ts:460`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/files.ts#L460) | `export async function probeScopeInstruction(` |
| `candidateScopeKey` | `function` | [`packages/context/agent-instructions/src/render.ts:123`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/render.ts#L123) | `export function candidateScopeKey(directory: string, candidateName: string): string {` |
| `decodeScopeKey` | `function` | [`packages/context/agent-instructions/src/render.ts:141`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/render.ts#L141) | `export function decodeScopeKey(scope: string): { directory: string; candidateName: string } {` |
| `trimmedDigest` | `const` | [`packages/context/agent-instructions/src/state.ts:394`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/state.ts#L394) | `const trimmedDigest = trimmedInstructionDigest(file.content)` |
| `set` | `const` | [`packages/test-support/client-runtime/src/sessions.ts:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/sessions.ts#L52) | `const set = listeners.get(key) ?? new Set()` |
| `replace` | `const` | [`vendor/loader/src/config/entry.ts:194`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/entry.ts#L194) | `const replace = diff.some(key => key === 'name' \|\| key === 'inject' \|\| key === 'group')` |

### Tests and executable evidence

- [`packages/context/agent-instructions/tests/agent-instructions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/tests/agent-instructions.e2e.ts) — A test under the owning area exercises or imports `candidateScopeKey`.
- [`packages/context/agent-instructions/tests/agent-instructions.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/tests/agent-instructions.spec.ts) — A test under the owning area exercises or imports `candidateScopeKey`. A test under the owning area exercises or imports `trimmedDigest`.
- [`scripts/client-bundle-purity.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-purity.spec.ts) — Contains the exact code literal `dsh-session` named by the note.
- [`packages/session-query/tool-session-query/tests/tool-session-query.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/tests/tool-session-query.spec.ts) — Contains the exact code literal `context/message` named by the note.

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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/simplification`, `concern/trust`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/storage`, `lifecycle/implemented`
- Aliases: `probeScopeInstruction`, `candidateScopeKey`, `decodeScopeKey`, `trimmedDigest`, `set`, `replace`, `instructionFileCandidates`, `AGENTS.md`, `CLAUDE.md`, `directory\u0000candidateName`, `previousPath`, `context/message`, `dsh-session`, `Load all instruction candidates with per-directory dedup`
- Regex: `(?i)(probeScopeInstruction|candidateScopeKey|decodeScopeKey|trimmedDigest|replace|instructionFileCandidates|AGENTS\.md|CLAUDE\.md)`

```bash
rg -n --pcre2 "(?i)(probeScopeInstruction|candidateScopeKey|decodeScopeKey|trimmedDigest|replace|instructionFileCandidates|AGENTS\\.md|CLAUDE\\.md)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0136. Workspace context instruction files](0136-workspace-context-instruction-files.md): The source note links to this decision directly.
- **`source-link`** — [0169. Follow symlinked instruction files](0169-follow-symlinked-instruction-files.md): The source note links to this decision directly.
- **`source-link`** — [0171. Default local instruction overlay](0171-default-local-instruction-overlay.md): The source note links to this decision directly.
- **`shares-code-with`** — [0460. Collapse live persistence into one flush controller](0460-collapse-live-persistence-into-one-flush-controller.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0466. Remove synthetic turns for log-only events](0466-remove-synthetic-turns-for-log-only-events.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0102. Reusable Session preparation before publication](0102-reusable-session-preparation-before-publication.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0003. Event-sourced sessions with derived message history](0003-event-sourced-sessions-with-derived-message-history.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0170-load-all-instruction-candidates-with-per-directory-dedup.md`.
