---
id: "dsh-note-0652"
title: "Prune dead methods from the persistence seam"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-06-20-prune-dead-seam-methods.md"
implementation_evidence: "medium"
target_anchor: "append-only event and session store"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/performance"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/archived"
aliases:
  - "append"
  - "list"
  - "SessionPersistence.has"
  - ".delete"
  - "BashExecutor.get"
  - ".list"
  - "loadStored"
  - "deleteStored"
  - "deleteCore"
  - "PersistenceBackend.deleteStored"
  - "docs/architecture.md"
  - "Prune dead methods from the persistence seam"
  - "simplification"
  - "boundary"
search_regex: "(?i)(append|list|SessionPersistence\\.has|\\.delete|BashExecutor\\.get|\\.list|loadStored|deleteStored)"
---

# 0652. Prune dead methods from the persistence seam — implementation context

## Open this when

A capability seam (interface / implementation / consumer) carries abstract methods that no consumer calls. The seam exists to let implementations and consumers evolve independently --- but a method no consumer programs against is not a seam, it is speculative surface every implementation must still implement and test. The abstract service declared its operations beyond create/append: load, list, has, delete. Production consumers use load() and list() for resume and session discovery, while no production caller uses persistence has() or delete().

## Source decision

The methods nothing consumes are removed --- from the abstract seam, the implementation, and the contract/spec suites that existed only to exercise them: SessionPersistence.has() / .delete() are gone: the abstract declarations, the coordinator's has/delete/deleteCore, and the PersistenceBackend.deleteStored hook (jsonl + sqlite each implemented deleteStored only to satisfy the hook --- those implementations went too). The backends are the dual-backend design and otherwise out of scope; removing a hook they implemented for no consumer is part of removing the hook, not a backend redesign.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-06-20-prune-dead-seam-methods.md](../02-notes/archived/simplification/2026-06-20-prune-dead-seam-methods.md)
- Pinned source: [.agents/notes/archived/simplification/2026-06-20-prune-dead-seam-methods.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-06-20-prune-dead-seam-methods.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `docs/architecture.md` named by the note. | `exact-code-occurrence, named-file` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/hooks/hook-protocol/src/merge.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/Menu.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Menu.tsx) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/workflow/tool-workflow/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/index.ts) | package entry point | Defines `append`, a construct named by the note. | `symbol-definition` |
| [`packages/client/web-react/src/scoped-slots.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/scoped-slots.tsx) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `append`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-commands/src/client/service.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/service.ts) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/code-runtime/code-runtime-worker-thread/src/output-json.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/output-json.ts) | runtime implementation | Defines `append`, a construct named by the note. | `symbol-definition` |
| [`packages/code-runtime/code-runtime-worker-thread/src/worker-json.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/worker-json.ts) | runtime implementation | Defines `append`, a construct named by the note. | `symbol-definition` |
| [`CLAUDE.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/CLAUDE.md) | package contract and examples | Contains the exact code literal `docs/architecture.md` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `append` | `const` | [`packages/client/connection/src/client/fixture.ts:1681`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1681) | `const append = (id: SessionId, e: Record<string, unknown>): void => {` |
| `list` | `const` | [`packages/client/ui-commands/src/client/service.ts:244`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/service.ts#L244) | `const list = await this.directory.ensureReady(session.sessionId, req.signal)` |
| `list` | `const` | [`packages/client/ui-primitives/src/Menu.tsx:265`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Menu.tsx#L265) | `const list = open && (` |
| `list` | `let` | [`packages/client/web-react/src/scoped-slots.tsx:839`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/scoped-slots.tsx#L839) | `let list = [...rows].sort((a, b) => a.order - b.order)` |
| `append` | `function` | [`packages/code-runtime/code-runtime-worker-thread/src/output-json.ts:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/output-json.ts#L45) | `function append<T>(target: T[], value: T): void {` |
| `append` | `function` | [`packages/code-runtime/code-runtime-worker-thread/src/worker-json.ts:54`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/worker-json.ts#L54) | `function append<T>(target: T[], value: T): void {` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `list` | `let` | [`packages/typert/generator/src/cordis-catalog.ts:433`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts#L433) | `let list: string[] = []` |
| `append` | `const` | [`packages/workflow/tool-workflow/src/index.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/index.ts#L75) | `const append = <Type extends keyof ToolWorkflowRecordEventMap>(` |

### Tests and executable evidence

- [`scripts/clean.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/clean.spec.ts) — A test under the owning area exercises or imports `delete`.
- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `has`.
- [`scripts/test-invariants.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-invariants.ts) — A test under the owning area exercises or imports `has`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `has`.
- [`scripts/test-fixture-cleanup.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-fixture-cleanup.ts) — A test under the owning area exercises or imports `delete`.
- [`scripts/project-doc-site.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.spec.ts) — A test under the owning area exercises or imports `has`.
- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — A test under the owning area exercises or imports `delete`.
- [`apps/web/tests/assembled-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/assembled-boot.ts) — A test under the owning area exercises or imports `delete`.
- Source verification intent: has/delete/deleteStored are gone from the persistence seam, impl, and contract suites with no new dead exports; the remaining operations (create/append/load/list) are untouched, with persistence-backed session queries and crash recovery behaving identically; and the seam README and docs/architecture.md list only the surviving methods.

## How to read the implementation

1. Start with [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** append-only event and session store.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/performance`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/archived`
- Aliases: `append`, `list`, `SessionPersistence.has`, `.delete`, `BashExecutor.get`, `.list`, `loadStored`, `deleteStored`, `deleteCore`, `PersistenceBackend.deleteStored`, `docs/architecture.md`, `Prune dead methods from the persistence seam`, `simplification`, `boundary`
- Regex: `(?i)(append|list|SessionPersistence\.has|\.delete|BashExecutor\.get|\.list|loadStored|deleteStored)`

```bash
rg -n --pcre2 "(?i)(append|list|SessionPersistence\\.has|\\.delete|BashExecutor\\.get|\\.list|loadStored|deleteStored)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0303. Bind JSONL session identity before mutation](0303-bind-jsonl-session-identity-before-mutation.md): Shares source implementation: `packages/client/ui-commands/src/client/service.ts`, `packages/client/ui-primitives/src/Menu.tsx`.
- **`shares-code-with`** — [0199. Workspace Registration Deletion](0199-workspace-registration-deletion.md): Shares source implementation: `packages/client/ui-primitives/src/Menu.tsx`, `packages/client/web-react/src/scoped-slots.tsx`.
- **`shares-code-with`** — [0232. Session archive (registry-global set)](0232-session-archive-registry-global-set.md): Shares source implementation: `packages/client/ui-primitives/src/Menu.tsx`, `packages/client/web-react/src/scoped-slots.tsx`.
- **`shares-code-with`** — [0535. Drop durable step boundary events](0535-drop-durable-step-boundary-events.md): Shares source implementation: `docs/architecture.md`, `packages/client/connection/src/client/fixture.ts`.
- **`shares-code-with`** — [0466. Remove synthetic turns for log-only events](0466-remove-synthetic-turns-for-log-only-events.md): Shares source implementation: `docs/architecture.md`, `packages/workflow/tool-workflow/src/index.ts`.
- **`shares-code-with`** — [0122. Session log versioning --- one integer, an upgrade chain, and a per-event ignorable marker](0122-session-log-versioning-one-integer-an-upgrade-chain-and-a-per-event-igno.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/workflow/tool-workflow/src/index.ts`.
- **`shares-code-with`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares source implementation: `docs/architecture.md`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0652-prune-dead-methods-from-the-persistence-seam.md`.
