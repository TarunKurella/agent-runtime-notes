---
id: "dsh-note-0410"
title: "Retarget PR bases incrementally"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-26-incremental-pr-base-retargeting.md"
implementation_evidence: "lead-only"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/implemented"
aliases:
  - "Retarget PR bases incrementally"
  - "process"
  - "ownership"
  - "recovery"
  - "schema types"
  - "session state"
  - "storage"
  - "implemented"
search_regex: "(?i)(Retarget[- ]PR[- ]bases[- ]incrementally|ownership|recovery|schema[- ]types|session[- ]state|storage|implemented|AGENTS)"
---

# 0410. Retarget PR bases incrementally — implementation context

## Open this when

A PR base can advance while its current tip is being merged into the PR branch. Restarting from the newer tip discards completed conflict resolution and validation. Rewriting a merge that is already pushed also erases reviewable history.

## Source decision

When merge-forward is chosen, each observed base tip gets its own merge checkpoint. If the base advances during the work, finish and validate the merge already in progress, commit it, and push it when the task authorizes a push. Only then fetch and merge the newer base in a separate merge commit. Do not abandon or rewrite a checkpoint within that merge-forward sequence. The native-stack and optional-rebase decision also permits a lease-protected rebase for standalone or stacked PRs, including after review. This note owns the merge-forward path only.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-26-incremental-pr-base-retargeting.md](../02-notes/implemented/process/2026-07-26-incremental-pr-base-retargeting.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-26-incremental-pr-base-retargeting.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-26-incremental-pr-base-retargeting.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`.agents/skills/dsh-merging-stacked-prs/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-merging-stacked-prs/SKILL.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/cookbook/responding-to-pr-review-on-a-stack.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cookbook/responding-to-pr-review-on-a-stack.md) | package contract and examples | The source note names this file directly. | `named-file` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.

## How to read the implementation

1. Start with [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/session-state`, `domain/storage`, `lifecycle/implemented`
- Aliases: `Retarget PR bases incrementally`, `process`, `ownership`, `recovery`, `schema types`, `session state`, `storage`, `implemented`
- Regex: `(?i)(Retarget[- ]PR[- ]bases[- ]incrementally|ownership|recovery|schema[- ]types|session[- ]state|storage|implemented|AGENTS)`

```bash
rg -n --pcre2 "(?i)(Retarget[- ]PR[- ]bases[- ]incrementally|ownership|recovery|schema[- ]types|session[- ]state|storage|implemented|AGENTS)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0422. Native GitHub stacks and optional PR rebases](0422-native-github-stacks-and-optional-pr-rebases.md): The source note links to this decision directly.
- **`shares-code-with`** — [0228. Code Mode language dispatch and the Python SDK renderer](0228-code-mode-language-dispatch-and-the-python-sdk-renderer.md): Shares source implementation: `AGENTS.md`.
- **`shares-code-with`** — [0221. Read card --- the read tool's structured line window reaches the client](0221-read-card-the-read-tool-s-structured-line-window-reaches-the-client.md): Shares source implementation: `AGENTS.md`.
- **`shares-code-with`** — [0021. Event-domain semantics --- session is the fact log, agent is the live event channel](0021-event-domain-semantics-session-is-the-fact-log-agent-is-the-live-event-c.md): Shares source implementation: `AGENTS.md`.
- **`shares-code-with`** — [0446. Drop the mutable session summary](0446-drop-the-mutable-session-summary.md): Shares source implementation: `AGENTS.md`.
- **`shares-code-with`** — [0651. Drop unconsumed assembled LLM convenience surfaces](0651-drop-unconsumed-assembled-llm-convenience-surfaces.md): Shares source implementation: `AGENTS.md`.
- **`shares-code-with`** — [0023. Tagged render-intent union for tool-call presentation](0023-tagged-render-intent-union-for-tool-call-presentation.md): Shares source implementation: `AGENTS.md`.
- **`shares-code-with`** — [0632. Generated cordis events + services catalog](0632-generated-cordis-events-services-catalog.md): Shares source implementation: `AGENTS.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0410-retarget-pr-bases-incrementally.md`.
