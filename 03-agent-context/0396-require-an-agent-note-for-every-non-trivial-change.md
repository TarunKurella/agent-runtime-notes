---
id: "dsh-note-0396"
title: "Require an Agent Note for every non-trivial change"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-19-require-agent-notes-for-non-trivial-changes.md"
implementation_evidence: "lead-only"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "AGENTS.md"
  - "superseded/"
  - "Require an Agent Note for every non-trivial change"
  - "process"
  - "boundary"
  - "compatibility"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "simplification"
  - "build release"
  - "session state"
  - "storage"
  - "testing"
search_regex: "(?i)(AGENTS\\.md|superseded/|Require[- ]an[- ]Agent[- ]Note[- ]for[- ]every[- ]non\\-trivial[- ]change|boundary|compatibility|evidence|lifecycle|ownership)"
---

# 0396. Require an Agent Note for every non-trivial change — implementation context

## Open this when

A selective threshold based on whether a decision seems durable, contested, and surprising lets substantial changes land without preserving their rationale. Code and tests show what changed, but they cannot consistently preserve why an approach won, which alternatives lost, or what costs maintainers accepted.

## Source decision

Every non-trivial change adds or updates at least one Agent Note in the same PR. Non-trivial changes include behavior, architecture, cross-file or cross-package contracts, process or tooling, testing strategy, on-disk, wire, or configuration formats, and other decisions a maintainer may reasonably revisit. Updating the note that already owns a decision satisfies the rule; a new note is required only when no note owns it. Purely mechanical or local edits with no behavioral, contractual, structural, process, or rationale change are exempt.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-19-require-agent-notes-for-non-trivial-changes.md](../02-notes/implemented/process/2026-07-19-require-agent-notes-for-non-trivial-changes.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-19-require-agent-notes-for-non-trivial-changes.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-19-require-agent-notes-for-non-trivial-changes.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/change-scope.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/change-scope.ts) | repository automation | Path shares title concepts: change. | `title-path-lead` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/change-scope.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/change-scope.spec.ts) — Path shares title concepts: change.
- [`apps/web/tests/snapshots/schedule-after/every-conversation.expected.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/schedule-after/every-conversation.expected.md) — Path shares title concepts: every.
- [`examples/headless-agent/tests/workspace-context-resume-snapshots/precedence-change/session.expected.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/headless-agent/tests/workspace-context-resume-snapshots/precedence-change/session.expected.jsonl) — Path shares title concepts: change.

## How to read the implementation

1. Start with [`scripts/change-scope.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/change-scope.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/simplification`, `domain/build-release`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `AGENTS.md`, `superseded/`, `Require an Agent Note for every non-trivial change`, `process`, `boundary`, `compatibility`, `evidence`, `lifecycle`, `ownership`, `simplification`, `build release`, `session state`, `storage`, `testing`
- Regex: `(?i)(AGENTS\.md|superseded/|Require[- ]an[- ]Agent[- ]Note[- ]for[- ]every[- ]non\-trivial[- ]change|boundary|compatibility|evidence|lifecycle|ownership)`

```bash
rg -n --pcre2 "(?i)(AGENTS\\.md|superseded/|Require[- ]an[- ]Agent[- ]Note[- ]for[- ]every[- ]non\\-trivial[- ]change|boundary|compatibility|evidence|lifecycle|ownership)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0687. Agent Notes](0687-agent-notes.md): The source note links to this decision directly.
- **`same-design-pressure`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.
- **`same-design-pressure`** — [0356. Filesystem absence is an observation and guarded creation never replaces](0356-filesystem-absence-is-an-observation-and-guarded-creation-never-replaces.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.
- **`same-design-pressure`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.
- **`same-design-pressure`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.
- **`same-design-pressure`** — [0512. Storage root placement and derived-medium recovery](0512-storage-root-placement-and-derived-medium-recovery.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.
- **`same-design-pressure`** — [0514. Cordis Host/Client Dynamic Plugin Runtime](0514-cordis-host-client-dynamic-plugin-runtime.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0396-require-an-agent-note-for-every-non-trivial-change.md`.
