---
id: "dsh-note-0685"
title: "AGENTS.md --- Archived Agent Notes"
status: "archived"
class: "root"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/AGENTS.md"
implementation_evidence: "lead-only"
target_anchor: "decision-note governance and retrieval"
tags:
  - "class/root"
  - "concern/evidence"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "lifecycle/archived"
  - "mechanism/adapter"
aliases:
  - "Archived: YYYY-MM-DD"
  - "dsh-archive-agent-notes"
  - "pnpm run verify-archived-agent-notes --write"
  - "AGENTS.md --- Archived Agent Notes"
  - "root"
  - "evidence"
  - "recovery"
  - "simplification"
  - "build release"
  - "archived"
  - "adapter"
search_regex: "(?i)(Archived:[- ]YYYY\\-MM\\-DD|dsh\\-archive\\-agent\\-notes|pnpm[- ]run[- ]verify\\-archived\\-agent\\-notes[- ]\\-\\-write|AGENTS\\.md[- ]\\-\\-\\-[- ]Archived[- ]Agent[- ]Notes|root|evidence|recovery|simplification)"
---

# 0685. AGENTS.md --- Archived Agent Notes — implementation context

## Open this when

Archived Agent Note triplets under the kind directories are frozen historical snapshots, not current authority. Never edit, reformat, translate, repair, delete, or move a sealed artifact; use an active Agent Note or current documentation for new decisions and facts. The archival change may only relocate a complete English/Chinese/sidecar triplet, insert the identical Archived: YYYY-MM-DD line below both Status: implemented lines, re-record the sidecar, and repair or delete inbound links. Do not inspect, verify, or repair links out of archived notes.

## Source decision

The source note does not isolate a compact implementation decision; read it as a whole before changing code.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/AGENTS.md](../02-notes/archived/AGENTS.md)
- Pinned source: [.agents/notes/archived/AGENTS.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/AGENTS.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`.agents/skills/dsh-archive-agent-notes/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-archive-agent-notes/SKILL.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `dsh-archive-agent-notes` named by the note. | `exact-code-occurrence, named-file` |
| [`.agents/skills/dsh-find-simplifications/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-find-simplifications/SKILL.md) | package contract and examples | Contains the exact code literal `dsh-archive-agent-notes` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.

## How to read the implementation

1. Start with [`.agents/skills/dsh-archive-agent-notes/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-archive-agent-notes/SKILL.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** decision-note governance and retrieval.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/root`, `concern/evidence`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `lifecycle/archived`, `mechanism/adapter`
- Aliases: `Archived: YYYY-MM-DD`, `dsh-archive-agent-notes`, `pnpm run verify-archived-agent-notes --write`, `AGENTS.md --- Archived Agent Notes`, `root`, `evidence`, `recovery`, `simplification`, `build release`, `archived`, `adapter`
- Regex: `(?i)(Archived:[- ]YYYY\-MM\-DD|dsh\-archive\-agent\-notes|pnpm[- ]run[- ]verify\-archived\-agent\-notes[- ]\-\-write|AGENTS\.md[- ]\-\-\-[- ]Archived[- ]Agent[- ]Notes|root|evidence|recovery|simplification)`

```bash
rg -n --pcre2 "(?i)(Archived:[- ]YYYY\\-MM\\-DD|dsh\\-archive\\-agent\\-notes|pnpm[- ]run[- ]verify\\-archived\\-agent\\-notes[- ]\\-\\-write|AGENTS\\.md[- ]\\-\\-\\-[- ]Archived[- ]Agent[- ]Notes|root|evidence|recovery|simplification)" source-deepseek-harness
rg -l --fixed-strings "class/root" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0409. Freeze low-future-value Agent Notes outside the active corpus](0409-freeze-low-future-value-agent-notes-outside-the-active-corpus.md): Shares source implementation: `.agents/skills/dsh-archive-agent-notes/SKILL.md`, `.agents/skills/dsh-find-simplifications/SKILL.md`.
- **`shares-code-with`** — [0686. AGENTS.md --- Agent Notes](0686-agents-md-agent-notes.md): Shares source implementation: `.agents/skills/dsh-archive-agent-notes/SKILL.md`, `.agents/skills/dsh-find-simplifications/SKILL.md`.
- **`shares-code-with`** — [0506. AGENTS.md --- Implemented Agent Notes](0506-agents-md-implemented-agent-notes.md): Shares source implementation: `.agents/skills/dsh-archive-agent-notes/SKILL.md`, `.agents/skills/dsh-find-simplifications/SKILL.md`.
- **`shares-code-with`** — [0687. Agent Notes](0687-agent-notes.md): Shares source implementation: `.agents/skills/dsh-archive-agent-notes/SKILL.md`.
- **`shares-code-with`** — [0408. Prefer maintained dependencies over hand-rolling](0408-prefer-maintained-dependencies-over-hand-rolling.md): Shares source implementation: `.agents/skills/dsh-find-simplifications/SKILL.md`.
- **`same-design-pressure`** — [0651. Drop unconsumed assembled LLM convenience surfaces](0651-drop-unconsumed-assembled-llm-convenience-surfaces.md): Shares design concerns: `concern/evidence`, `concern/recovery`, `concern/simplification`.
- **`same-design-pressure`** — [0567. Dedicated full-screen TUI front door](0567-dedicated-full-screen-tui-front-door.md): Shares design concerns: `concern/evidence`, `concern/recovery`, `concern/simplification`.
- **`same-design-pressure`** — [0528. Prune dead public API and result fields](0528-prune-dead-public-api-and-result-fields.md): Shares design concerns: `concern/evidence`, `concern/recovery`, `concern/simplification`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0685-agents-md-archived-agent-notes.md`.
