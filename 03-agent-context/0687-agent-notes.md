---
id: "dsh-note-0687"
title: "Agent Notes"
status: "root"
class: "root"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/README.md"
implementation_evidence: "high"
target_anchor: "decision-note governance and retrieval"
tags:
  - "class/root"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/root"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "{lifecycle}/{class}/yyyy-mm-dd-topic-title.md"
  - "proposed/"
  - "implemented/"
  - "rejected/"
  - "INDEX.md"
  - "archived/"
  - "scripts/agent-note-tree.ts"
  - "bug-fix"
  - "dsh-archive-agent-notes"
  - "archived/{class}/yyyy-mm-dd-topic-title.md"
  - "Archived: YYYY-MM-DD"
  - "verify-archived-agent-notes"
  - "pnpm run verify-agent-note-format"
  - "doc-sync"
search_regex: "(?i)(\\{lifecycle\\}/\\{class\\}/yyyy\\-mm\\-dd\\-topic\\-title\\.md|proposed/|implemented/|rejected/|INDEX\\.md|archived/|scripts/agent\\-note\\-tree\\.ts|bug\\-fix)"
---

# 0687. Agent Notes — implementation context

## Open this when

One kind of design doc lives here. An Agent Note records a decision or proposal that affects this codebase --- the why and what we gave up, the parts code and docs can't carry. This file defines where Agent Notes live, when to write one, and the in-file format.

## Source decision

Keep design decisions in one predictable tree, mark their lifecycle clearly, and verify the format in automation. The note explains why a choice was made; code and tests show what actually ships.

## Decision status

Governance for the note system itself. Apply it to documentation and decision maintenance, not automatically to the runtime.

- Raw note: [README.md](../02-notes/README.md)
- Pinned source: [.agents/notes/README.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/README.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/AGENTS.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/i18n/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/i18n/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`scripts/agent-note-tree.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/agent-note-tree.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/verify-agent-note-format.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-agent-note-format.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/verify-archived-agent-notes.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-archived-agent-notes.ts) | repository automation | The source note names this file directly. | `named-file` |
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

- Tags: `class/root`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/root`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`
- Aliases: `{lifecycle}/{class}/yyyy-mm-dd-topic-title.md`, `proposed/`, `implemented/`, `rejected/`, `INDEX.md`, `archived/`, `scripts/agent-note-tree.ts`, `bug-fix`, `dsh-archive-agent-notes`, `archived/{class}/yyyy-mm-dd-topic-title.md`, `Archived: YYYY-MM-DD`, `verify-archived-agent-notes`, `pnpm run verify-agent-note-format`, `doc-sync`
- Regex: `(?i)(\{lifecycle\}/\{class\}/yyyy\-mm\-dd\-topic\-title\.md|proposed/|implemented/|rejected/|INDEX\.md|archived/|scripts/agent\-note\-tree\.ts|bug\-fix)`

```bash
rg -n --pcre2 "(?i)(\\{lifecycle\\}/\\{class\\}/yyyy\\-mm\\-dd\\-topic\\-title\\.md|proposed/|implemented/|rejected/|INDEX\\.md|archived/|scripts/agent\\-note\\-tree\\.ts|bug\\-fix)" source-deepseek-harness
rg -l --fixed-strings "class/root" 03-agent-context
```

## Connected notes

- **`source-link`** — [0685. AGENTS.md --- Archived Agent Notes](0685-agents-md-archived-agent-notes.md): The source note links to this decision directly.
- **`source-link`** — [0506. AGENTS.md --- Implemented Agent Notes](0506-agents-md-implemented-agent-notes.md): The source note links to this decision directly.
- **`source-link`** — [0382. Classify Agent Notes by kind via path-encoded subdirectories](0382-classify-agent-notes-by-kind-via-path-encoded-subdirectories.md): The source note links to this decision directly.
- **`source-link`** — [0387. One gated in-file format for Agent Notes](0387-one-gated-in-file-format-for-agent-notes.md): The source note links to this decision directly.
- **`shares-code-with`** — [0433. Standardize Chinese contract terminology on 约定](0433-standardize-chinese-contract-terminology-on-contract.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `apps/web/tests/support.ts`.
- **`shares-code-with`** — [0406. Briefed minimal translation updates](0406-briefed-minimal-translation-updates.md): Shares source implementation: `docs/i18n/README.md`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0630. Doc-sync enforcement](0630-doc-sync-enforcement.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `docs/AGENTS.md`.
- **`shares-code-with`** — [0409. Freeze low-future-value Agent Notes outside the active corpus](0409-freeze-low-future-value-agent-notes-outside-the-active-corpus.md): Shares source implementation: `.agents/skills/dsh-archive-agent-notes/SKILL.md`, `scripts/verify-archived-agent-notes.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0687-agent-notes.md`.
