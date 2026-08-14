---
id: "dsh-note-0257"
title: "Durable Session-local reminders"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-05-durable-web-schedule.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "jobs"
  - "examples/web-schedule"
  - "@deepseek-ai/dsh-time-context"
  - "@deepseek-ai/dsh-schedule"
  - "session-local"
  - "schedule/change"
  - "seedLength"
  - "SessionHeader.seedLength"
  - "after_seconds"
  - "{ id, kind: 'after', prompt, afterSeconds, scheduledAt }"
  - "."
  - ". Tool values derive"
  - "ctx.sessions.flush"
  - "persistence_uncertain"
search_regex: "(?i)(jobs|examples/web\\-schedule|@deepseek\\-ai/dsh\\-time\\-context|@deepseek\\-ai/dsh\\-schedule|session\\-local|schedule/change|seedLength|SessionHeader\\.seedLength)"
---

# 0257. Durable Session-local reminders — implementation context

## Open this when

A reminder created inside a conversation must remain attributable to that exact Session and survive a process restart. A process-local timer or inbox item cannot provide that durability, while a global scheduler or private database introduces a second identity, persistence, and lifecycle system. Busy Agents, long waits, wall-clock changes, cold Sessions, forks, persistence failures, absolute calendar input, and teardown make a simple timeout insufficient.

## Source decision

The examples/web-schedule overlay explicitly loads @deepseek-ai/dsh-time-context and @deepseek-ai/dsh-schedule; the default Web tree remains unchanged. Schedule observes only root Agents published after the plugin loads and installs its three tools plus one disposable owner in that Agent scope. Cold history reads, already-published roots, child Agents, and other hosts do not activate it. The user-visible boundary is session-local: the original Session runs an on-time reminder only while live, does no external notification while cold, and processes an overdue reminder after it becomes live again.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-05-durable-web-schedule.md](../02-notes/implemented/feature/2026-08-05-durable-web-schedule.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-05-durable-web-schedule.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-05-durable-web-schedule.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`examples/web-schedule/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/web-schedule/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `examples/web-schedule`. The source note names this file directly. | `named-directory-member, named-file` |
| [`packages/jobs/jobs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/jobs/jobs`. | `named-package-member` |
| [`packages/jobs/jobs/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/jobs/jobs`. | `named-package-member` |
| [`packages/jobs/jobs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/jobs/jobs`. | `named-package-member` |
| [`packages/schedule/schedule/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/index.ts) | package entry point | Core file in the package named by the note: `packages/schedule/schedule`. | `named-package-member` |
| [`packages/schedule/schedule/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/schedule/schedule`. | `named-package-member` |
| [`packages/context/time-context/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/index.ts) | package entry point | Core file in the package named by the note: `packages/context/time-context`. | `named-package-member` |
| [`packages/schedule/schedule/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/schedule/schedule`. | `named-package-member` |
| [`packages/context/time-context/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/context/time-context`. | `named-package-member` |
| [`examples/web-schedule`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/examples/web-schedule) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/jobs/jobs`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/schedule/schedule`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `jobs` | `const` | [`packages/jobs/tool-jobs/src/index.ts:356`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L356) | `const jobs = ctx.jobs.list(exec.agent)` |

### Tests and executable evidence

- [`packages/schedule/schedule/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/tests/tools.spec.ts) — A test under the owning area exercises or imports `session-local`. A test under the owning area exercises or imports `after_seconds`.
- [`packages/schedule/schedule/tests/plugin.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/tests/plugin.spec.ts) — A test under the owning area exercises or imports `session-local`. A test under the owning area exercises or imports `after_seconds`.
- [`packages/schedule/schedule/tests/domain.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/tests/domain.spec.ts) — A test under the owning area exercises or imports `session-local`. A test under the owning area exercises or imports `time_zone`.
- [`packages/schedule/schedule/tests/runtime.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/tests/runtime.spec.ts) — A test under the owning area exercises or imports `scheduledAt`. A test under the owning area exercises or imports `acceptedAt`.
- [`packages/schedule/schedule/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/tests/invariant.spec.ts) — A test under the owning area exercises or imports `scheduledAt`. A test under the owning area exercises or imports `acceptedAt`.
- [`packages/schedule/schedule/tests/recurrence.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/tests/recurrence.spec.ts) — A test under the owning area exercises or imports `scheduledAt`. A test under the owning area exercises or imports `acceptedAt`.
- [`apps/web/tests/schedule-after.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/schedule-after.e2e.ts) — Contains the exact code literal `schedule/change` named by the note.
- Source verification intent: Package tests pin strict replay, one-shot and Every transitions, creation-anchor arithmetic, latest-only catch-up, multi-record batching, fork suffixes, id reuse, offset and local-calendar profiles, IANA validation, daylight-saving gaps and overlaps, time bounds, timer segmentation, wall-clock movement, overdue admission, fixed framing, enqueue and append failures, barrier recovery, registration rollback, and quiescent disposal at per-file 100% coverage. A property test compares Every calculation and replay across varied intervals and skipped spans.

## How to read the implementation

1. Start with [`examples/web-schedule/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/web-schedule/README.md) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/jobs-tasks`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `jobs`, `examples/web-schedule`, `@deepseek-ai/dsh-time-context`, `@deepseek-ai/dsh-schedule`, `session-local`, `schedule/change`, `seedLength`, `SessionHeader.seedLength`, `after_seconds`, `{ id, kind: 'after', prompt, afterSeconds, scheduledAt }`, `.`, `. Tool values derive`, `ctx.sessions.flush`, `persistence_uncertain`
- Regex: `(?i)(jobs|examples/web\-schedule|@deepseek\-ai/dsh\-time\-context|@deepseek\-ai/dsh\-schedule|session\-local|schedule/change|seedLength|SessionHeader\.seedLength)`

```bash
rg -n --pcre2 "(?i)(jobs|examples/web\\-schedule|@deepseek\\-ai/dsh\\-time\\-context|@deepseek\\-ai/dsh\\-schedule|session\\-local|schedule/change|seedLength|SessionHeader\\.seedLength)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0480. Bounded fixed-rate Schedule](0480-bounded-fixed-rate-schedule.md): The source note links to this decision directly.
- **`source-link`** — [0481. Conversational Schedule delivery](0481-conversational-schedule-delivery.md): The source note links to this decision directly.
- **`source-link`** — [0482. Explicit Schedule time-zone boundary](0482-explicit-schedule-time-zone-boundary.md): The source note links to this decision directly.
- **`shares-code-with`** — [0015. The background job runtime (`ctx.jobs`) and generic task control tools](0015-the-background-job-runtime-ctx-jobs-and-generic-task-control-tools.md): Shares source implementation: `packages/jobs/jobs/src/index.ts`, `packages/jobs/jobs/src/invariant.ts`.
- **`shares-code-with`** — [0158. persistent PTY sessions](0158-persistent-pty-sessions.md): Shares source implementation: `packages/jobs/jobs/src/index.ts`, `packages/jobs/jobs/src/types.ts`.
- **`shares-code-with`** — [0134. Subagent capability seam](0134-subagent-capability-seam.md): Shares source implementation: `packages/jobs/jobs/src/index.ts`, `packages/jobs/jobs/src/invariant.ts`.
- **`shares-code-with`** — [0066. The subprocess service is its own seam under the bash executors (`dsh-subprocess` / `dsh-subprocess-local`)](0066-the-subprocess-service-is-its-own-seam-under-the-bash-executors-dsh-subp.md): Shares source implementation: `packages/jobs/jobs/src/index.ts`, `packages/jobs/jobs/src/invariant.ts`.
- **`shares-code-with`** — [0496. Real-API e2e in CI against the external DeepSeek API](0496-real-api-e2e-in-ci-against-the-external-deepseek-api.md): Shares source implementation: `packages/schedule/schedule/src/index.ts`, `packages/schedule/schedule/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0257-durable-session-local-reminders.md`.
