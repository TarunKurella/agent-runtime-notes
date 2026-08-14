---
id: "dsh-note-0686"
title: "AGENTS.md --- Agent Notes"
status: "root"
class: "root"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/AGENTS.md"
implementation_evidence: "lead-only"
target_anchor: "decision-note governance and retrieval"
tags:
  - "class/root"
  - "concern/evidence"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/root"
aliases:
  - "dsh-archive-agent-notes"
  - "archived/"
  - "AGENTS.md --- Agent Notes"
  - "root"
  - "evidence"
  - "storage"
  - "testing"
search_regex: "(?i)(dsh\\-archive\\-agent\\-notes|archived/|AGENTS\\.md[- ]\\-\\-\\-[- ]Agent[- ]Notes|root|evidence|storage|testing|AGENTS)"
---

# 0686. AGENTS.md --- Agent Notes — implementation context

## Open this when

Agent Notes are effectively RFCs written by agents: durable proposals and decision records that preserve rationale, alternatives, consequences, and required verification. Follow the documentation standard and the Agent Note rules. Every new Agent Note triggers a supersession check. Search the active tree for older notes covering the same decision or mechanism, classify any full or partial supersession with dsh-archive-agent-notes, and archive every qualifying implemented triplet in the same PR. Keep partial supersessions active and cross-linked.

## Source decision

Keep design decisions in one predictable tree, mark their lifecycle clearly, and verify the format in automation. The note explains why a choice was made; code and tests show what actually ships.

## Decision status

Governance for the note system itself. Apply it to documentation and decision maintenance, not automatically to the runtime.

- Raw note: [AGENTS.md](../02-notes/AGENTS.md)
- Pinned source: [.agents/notes/AGENTS.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/AGENTS.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/AGENTS.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`.agents/skills/dsh-archive-agent-notes/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-archive-agent-notes/SKILL.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `dsh-archive-agent-notes` named by the note. | `exact-code-occurrence, named-file` |
| [`.agents/skills/dsh-find-simplifications/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-find-simplifications/SKILL.md) | package contract and examples | Contains the exact code literal `dsh-archive-agent-notes` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- Use the named note-format and archive verification scripts above. Do not treat unrelated product tests as evidence for note governance.

## How to read the implementation

1. Start with [`docs/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/AGENTS.md) because it has the strongest evidence link to the note.
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

1. Keep the lifecycle and class folders stable.
2. Give each decision one canonical note and preserve its history when its status changes.
3. Add format checks that reject missing dates, status metadata, or broken links.
4. Keep search tags and aliases in generated context; keep the raw note unchanged.
5. Change runtime code only when a specific decision note calls for it.

## Search handles

- Tags: `class/root`, `concern/evidence`, `domain/storage`, `domain/testing`, `lifecycle/root`
- Aliases: `dsh-archive-agent-notes`, `archived/`, `AGENTS.md --- Agent Notes`, `root`, `evidence`, `storage`, `testing`
- Regex: `(?i)(dsh\-archive\-agent\-notes|archived/|AGENTS\.md[- ]\-\-\-[- ]Agent[- ]Notes|root|evidence|storage|testing|AGENTS)`

```bash
rg -n --pcre2 "(?i)(dsh\\-archive\\-agent\\-notes|archived/|AGENTS\\.md[- ]\\-\\-\\-[- ]Agent[- ]Notes|root|evidence|storage|testing|AGENTS)" source-deepseek-harness
rg -l --fixed-strings "class/root" 03-agent-context
```

## Connected notes

- **`source-link`** — [0687. Agent Notes](0687-agent-notes.md): The source note links to this decision directly.
- **`source-link`** — [0685. AGENTS.md --- Archived Agent Notes](0685-agents-md-archived-agent-notes.md): The source note links to this decision directly.
- **`shares-code-with`** — [0506. AGENTS.md --- Implemented Agent Notes](0506-agents-md-implemented-agent-notes.md): Shares source implementation: `.agents/skills/dsh-archive-agent-notes/SKILL.md`, `.agents/skills/dsh-find-simplifications/SKILL.md`.
- **`shares-code-with`** — [0409. Freeze low-future-value Agent Notes outside the active corpus](0409-freeze-low-future-value-agent-notes-outside-the-active-corpus.md): Shares source implementation: `.agents/skills/dsh-archive-agent-notes/SKILL.md`, `.agents/skills/dsh-find-simplifications/SKILL.md`.
- **`shares-code-with`** — [0434. Cite committed artifacts, never design-session ordinals](0434-cite-committed-artifacts-never-design-session-ordinals.md): Shares source implementation: `docs/AGENTS.md`.
- **`shares-code-with`** — [0436. verify-md-links validates fragment anchors, closing the last dead-link class](0436-verify-md-links-validates-fragment-anchors-closing-the-last-dead-link-cl.md): Shares source implementation: `docs/AGENTS.md`.
- **`shares-code-with`** — [0430. Lightweight routine documentation translation](0430-lightweight-routine-documentation-translation.md): Shares source implementation: `docs/AGENTS.md`.
- **`shares-code-with`** — [0408. Prefer maintained dependencies over hand-rolling](0408-prefer-maintained-dependencies-over-hand-rolling.md): Shares source implementation: `.agents/skills/dsh-find-simplifications/SKILL.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0686-agents-md-agent-notes.md`.
