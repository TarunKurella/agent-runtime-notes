---
id: "dsh-note-0506"
title: "AGENTS.md --- Implemented Agent Notes"
status: "implemented"
class: "root"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/AGENTS.md"
implementation_evidence: "lead-only"
target_anchor: "decision-note governance and retrieval"
tags:
  - "class/root"
  - "concern/evidence"
  - "concern/lifecycle"
  - "domain/session-state"
  - "lifecycle/implemented"
aliases:
  - "verify-agent-note-format"
  - "dsh-archive-agent-notes"
  - "AGENTS.md --- Implemented Agent Notes"
  - "root"
  - "evidence"
  - "lifecycle"
  - "session state"
  - "implemented"
search_regex: "(?i)(verify\\-agent\\-note\\-format|dsh\\-archive\\-agent\\-notes|AGENTS\\.md[- ]\\-\\-\\-[- ]Implemented[- ]Agent[- ]Notes|root|evidence|lifecycle|session[- ]state|implemented)"
---

# 0506. AGENTS.md --- Implemented Agent Notes — implementation context

## Open this when

These Agent Notes describe shipped decisions. Follow the root instructions, documentation standard, and Agent Note format; verify-agent-note-format gates the lifecycle-specific structure.

## Source decision

The source note does not isolate a compact implementation decision; read it as a whole before changing code.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/AGENTS.md](../02-notes/implemented/AGENTS.md)
- Pinned source: [.agents/notes/implemented/AGENTS.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/AGENTS.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/AGENTS.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`.agents/skills/dsh-archive-agent-notes/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-archive-agent-notes/SKILL.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `dsh-archive-agent-notes` named by the note. | `exact-code-occurrence, named-file` |
| [`.agents/skills/dsh-find-simplifications/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-find-simplifications/SKILL.md) | package contract and examples | Contains the exact code literal `dsh-archive-agent-notes` named by the note. | `exact-code-occurrence` |

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

**Likely integration point:** decision-note governance and retrieval.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/root`, `concern/evidence`, `concern/lifecycle`, `domain/session-state`, `lifecycle/implemented`
- Aliases: `verify-agent-note-format`, `dsh-archive-agent-notes`, `AGENTS.md --- Implemented Agent Notes`, `root`, `evidence`, `lifecycle`, `session state`, `implemented`
- Regex: `(?i)(verify\-agent\-note\-format|dsh\-archive\-agent\-notes|AGENTS\.md[- ]\-\-\-[- ]Implemented[- ]Agent[- ]Notes|root|evidence|lifecycle|session[- ]state|implemented)`

```bash
rg -n --pcre2 "(?i)(verify\\-agent\\-note\\-format|dsh\\-archive\\-agent\\-notes|AGENTS\\.md[- ]\\-\\-\\-[- ]Implemented[- ]Agent[- ]Notes|root|evidence|lifecycle|session[- ]state|implemented)" source-deepseek-harness
rg -l --fixed-strings "class/root" 03-agent-context
```

## Connected notes

- **`source-link`** — [0687. Agent Notes](0687-agent-notes.md): The source note links to this decision directly.
- **`shares-code-with`** — [0686. AGENTS.md --- Agent Notes](0686-agents-md-agent-notes.md): Shares source implementation: `.agents/skills/dsh-archive-agent-notes/SKILL.md`, `.agents/skills/dsh-find-simplifications/SKILL.md`.
- **`shares-code-with`** — [0409. Freeze low-future-value Agent Notes outside the active corpus](0409-freeze-low-future-value-agent-notes-outside-the-active-corpus.md): Shares source implementation: `.agents/skills/dsh-archive-agent-notes/SKILL.md`, `.agents/skills/dsh-find-simplifications/SKILL.md`.
- **`shares-code-with`** — [0430. Lightweight routine documentation translation](0430-lightweight-routine-documentation-translation.md): Shares source implementation: `AGENTS.md`, `docs/AGENTS.md`.
- **`shares-code-with`** — [0685. AGENTS.md --- Archived Agent Notes](0685-agents-md-archived-agent-notes.md): Shares source implementation: `.agents/skills/dsh-archive-agent-notes/SKILL.md`, `.agents/skills/dsh-find-simplifications/SKILL.md`.
- **`shares-code-with`** — [0021. Event-domain semantics --- session is the fact log, agent is the live event channel](0021-event-domain-semantics-session-is-the-fact-log-agent-is-the-live-event-c.md): Shares source implementation: `AGENTS.md`.
- **`shares-code-with`** — [0446. Drop the mutable session summary](0446-drop-the-mutable-session-summary.md): Shares source implementation: `AGENTS.md`.
- **`shares-code-with`** — [0434. Cite committed artifacts, never design-session ordinals](0434-cite-committed-artifacts-never-design-session-ordinals.md): Shares source implementation: `docs/AGENTS.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0506-agents-md-implemented-agent-notes.md`.
