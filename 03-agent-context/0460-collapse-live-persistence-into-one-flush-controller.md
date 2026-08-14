---
id: "dsh-note-0460"
title: "Collapse live persistence into one flush controller"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-07-23-collapse-persistence-flush-state.md"
implementation_evidence: "medium"
target_anchor: "append-only event and session store"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/extensions"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "resume"
  - "interrupted"
  - "pending"
  - "meta"
  - "createdAt"
  - "append"
  - "session/flush"
  - "session/event"
  - "SessionState.meta"
  - "session/created"
  - "loadStored"
  - "Collapse live persistence into one flush controller"
  - "simplification"
  - "boundary"
search_regex: "(?i)(resume|interrupted|pending|meta|createdAt|append|session/flush|session/event)"
---

# 0460. Collapse live persistence into one flush controller — implementation context

## Open this when

The persistence coordinator represented one live session's write lifecycle with separate buffer, initialization, and retirement containers plus the per-id operation chain. Those structures mirrored the same fact: whether that exact Session still had initialization or events that must settle before its state could be released. The checkpoint-only drain also kept every event volatile until another plugin requested session/flush, even though the backend could begin durability work without blocking the synchronous producer.

## Source decision

Each live Session has one lifecycle entry containing initialization and one package-private write controller. The controller owns pending, the fixed batching timer, the optional active write, automatic-retry pause, and the shared flush barrier. A session/event listener copies the frozen event into pending; the first event starts a fixed deadline, and later events join without resetting it. A write takes one stable pending prefix; events admitted while it runs remain pending for a separately bounded follow-up batch. session/flush is an immediate quiescence barrier.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-07-23-collapse-persistence-flush-state.md](../02-notes/implemented/simplification/2026-07-23-collapse-persistence-flush-state.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-07-23-collapse-persistence-flush-state.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-07-23-collapse-persistence-flush-state.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. Defines `pending`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/surface.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `pending`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/session) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `createdAt`, a construct named by the note. | `symbol-definition` |
| [`packages/api/remotes/src/agent-lookup.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/src/agent-lookup.ts) | runtime implementation | Defines `resume`, a construct named by the note. | `symbol-definition` |
| [`packages/workflow/tool-workflow/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/index.ts) | package entry point | Defines `append`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-workflow-run/src/client/workflow-definition.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workflow-run/src/client/workflow-definition.ts) | runtime implementation | Defines `interrupted`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/session`. Contains the exact code literal `session/created` named by the note. | `exact-code-occurrence, named-package-member` |
| [`packages/core/session/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `session/event` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `resume` | `let` | [`packages/api/remotes/src/agent-lookup.ts:143`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/src/agent-lookup.ts#L143) | `let resume = resumes.get(sessionId)` |
| `interrupted` | `const` | [`packages/client/ui-workflow-run/src/client/workflow-definition.ts:95`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workflow-run/src/client/workflow-definition.ts#L95) | `const interrupted = state.stopReason === undefined` |
| `pending` | `const` | [`packages/core/session/src/index.ts:198`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L198) | `const pending: object[] = [value]` |
| `meta` | `const` | [`packages/core/session/src/index.ts:876`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L876) | `const meta = options?.meta` |
| `pending` | `const` | [`packages/core/session/src/surface.ts:450`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L450) | `const pending = this._pendingPlan` |
| `createdAt` | `const` | [`packages/goal/goal/src/fold.ts:160`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L160) | `const createdAt = nonNegativeInteger(value['createdAt'], 'createdAt')` |
| `append` | `const` | [`packages/workflow/tool-workflow/src/index.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/index.ts#L75) | `const append = <Type extends keyof ToolWorkflowRecordEventMap>(` |

### Tests and executable evidence

- [`packages/core/session/tests/fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/fork.spec.ts) — A test under the owning area exercises or imports `interrupted`.
- [`packages/core/session/tests/repair.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/repair.spec.ts) — A test under the owning area exercises or imports `interrupted`.
- [`packages/core/session/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/invariant.spec.ts) — A test under the owning area exercises or imports `interrupted`.
- Source verification intent: Focused controller tests use a fake clock to prove the non-resetting fixed window, gate the first append, admit another event during that write, and observe an automatic second durable batch without calling session/flush. The shared coordinator contract still covers live adoption, collisions, crash repair, and session/backend disposal over the in-memory, JSONL, and SQLite backends. Failure and teardown tests keep rejected batches pending, retry them before close, and prove an in-flight controller delays backend close.

## How to read the implementation

1. Start with [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/simplification`, `domain/extensions`, `domain/observability`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `resume`, `interrupted`, `pending`, `meta`, `createdAt`, `append`, `session/flush`, `session/event`, `SessionState.meta`, `session/created`, `loadStored`, `Collapse live persistence into one flush controller`, `simplification`, `boundary`
- Regex: `(?i)(resume|interrupted|pending|meta|createdAt|append|session/flush|session/event)`

```bash
rg -n --pcre2 "(?i)(resume|interrupted|pending|meta|createdAt|append|session/flush|session/event)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0110. Bounded session persistence write batching](0110-bounded-session-persistence-write-batching.md): The source note links to this decision directly.
- **`shares-code-with`** — [0466. Remove synthetic turns for log-only events](0466-remove-synthetic-turns-for-log-only-events.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0635. Generated persistence log event catalog](0635-generated-persistence-log-event-catalog.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0504. Frame-coalesced reasoning-chunk publication and browser stress validation](0504-frame-coalesced-reasoning-chunk-publication-and-browser-stress-validatio.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0102. Reusable Session preparation before publication](0102-reusable-session-preparation-before-publication.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0446. Drop the mutable session summary](0446-drop-the-mutable-session-summary.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0565. Exact session query service](0565-exact-session-query-service.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0460-collapse-live-persistence-into-one-flush-controller.md`.
