---
id: "dsh-note-0303"
title: "Bind JSONL session identity before mutation"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-20-jsonl-storage-identity.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "list"
  - "SessionHeader"
  - "logPath"
  - "PersistenceBackend"
  - "loadStored"
  - "header.id === id"
  - "PersistenceBackend<TornMarker>"
  - "Bind JSONL session identity before mutation"
  - "bug fix"
  - "boundary"
  - "concurrency"
  - "discovery routing"
  - "evidence"
  - "lifecycle"
search_regex: "(?i)(list|SessionHeader|logPath|PersistenceBackend|loadStored|header\\.id[- ]===[- ]id|PersistenceBackend<TornMarker>|Bind[- ]JSONL[- ]session[- ]identity[- ]before[- ]mutation)"
---

# 0303. Bind JSONL session identity before mutation — implementation context

## Open this when

JSONL lookup selects a physical log from the requested session id across project directories, while the parsed SessionHeader supplies the metadata used by later repair and append operations. Without binding those two facts, a log selected for session A can declare session B's id or cwd and redirect a repair or later append to B's path. The project scan also needs a defined result when the same encoded id exists in more than one project directory. SQLite does not share this ambiguity because its primary-key query binds metadata and events to the requested id.

## Source decision

loadStored(id) is the coordinator's single stored-prefix lookup. The JSONL backend scans every project directory, requires at most one matching encoded session directory with a transcript, parses that file, then validates header.id === id and that the selected path either equals logPath(root, header.cwd, header.id) or filesystem canonicalization resolves both spellings to the same transcript. list() applies the same path validation and rejects duplicate ids across project directories.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-20-jsonl-storage-identity.md](../02-notes/implemented/bug-fix/2026-07-20-jsonl-storage-identity.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-20-jsonl-storage-identity.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-20-jsonl-storage-identity.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Defines `SessionHeader`, a construct named by the note. | `symbol-definition` |
| [`packages/hooks/hook-protocol/src/merge.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/Menu.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Menu.tsx) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/client/web-react/src/scoped-slots.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/scoped-slots.tsx) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-commands/src/client/service.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/service.ts) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-persistence/src/coordinator.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/coordinator.ts) | runtime implementation | Defines `PersistenceBackend`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-persistence-jsonl/src/format.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/format.ts) | runtime implementation | Defines `logPath`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `list` | `const` | [`packages/client/ui-commands/src/client/service.ts:244`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/service.ts#L244) | `const list = await this.directory.ensureReady(session.sessionId, req.signal)` |
| `list` | `const` | [`packages/client/ui-primitives/src/Menu.tsx:265`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Menu.tsx#L265) | `const list = open && (` |
| `list` | `let` | [`packages/client/web-react/src/scoped-slots.tsx:839`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/scoped-slots.tsx#L839) | `let list = [...rows].sort((a, b) => a.order - b.order)` |
| `SessionHeader` | `interface` | [`packages/core/session/src/types.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L61) | `export interface SessionHeader {` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `logPath` | `function` | [`packages/session/session-persistence-jsonl/src/format.ts:201`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/format.ts#L201) | `export function logPath(` |
| `PersistenceBackend` | `interface` | [`packages/session/session-persistence/src/coordinator.ts:127`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/coordinator.ts#L127) | `export interface PersistenceBackend<TornMarker = unknown> {` |
| `list` | `let` | [`packages/typert/generator/src/cordis-catalog.ts:433`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts#L433) | `let list: string[] = []` |

### Tests and executable evidence

- [`packages/host/apiproxy/tests/api-proxy-cold.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-cold.spec.ts) — A test under the owning area exercises or imports `loadStored`.
- [`packages/session/session-persistence-jsonl/tests/zstd.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/tests/zstd.spec.ts) — A test under the owning area exercises or imports `loadStored`. A test under the owning area exercises or imports `logPath`.
- [`packages/session/session-persistence-jsonl/tests/jsonl.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/tests/jsonl.spec.ts) — A test under the owning area exercises or imports `loadStored`. A test under the owning area exercises or imports `logPath`.
- [`packages/session/session-persistence/tests/persistence.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/tests/persistence.spec.ts) — A test under the owning area exercises or imports `loadStored`. A test under the owning area exercises or imports `PersistenceBackend`.
- [`packages/session/session-persistence-sqlite/tests/sqlite.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-sqlite/tests/sqlite.spec.ts) — A test under the owning area exercises or imports `loadStored`.

## How to read the implementation

1. Start with [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/extensions`, `domain/filesystem`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/projection`, `mechanism/registry`
- Aliases: `list`, `SessionHeader`, `logPath`, `PersistenceBackend`, `loadStored`, `header.id === id`, `PersistenceBackend<TornMarker>`, `Bind JSONL session identity before mutation`, `bug fix`, `boundary`, `concurrency`, `discovery routing`, `evidence`, `lifecycle`
- Regex: `(?i)(list|SessionHeader|logPath|PersistenceBackend|loadStored|header\.id[- ]===[- ]id|PersistenceBackend<TornMarker>|Bind[- ]JSONL[- ]session[- ]identity[- ]before[- ]mutation)`

```bash
rg -n --pcre2 "(?i)(list|SessionHeader|logPath|PersistenceBackend|loadStored|header\\.id[- ]===[- ]id|PersistenceBackend<TornMarker>|Bind[- ]JSONL[- ]session[- ]identity[- ]before[- ]mutation)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0652. Prune dead methods from the persistence seam](0652-prune-dead-methods-from-the-persistence-seam.md): Shares source implementation: `packages/client/ui-commands/src/client/service.ts`, `packages/client/ui-primitives/src/Menu.tsx`.
- **`shares-code-with`** — [0199. Workspace Registration Deletion](0199-workspace-registration-deletion.md): Shares source implementation: `packages/client/ui-primitives/src/Menu.tsx`, `packages/client/web-react/src/scoped-slots.tsx`.
- **`shares-code-with`** — [0232. Session archive (registry-global set)](0232-session-archive-registry-global-set.md): Shares source implementation: `packages/client/ui-primitives/src/Menu.tsx`, `packages/client/web-react/src/scoped-slots.tsx`.
- **`shares-code-with`** — [0048. Zstandard JSONL session logs](0048-zstandard-jsonl-session-logs.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/session/session-persistence-jsonl/tests/zstd.spec.ts`.
- **`shares-code-with`** — [0150. Expose agent session identity and JSONL location to tools and hooks](0150-expose-agent-session-identity-and-jsonl-location-to-tools-and-hooks.md): Shares source implementation: `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0119. Lifecycle-bound message feedback sidecar](0119-lifecycle-bound-message-feedback-sidecar.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/hooks/hook-protocol/src/merge.ts`.
- **`shares-code-with`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares source implementation: `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0122. Session log versioning --- one integer, an upgrade chain, and a per-event ignorable marker](0122-session-log-versioning-one-integer-an-upgrade-chain-and-a-per-event-igno.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/session/session-persistence/src/coordinator.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0303-bind-jsonl-session-identity-before-mutation.md`.
