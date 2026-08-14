---
id: "dsh-note-0480"
title: "Bounded fixed-rate Schedule"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-08-09-bounded-fixed-rate-schedule.md"
implementation_evidence: "lead-only"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
aliases:
  - "every_seconds"
  - "acceptedAt"
  - "lastRecurringAcceptedAt"
  - "deliveryNotBefore"
  - "after_seconds"
  - "Bounded fixed-rate Schedule"
  - "simplification"
  - "boundary"
  - "compatibility"
  - "discovery routing"
  - "evidence"
  - "ownership"
  - "schema types"
  - "build release"
search_regex: "(?i)(every_seconds|acceptedAt|lastRecurringAcceptedAt|deliveryNotBefore|after_seconds|Bounded[- ]fixed\\-rate[- ]Schedule|simplification|boundary)"
---

# 0480. Bounded fixed-rate Schedule — implementation context

## Open this when

Users need simple repeating reminders, but the initial recurrence layer of durable Session-local reminders treated fixed intervals and calendar expressions as one general subsystem. It added a Cron language and evaluator, time-zone-sensitive occurrence search, tzdata replay rules, a cross-record 300-second admission gate, persisted gate evidence, deferred-delivery fields, and gate-exhaustion states. Those mechanisms enlarged the durable protocol and live owner even when the requested behavior was only "repeat every N seconds." A cold or busy Session also cannot usefully replay every missed interval.

## Source decision

The retained recurring selector is only every_seconds, a safe integer of at least 300. Creation stores the first target at creation time plus the interval. Each dispatch stores the record id and one wall-clock acceptedAt; pure integer arithmetic selects the latest creation-anchor-aligned occurrence at or before that decision and advances directly to the first aligned target after it. No missed occurrences are enumerated, persisted, or replayed. When no one-shot is due, every distinct overdue Every record participates in one follow-up batch in target and creation order.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-08-09-bounded-fixed-rate-schedule.md](../02-notes/implemented/simplification/2026-08-09-bounded-fixed-rate-schedule.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-08-09-bounded-fixed-rate-schedule.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-08-09-bounded-fixed-rate-schedule.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

No implementation path could be confirmed from the pinned tree. Use the search handles below; do not invent a source location.

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`apps/web/tests/schedule-after.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/schedule-after.e2e.ts) — A test under the owning area exercises or imports `acceptedAt`. A test under the owning area exercises or imports `after_seconds`.
- [`packages/schedule/schedule/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/tests/tools.spec.ts) — A test under the owning area exercises or imports `every_seconds`. A test under the owning area exercises or imports `after_seconds`.
- [`packages/schedule/schedule/tests/domain.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/tests/domain.spec.ts) — A test under the owning area exercises or imports `acceptedAt`.
- [`packages/schedule/schedule/tests/plugin.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/tests/plugin.spec.ts) — A test under the owning area exercises or imports `after_seconds`.
- [`packages/schedule/schedule/tests/runtime.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/tests/runtime.spec.ts) — A test under the owning area exercises or imports `acceptedAt`.
- [`packages/schedule/schedule/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/tests/invariant.spec.ts) — A test under the owning area exercises or imports `acceptedAt`.
- [`packages/schedule/schedule/tests/recurrence.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/tests/recurrence.spec.ts) — A test under the owning area exercises or imports `acceptedAt`.
- Source verification intent: Strict decoder and invariant tests reject unsupported rule and dispatch shapes. Domain and property tests prove minimum-frequency validation, creation-anchor arithmetic, latest-only selection, advancement, and range exhaustion. Runtime tests prove one-shot priority, one shared batch for all overdue Every records, one occurrence per record, fixed ordering, and no immediate backlog loop. The assembled Web snapshot proves a two-record overdue batch becomes one ordinary assistant response with two same-time durable transitions and no Schedule UI sidecar.

## How to read the implementation

1. Start from the source note and run the regex below across the pinned repository.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/schema-types`, `domain/build-release`, `domain/jobs-tasks`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`
- Aliases: `every_seconds`, `acceptedAt`, `lastRecurringAcceptedAt`, `deliveryNotBefore`, `after_seconds`, `Bounded fixed-rate Schedule`, `simplification`, `boundary`, `compatibility`, `discovery routing`, `evidence`, `ownership`, `schema types`, `build release`
- Regex: `(?i)(every_seconds|acceptedAt|lastRecurringAcceptedAt|deliveryNotBefore|after_seconds|Bounded[- ]fixed\-rate[- ]Schedule|simplification|boundary)`

```bash
rg -n --pcre2 "(?i)(every_seconds|acceptedAt|lastRecurringAcceptedAt|deliveryNotBefore|after_seconds|Bounded[- ]fixed\\-rate[- ]Schedule|simplification|boundary)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0257. Durable Session-local reminders](0257-durable-session-local-reminders.md): The source note links to this decision directly.
- **`shares-code-with`** — [0482. Explicit Schedule time-zone boundary](0482-explicit-schedule-time-zone-boundary.md): Shares source implementation: `apps/web/tests/schedule-after.e2e.ts`, `packages/schedule/schedule/tests/domain.spec.ts`.
- **`shares-code-with`** — [0508. Required cancellation through tool-reachable capability seams](0508-required-cancellation-through-tool-reachable-capability-seams.md): Shares source implementation: `apps/web/tests/schedule-after.e2e.ts`.
- **`shares-code-with`** — [0598. Address pending queue occurrences for edit and removal](0598-address-pending-queue-occurrences-for-edit-and-removal.md): Shares source implementation: `apps/web/tests/schedule-after.e2e.ts`.
- **`same-design-pressure`** — [0363. Bounded background job admission](0363-bounded-background-job-admission.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/evidence`.
- **`same-design-pressure`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0399. Serial cross-platform CI reference](0399-serial-cross-platform-ci-reference.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0158. persistent PTY sessions](0158-persistent-pty-sessions.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0480-bounded-fixed-rate-schedule.md`.
