---
id: "dsh-note-0156"
title: "Durable per-step time context"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-16-durable-per-step-time-context.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "timeZone"
  - "refreshIntervalMs"
  - "SessionHeader"
  - "mixed"
  - "@deepseek-ai/dsh-time-context"
  - "packages/context/time-context/"
  - "agent/pre-step"
  - "user-rpc"
  - "time_zone"
  - "Intl.DateTimeFormat"
  - "DateTimeFormat"
  - "step/start"
  - "request/header"
  - "schedule_create"
search_regex: "(?i)(timeZone|refreshIntervalMs|SessionHeader|mixed|@deepseek\\-ai/dsh\\-time\\-context|packages/context/time\\-context/|agent/pre\\-step|user\\-rpc)"
---

# 0156. Durable per-step time context — implementation context

## Open this when

A request-only clock can tell the model the current time, but replacing that value in the system prompt removes the evidence behind earlier time-sensitive reasoning. Multi-step turns need requests to retain the readings used by preceding steps. The request must remain reconstructable after restart, and automatic compaction must account for the same timing context the model receives. A process-local refresh cache makes displayed time depend on state that cannot survive resume.

## Source decision

@deepseek-ai/dsh-time-context is an opt-in function plugin in packages/context/time-context/. Default compositions leave its disclosure and token cost disabled; the Schedule Web overlay mounts it so the model can interpret otherwise-unqualified dates and times in the browser zone attached to the current request. The plugin prepends an agent/pre-step listener and delegates first.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-16-durable-per-step-time-context.md](../02-notes/implemented/feature/2026-07-16-durable-per-step-time-context.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-16-durable-per-step-time-context.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-16-durable-per-step-time-context.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/context/time-context/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/context/time-context`. Core file in the package named by the note: `packages/context/time-context`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/context/time-context/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/context/time-context`. Core file in the package named by the note: `packages/context/time-context`. | `named-directory-member, named-package-member` |
| [`packages/context/time-context/src/request-zone.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/request-zone.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/context/time-context`. Core file in the package named by the note: `packages/context/time-context`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/context/time-context/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/context/time-context`. Core file in the package named by the note: `packages/context/time-context`. | `named-directory-member, named-package-member` |
| [`packages/context/time-context/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/context/time-context`. Core file in the package named by the note: `packages/context/time-context`. | `named-directory-member, named-package-member` |
| [`packages/context/time-context`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context) | package or module directory | The source note names this implementation area directly. The note names this package or capability. | `named-directory, named-package` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Defines `SessionHeader`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/llm-mock-server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-mock-server/src/index.ts) | package entry point | Defines `mixed`, a construct named by the note. | `symbol-definition` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `agent/pre-step` named by the note. Contains the exact code literal `step/start` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `agent/pre-step` named by the note. Contains the exact code literal `step/start` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `agent/pre-step` named by the note. Contains the exact code literal `step/start` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Contains the exact code literal `agent/pre-step` named by the note. Contains the exact code literal `step/start` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `timeZone` | `const` | [`packages/context/time-context/src/index.ts:146`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/index.ts#L146) | `const timeZone = config.timeZone` |
| `refreshIntervalMs` | `const` | [`packages/context/time-context/src/index.ts:147`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/index.ts#L147) | `const refreshIntervalMs = config.refreshIntervalMs` |
| `timeZone` | `const` | [`packages/context/time-context/src/request-zone.ts:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/request-zone.ts#L52) | `const timeZone = browserTimeZone(message)` |
| `SessionHeader` | `interface` | [`packages/core/session/src/types.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L61) | `export interface SessionHeader {` |
| `mixed` | `let` | [`packages/test-support/llm-mock-server/src/index.ts:595`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-mock-server/src/index.ts#L595) | `let mixed = state` |

### Tests and executable evidence

- [`packages/context/time-context/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/tests/invariant.spec.ts) — A test under the owning area exercises or imports `timeZone`. A test under the owning area exercises or imports `DateTimeFormat`.
- [`packages/context/time-context/tests/request-zone.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/tests/request-zone.spec.ts) — A test under the owning area exercises or imports `user-rpc`. A test under the owning area exercises or imports `timeZone`.
- [`packages/context/time-context/tests/time-context.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/tests/time-context.spec.ts) — A test under the owning area exercises or imports `timeZone`. A test under the owning area exercises or imports `DateTimeFormat`.
- [`apps/web/tests/live-interactions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/live-interactions.e2e.ts) — Contains the exact code literal `request/header` named by the note.
- [`apps/cli/tests/fixtures/dsh-badge/snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/dsh-badge/snapshot.ts) — Contains the exact code literal `agent/pre-step` named by the note.
- Source verification intent: Unit and real-loop tests pin timestamp formatting, unique/mixed/missing browser derivation, fallback display, both elapsed baselines, interval boundaries, cross-turn and resumed scheduling, backward-clock behavior, steering ownership, cancellation, exact snapshot validation, and request reconstruction. Host/client tests pin browser sampling plus validation and canonicalization at prompt entry. The keyless assembled Schedule Web scenario sends a real browser prompt, observes the same zone in the model request, and verifies that the model supplies it explicitly to schedule_create.

## How to read the implementation

1. Start with [`packages/context/time-context/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/jobs-tasks`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`
- Aliases: `timeZone`, `refreshIntervalMs`, `SessionHeader`, `mixed`, `@deepseek-ai/dsh-time-context`, `packages/context/time-context/`, `agent/pre-step`, `user-rpc`, `time_zone`, `Intl.DateTimeFormat`, `DateTimeFormat`, `step/start`, `request/header`, `schedule_create`
- Regex: `(?i)(timeZone|refreshIntervalMs|SessionHeader|mixed|@deepseek\-ai/dsh\-time\-context|packages/context/time\-context/|agent/pre\-step|user\-rpc)`

```bash
rg -n --pcre2 "(?i)(timeZone|refreshIntervalMs|SessionHeader|mixed|@deepseek\\-ai/dsh\\-time\\-context|packages/context/time\\-context/|agent/pre\\-step|user\\-rpc)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0482. Explicit Schedule time-zone boundary](0482-explicit-schedule-time-zone-boundary.md): The source note links to this decision directly.
- **`shares-code-with`** — [0566. Optional time-context plugin](0566-optional-time-context-plugin.md): Shares source implementation: `packages/context/time-context`, `packages/context/time-context/README.md`.
- **`shares-code-with`** — [0257. Durable Session-local reminders](0257-durable-session-local-reminders.md): Shares source implementation: `packages/context/time-context/src/index.ts`, `packages/context/time-context/src/invariant.ts`.
- **`shares-code-with`** — [0535. Drop durable step boundary events](0535-drop-durable-step-boundary-events.md): Shares source implementation: `packages/context/time-context/src/invariant.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): Shares source implementation: `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0265. Settlement delivery belongs to the continuation manager](0265-settlement-delivery-belongs-to-the-continuation-manager.md): Shares source implementation: `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0035. Agent-scope runtime design and correctness](0035-agent-scope-runtime-design-and-correctness.md): Shares source implementation: `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): Shares source implementation: `packages/core/session/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0156-durable-per-step-time-context.md`.
