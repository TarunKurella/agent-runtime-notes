---
id: "dsh-note-0482"
title: "Explicit Schedule time-zone boundary"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-08-09-explicit-schedule-time-zone.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/context"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
aliases:
  - "timeZone"
  - "Intl.DateTimeFormat().resolvedOptions().timeZone"
  - "Intl.DateTimeFormat"
  - "clientTimeZone"
  - "UTC"
  - "user-rpc"
  - "{ date, time, time_zone }"
  - "scheduledAt"
  - "time_zone"
  - "Asia/Shanghai"
  - "SessionHeader.timeZone"
  - "Explicit Schedule time-zone boundary"
  - "simplification"
  - "boundary"
search_regex: "(?i)(timeZone|Intl\\.DateTimeFormat\\(\\)\\.resolvedOptions\\(\\)\\.timeZone|Intl\\.DateTimeFormat|clientTimeZone|user\\-rpc|\\{[- ]date,[- ]time,[- ]time_zone[- ]\\}|scheduledAt|time_zone)"
---

# 0482. Explicit Schedule time-zone boundary — implementation context

## Open this when

Implicit local at input made a browser fact into shared product state. Capturing a default zone on Session creation required new Session headers, create/resume/fork conflict rules, JSONL metadata, a SQLite migration, client creation plumbing, Host comparisons, and Schedule logic coupled to time-context markers. Travel, concurrent tabs, missing provenance, and old Sessions then needed a confirmation protocol merely to decide whether an omitted field was safe. Most of that complexity sat outside Schedule.

## Source decision

Browser zone is request-local provenance. The Web client samples Intl.DateTimeFormat().resolvedOptions().timeZone for every prompt. The Host accepts an optional clientTimeZone, validates and canonicalizes UTC or an IANA Area/Location at the RPC boundary, and logs it on that exact user-rpc message. Invalid values reject prompt admission. Non-browser clients may omit it. Time-context derives unique, mixed, or missing browser facts from original user-rpc messages in the open turn. A unique zone formats the clock and tells the model to interpret otherwise-unqualified dates and times in that zone.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-08-09-explicit-schedule-time-zone.md](../02-notes/implemented/simplification/2026-08-09-explicit-schedule-time-zone.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-08-09-explicit-schedule-time-zone.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-08-09-explicit-schedule-time-zone.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/context/time-context/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/index.ts) | package entry point | Defines `timeZone`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/time-zone.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/time-zone.ts) | runtime implementation | Defines `timeZone`, a construct named by the note. | `symbol-definition` |
| [`packages/context/time-context/src/request-zone.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/request-zone.ts) | runtime implementation | Defines `timeZone`, a construct named by the note. | `symbol-definition` |
| [`.github/dependabot.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/dependabot.yml) | repository automation | Contains the exact code literal `Asia/Shanghai` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/e2e.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/e2e.yml) | repository automation | Contains the exact code literal `Asia/Shanghai` named by the note. | `exact-code-occurrence` |
| [`packages/context/time-context/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/README.md) | package contract and examples | Contains the exact code literal `Asia/Shanghai` named by the note. | `exact-code-occurrence` |
| [`packages/context/time-context/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/README.zh.md) | package contract and examples | Contains the exact code literal `Asia/Shanghai` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `timeZone` | `const` | [`packages/client/runtime/src/client/time-zone.ts:9`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/time-zone.ts#L9) | `const timeZone = new Intl.DateTimeFormat().resolvedOptions().timeZone` |
| `timeZone` | `const` | [`packages/context/time-context/src/index.ts:146`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/index.ts#L146) | `const timeZone = config.timeZone` |
| `timeZone` | `const` | [`packages/context/time-context/src/request-zone.ts:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/request-zone.ts#L52) | `const timeZone = browserTimeZone(message)` |

### Tests and executable evidence

- [`apps/web/tests/schedule-after.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/schedule-after.e2e.ts) — A test under the owning area exercises or imports `clientTimeZone`. A test under the owning area exercises or imports `user-rpc`.
- [`apps/web/tests/bash-abort-row.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/bash-abort-row.e2e.ts) — A test under the owning area exercises or imports `UTC`.
- [`packages/schedule/schedule/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/tests/tools.spec.ts) — A test under the owning area exercises or imports `UTC`. A test under the owning area exercises or imports `scheduledAt`.
- [`packages/schedule/schedule/tests/domain.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/tests/domain.spec.ts) — A test under the owning area exercises or imports `UTC`. A test under the owning area exercises or imports `scheduledAt`.
- [`packages/host/apiproxy/tests/rpc-schemas.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/rpc-schemas.spec.ts) — A test under the owning area exercises or imports `clientTimeZone`. Contains the exact code literal `Asia/Shanghai` named by the note.
- [`packages/schedule/schedule/tests/runtime.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/tests/runtime.spec.ts) — A test under the owning area exercises or imports `scheduledAt`.
- [`packages/schedule/schedule/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/tests/invariant.spec.ts) — A test under the owning area exercises or imports `scheduledAt`.
- [`packages/host/apiproxy/tests/api-proxy-cold.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-cold.spec.ts) — A test under the owning area exercises or imports `clientTimeZone`. A test under the owning area exercises or imports `UTC`.
- Source verification intent: Host tests pin canonical aliases, omission, and rejection before Agent entry. Client tests pin one browser-zone sample on each prompt. Time-context tests pin unique, mixed, and missing current-turn derivation and exact model policy. Schedule tests pin required time_zone, strict offsets, calendar validation, canonical zones, gap rejection, overlap-first selection, and absence of an implicit context path.

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

- Tags: `class/simplification`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/context`, `domain/jobs-tasks`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`
- Aliases: `timeZone`, `Intl.DateTimeFormat().resolvedOptions().timeZone`, `Intl.DateTimeFormat`, `clientTimeZone`, `UTC`, `user-rpc`, `{ date, time, time_zone }`, `scheduledAt`, `time_zone`, `Asia/Shanghai`, `SessionHeader.timeZone`, `Explicit Schedule time-zone boundary`, `simplification`, `boundary`
- Regex: `(?i)(timeZone|Intl\.DateTimeFormat\(\)\.resolvedOptions\(\)\.timeZone|Intl\.DateTimeFormat|clientTimeZone|user\-rpc|\{[- ]date,[- ]time,[- ]time_zone[- ]\}|scheduledAt|time_zone)`

```bash
rg -n --pcre2 "(?i)(timeZone|Intl\\.DateTimeFormat\\(\\)\\.resolvedOptions\\(\\)\\.timeZone|Intl\\.DateTimeFormat|clientTimeZone|user\\-rpc|\\{[- ]date,[- ]time,[- ]time_zone[- ]\\}|scheduledAt|time_zone)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0480. Bounded fixed-rate Schedule](0480-bounded-fixed-rate-schedule.md): Shares source implementation: `apps/web/tests/schedule-after.e2e.ts`, `packages/schedule/schedule/tests/domain.spec.ts`.
- **`shares-code-with`** — [0156. Durable per-step time context](0156-durable-per-step-time-context.md): Shares source implementation: `packages/context/time-context/src/index.ts`, `packages/context/time-context/src/request-zone.ts`.
- **`shares-code-with`** — [0566. Optional time-context plugin](0566-optional-time-context-plugin.md): Shares source implementation: `packages/context/time-context/src/index.ts`, `packages/context/time-context/src/request-zone.ts`.
- **`shares-code-with`** — [0257. Durable Session-local reminders](0257-durable-session-local-reminders.md): Shares source implementation: `packages/context/time-context/src/index.ts`.
- **`shares-code-with`** — [0598. Address pending queue occurrences for edit and removal](0598-address-pending-queue-occurrences-for-edit-and-removal.md): Shares source implementation: `apps/web/tests/schedule-after.e2e.ts`.
- **`shares-code-with`** — [0508. Required cancellation through tool-reachable capability seams](0508-required-cancellation-through-tool-reachable-capability-seams.md): Shares source implementation: `apps/web/tests/schedule-after.e2e.ts`.
- **`shares-code-with`** — [0296. Status-driven disclosure for workflow runs](0296-status-driven-disclosure-for-workflow-runs.md): Shares source implementation: `apps/web/tests/bash-abort-row.e2e.ts`, `apps/web/tests/schedule-after.e2e.ts`.
- **`same-design-pressure`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/concurrency`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0482-explicit-schedule-time-zone-boundary.md`.
