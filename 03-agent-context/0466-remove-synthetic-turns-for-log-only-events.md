---
id: "dsh-note-0466"
title: "Remove synthetic turns for log-only events"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-07-28-remove-synthetic-log-only-turns.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/agent-loop"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "flush"
  - "append"
  - "appendOutOfBand"
  - "turn/start"
  - "turn/end"
  - "SessionStore.appendOutOfBand"
  - "OutOfBandSessionEventMap"
  - "OutOfBandSessionEventType"
  - "ctx.sessions.flush"
  - "session/title"
  - "session/title-llm-request"
  - "session/event"
  - "compaction/* { turn: null }"
  - "/compact"
search_regex: "(?i)(flush|append|appendOutOfBand|turn/start|turn/end|SessionStore\\.appendOutOfBand|OutOfBandSessionEventMap|OutOfBandSessionEventType)"
---

# 0466. Remove synthetic turns for log-only events — implementation context

## Open this when

The session store exposed appendOutOfBand() so a plugin could publish a late log-only event while no agent turn was running. The method wrapped that event in turn/start and turn/end, then flushed it. This preserved the old rule that every durable event had to live inside a turn, but it made one identifier mean both a model-loop execution and a persistence-only update. That rule was introduced when persistence recovery treated the last turn/end as the only committed boundary. The persistence scanners now preserve every valid contiguous event, and crash repair reacts only to an actually open turn.

## Source decision

SessionStore.appendOutOfBand(), OutOfBandSessionEventMap, and OutOfBandSessionEventType do not exist. A plugin that owns a log-only event appends it through Session; when the operation promises durability, it explicitly awaits ctx.sessions.flush(session). No turn is opened solely to obtain that checkpoint. Core session invariants continue to enforce core-owned execution relations: turn and step numbering, enclosure of steering, assistant, tool, todo, and request-header events, and same-step tool call/result pairing.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-07-28-remove-synthetic-log-only-turns.md](../02-notes/implemented/simplification/2026-07-28-remove-synthetic-log-only-turns.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-07-28-remove-synthetic-log-only-turns.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-07-28-remove-synthetic-log-only-turns.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/chunk-rows.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `flush`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/session) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/workflow/tool-workflow/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/index.ts) | package entry point | Defines `append`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `turn/start` named by the note. Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `turn/start` named by the note. Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `turn/start` named by the note. Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `flush` | `const` | [`packages/core/session/src/chunk-rows.ts:196`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts#L196) | `const flush = (): void => {` |
| `append` | `const` | [`packages/workflow/tool-workflow/src/index.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/index.ts#L75) | `const append = <Type extends keyof ToolWorkflowRecordEventMap>(` |

### Tests and executable evidence

- [`apps/web/tests/schedule-after.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/schedule-after.e2e.ts) — Contains the exact code literal `session/title` named by the note.
- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — Contains the exact code literal `session/title` named by the note.
- [`packages/session/session-title-llm/tests/llm.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title-llm/tests/llm.spec.ts) — Contains the exact code literal `session/title-llm-request` named by the note.
- [`examples/acp-agent/tests/snapshots/session-title-after-turn/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/snapshots/session-title-after-turn/session.jsonl) — Contains the exact code literal `session/title-llm-request` named by the note.
- Source verification intent: Core invariant tests accept an unknown plugin event between turns while continuing to reject built-in execution events there. Hook, plan-mode, Code Mode dispatch, and approval invariant companions reject their execution-scoped events when no turn is open; the compaction companion separately accepts a balanced turn: null manual bracket between turns and requires numeric owners to match an open turn. Session-title service tests pin one direct fallback event under concurrent refresh, detached-session rejection, and newest-revision acceptance.

## How to read the implementation

1. Start with [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/agent-loop`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/policy`, `mechanism/registry`
- Aliases: `flush`, `append`, `appendOutOfBand`, `turn/start`, `turn/end`, `SessionStore.appendOutOfBand`, `OutOfBandSessionEventMap`, `OutOfBandSessionEventType`, `ctx.sessions.flush`, `session/title`, `session/title-llm-request`, `session/event`, `compaction/* { turn: null }`, `/compact`
- Regex: `(?i)(flush|append|appendOutOfBand|turn/start|turn/end|SessionStore\.appendOutOfBand|OutOfBandSessionEventMap|OutOfBandSessionEventType)`

```bash
rg -n --pcre2 "(?i)(flush|append|appendOutOfBand|turn/start|turn/end|SessionStore\\.appendOutOfBand|OutOfBandSessionEventMap|OutOfBandSessionEventType)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0545. Every session event is enclosed in a turn](0545-every-session-event-is-enclosed-in-a-turn.md): The source note links to this decision directly.
- **`source-link`** — [0058. Separate context injection from turn execution](0058-separate-context-injection-from-turn-execution.md): The source note links to this decision directly.
- **`source-link`** — [0215. Queued manual compaction with one durable lock](0215-queued-manual-compaction-with-one-durable-lock.md): The source note links to this decision directly.
- **`shares-code-with`** — [0102. Reusable Session preparation before publication](0102-reusable-session-preparation-before-publication.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0447. Fold trace-only session facts into load-bearing events](0447-fold-trace-only-session-facts-into-load-bearing-events.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0460. Collapse live persistence into one flush controller](0460-collapse-live-persistence-into-one-flush-controller.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0003. Event-sourced sessions with derived message history](0003-event-sourced-sessions-with-derived-message-history.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0171. Default local instruction overlay](0171-default-local-instruction-overlay.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0466-remove-synthetic-turns-for-log-only-events.md`.
