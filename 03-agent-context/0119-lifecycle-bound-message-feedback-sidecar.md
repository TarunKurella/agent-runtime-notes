---
id: "dsh-note-0119"
title: "Lifecycle-bound message feedback sidecar"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-10-message-feedback-sidecar.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/projection"
aliases:
  - "SessionId"
  - "createdAt"
  - "updatedAt"
  - "list"
  - "MessageId"
  - "KvTable"
  - "flush"
  - "TypertRemoteService"
  - "/feedback"
  - "feedback/record"
  - "FEEDBACK_ONLY"
  - "@deepseek-ai/dsh-message-feedback"
  - "ctx.messageFeedback"
  - "messageFeedback"
search_regex: "(?i)(SessionId|createdAt|updatedAt|list|MessageId|KvTable|flush|TypertRemoteService)"
---

# 0119. Lifecycle-bound message feedback sidecar — implementation context

## Open this when

The existing /feedback command records an immutable Session-level feedback/record event. That event can release a pending telemetry prefix under FEEDBACK_ONLY, so it is the wrong authority for an editable positive/negative rating and optional note attached to one assistant message. Message feedback needs independent update and delete semantics without entering the canonical Session log, changing a projection, reaching model context, or implicitly consenting to telemetry. A sidecar keyed only by SessionId can outlive the log lifecycle it describes when an id is recreated with a different header identity.

## Source decision

@deepseek-ai/dsh-message-feedback owns the ctx.messageFeedback service and stores message feedback as one storage-domain sidecar row per Session. The sidecar is neither Session-log content nor a Session projection. It emits no feedback/record event and performs no telemetry handoff; the command-feedback and message-feedback contracts remain independent. Every usable row is bound to the inspected Session header identity {createdAt, cwd}, not merely its SessionId. A lifecycle mismatch is treated as absence: list returns no items, and put may replace the stale row with one bound to the current identity.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-10-message-feedback-sidecar.md](../02-notes/implemented/architecture/2026-08-10-message-feedback-sidecar.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-10-message-feedback-sidecar.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-10-message-feedback-sidecar.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/feedback/message-feedback/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/message-feedback/src/index.ts) | package entry point | Core file in the package named by the note: `packages/feedback/message-feedback`. | `named-package-member` |
| [`packages/feedback/message-feedback/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/message-feedback/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/feedback/message-feedback`. | `named-package-member` |
| [`packages/feedback/message-feedback/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/message-feedback/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/feedback/message-feedback`. | `named-package-member` |
| [`packages/feedback/message-feedback`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/message-feedback) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/llm/src/brand.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts) | runtime implementation | Defines `MessageId`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `createdAt`, a construct named by the note. Defines `updatedAt`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Defines `SessionId`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts) | package entry point | Defines `flush`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/protocol/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/protocol/src/index.ts) | package entry point | Defines `TypertRemoteService`, a construct named by the note. | `symbol-definition` |
| [`packages/hooks/hook-protocol/src/merge.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/storage/storage-domain/src/domain.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-domain/src/domain.ts) | runtime implementation | Defines `KvTable`, a construct named by the note. | `symbol-definition` |
| [`packages/feedback/message-feedback/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/message-feedback/README.md) | package contract and examples | Core file in the package named by the note: `packages/feedback/message-feedback`. Contains the exact code literal `session/disposed` named by the note. | `exact-code-occurrence, named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `SessionId` | `type` | [`packages/core/session/src/types.ts:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L22) | `export type SessionId = Branded<'SessionId'>` |
| `createdAt` | `const` | [`packages/goal/goal/src/fold.ts:160`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L160) | `const createdAt = nonNegativeInteger(value['createdAt'], 'createdAt')` |
| `updatedAt` | `const` | [`packages/goal/goal/src/fold.ts:161`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L161) | `const updatedAt = nonNegativeInteger(value['updatedAt'], 'updatedAt')` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `MessageId` | `type` | [`packages/llm/llm/src/brand.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts#L16) | `export type MessageId = Branded<'MessageId'>` |
| `KvTable` | `interface` | [`packages/storage/storage-domain/src/domain.ts:42`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-domain/src/domain.ts#L42) | `export interface KvTable<K extends string, V> {` |
| `flush` | `const` | [`packages/typert/loader/src/index.ts:392`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts#L392) | `const flush = (onError: (error: Error) => void): Promise<void>[] => {` |
| `TypertRemoteService` | `class` | [`packages/typert/protocol/src/index.ts:147`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/protocol/src/index.ts#L147) | `export abstract class TypertRemoteService<out T = never> extends Service<T> {` |

### Tests and executable evidence

- [`packages/typert/protocol/tests/protocol.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/protocol/tests/protocol.spec.ts) — A test under the owning area exercises or imports `TypertRemoteService`.
- [`packages/feedback/message-feedback/tests/helpers.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/message-feedback/tests/helpers.ts) — A test under the owning area exercises or imports `MessageId`. A test under the owning area exercises or imports `listSnapshots`.
- [`packages/storage/storage-domain/tests/domain.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-domain/tests/domain.spec.ts) — A test under the owning area exercises or imports `KvTable`.
- [`packages/typert/protocol/tests/fixtures/source-launch.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/protocol/tests/fixtures/source-launch.ts) — A test under the owning area exercises or imports `TypertRemoteService`.
- [`packages/feedback/message-feedback/tests/message-feedback.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/message-feedback/tests/message-feedback.spec.ts) — A test under the owning area exercises or imports `messageFeedback`. A test under the owning area exercises or imports `put`.
- [`packages/feedback/message-feedback/tests/loader-composition.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/message-feedback/tests/loader-composition.spec.ts) — A test under the owning area exercises or imports `messageFeedback`. A test under the owning area exercises or imports `put`.
- [`apps/web/tests/feedback-command.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/feedback-command.e2e.ts) — Contains the exact code literal `feedback/record` named by the note.
- [`packages/client/runtime/tests/manager.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/manager.client.spec.ts) — Contains the exact code literal `host/session-removed` named by the note.

## How to read the implementation

1. Start with [`packages/feedback/message-feedback/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/message-feedback/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/projection`
- Aliases: `SessionId`, `createdAt`, `updatedAt`, `list`, `MessageId`, `KvTable`, `flush`, `TypertRemoteService`, `/feedback`, `feedback/record`, `FEEDBACK_ONLY`, `@deepseek-ai/dsh-message-feedback`, `ctx.messageFeedback`, `messageFeedback`
- Regex: `(?i)(SessionId|createdAt|updatedAt|list|MessageId|KvTable|flush|TypertRemoteService)`

```bash
rg -n --pcre2 "(?i)(SessionId|createdAt|updatedAt|list|MessageId|KvTable|flush|TypertRemoteService)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0200. Continuable subagents](0200-continuable-subagents.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/llm/llm/src/brand.ts`.
- **`shares-code-with`** — [0014. Branded IDs everywhere they belong](0014-branded-ids-everywhere-they-belong.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/llm/llm/src/brand.ts`.
- **`shares-code-with`** — [0088. Claim inbox input before one pre-step decision](0088-claim-inbox-input-before-one-pre-step-decision.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/llm/llm/src/brand.ts`.
- **`shares-code-with`** — [0071. Create every message as an identified immutable value](0071-create-every-message-as-an-identified-immutable-value.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/llm/llm/src/brand.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0303. Bind JSONL session identity before mutation](0303-bind-jsonl-session-identity-before-mutation.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/hooks/hook-protocol/src/merge.ts`.
- **`shares-code-with`** — [0446. Drop the mutable session summary](0446-drop-the-mutable-session-summary.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/goal/goal/src/fold.ts`.
- **`shares-code-with`** — [0035. Agent-scope runtime design and correctness](0035-agent-scope-runtime-design-and-correctness.md): Shares source implementation: `packages/core/session/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0119-lifecycle-bound-message-feedback-sidecar.md`.
