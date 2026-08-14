---
id: "dsh-note-0386"
title: "Documentation structure, tiers, and budgets"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-04-doc-tiers-and-budgets.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
aliases:
  - "dsh plugin --profile"
  - "doc-sync"
  - "wc -w"
  - "AGENTS.md"
  - "architecture.md"
  - "packages/README.md"
  - "docs/testing.md"
  - "docs/defensive-patterns.md"
  - "packages/AGENTS.md"
  - "docs/AGENTS.md"
  - "docs/"
  - "Documentation structure, tiers, and budgets"
  - "process"
  - "boundary"
search_regex: "(?i)(dsh[- ]plugin[- ]\\-\\-profile|doc\\-sync|wc[- ]\\-w|AGENTS\\.md|architecture\\.md|packages/README\\.md|docs/testing\\.md|docs/defensive\\-patterns\\.md)"
---

# 0386. Documentation structure, tiers, and budgets — implementation context

## Open this when

Standing docs accumulated repeated rules, retold incidents, duplicated package maps, and stale Agent Note summaries despite existing writing guidance. That guidance also did not define how a document's place in the hierarchy limits its scope or how ordered teaching differs from lookup-oriented material. Because review alone did not prevent that growth, the repository needed a mechanical budget alongside its documentation taxonomy.

## Source decision

Structure follows the documentation tree. docs/AGENTS.md is the documentation standard: a document owns detail about its subject, summarizes only the purpose, responsibility, and high-level behavior of direct children, and links to deeper owners. Agent Notes remain outside this structural contract. Every human-facing document is a tutorial with an ordered outcome or a reference with an explicit lookup scope; a postmortem is an incident-scoped reference whose chronology records evidence. Tutorials introduce concepts in prerequisite order for the reader's starting knowledge. A tier taxonomy with one home per fact.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-04-doc-tiers-and-budgets.md](../02-notes/implemented/process/2026-07-04-doc-tiers-and-budgets.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-04-doc-tiers-and-budgets.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-04-doc-tiers-and-budgets.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/AGENTS.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `packages/README.md` named by the note. | `exact-code-occurrence, named-file` |
| [`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/AGENTS.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/postmortem/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/postmortem/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/defensive-patterns.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/defensive-patterns.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`scripts/verify-doc-budgets.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-doc-budgets.ts) | repository automation | The source note names this file directly. Contains the exact code literal `docs/AGENTS.md` named by the note. | `exact-code-occurrence, named-file` |
| [`scripts/doc-budgets.manifest.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/doc-budgets.manifest.json) | repository automation | The source note names this file directly. Contains the exact code literal `packages/README.md` named by the note. | `exact-code-occurrence, named-file` |
| [`.agents/skills/dsh-doc-standards/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-doc-standards/SKILL.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`.agents/skills/dsh-translate-docs/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-translate-docs/SKILL.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`.agents/skills/dsh-doc-standards`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-doc-standards) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`CLAUDE.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/CLAUDE.md) | package contract and examples | Contains the exact code literal `packages/README.md` named by the note. Contains the exact code literal `docs/testing.md` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`scripts/translation-pairing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing.spec.ts) — Contains the exact code literal `packages/README.md` named by the note.

## How to read the implementation

1. Start with [`docs/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/AGENTS.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`
- Aliases: `dsh plugin --profile`, `doc-sync`, `wc -w`, `AGENTS.md`, `architecture.md`, `packages/README.md`, `docs/testing.md`, `docs/defensive-patterns.md`, `packages/AGENTS.md`, `docs/AGENTS.md`, `docs/`, `Documentation structure, tiers, and budgets`, `process`, `boundary`
- Regex: `(?i)(dsh[- ]plugin[- ]\-\-profile|doc\-sync|wc[- ]\-w|AGENTS\.md|architecture\.md|packages/README\.md|docs/testing\.md|docs/defensive\-patterns\.md)`

```bash
rg -n --pcre2 "(?i)(dsh[- ]plugin[- ]\\-\\-profile|doc\\-sync|wc[- ]\\-w|AGENTS\\.md|architecture\\.md|packages/README\\.md|docs/testing\\.md|docs/defensive\\-patterns\\.md)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0687. Agent Notes](0687-agent-notes.md): The source note links to this decision directly.
- **`source-link`** — [0377. Mechanical quality gates over prose guidelines](0377-mechanical-quality-gates-over-prose-guidelines.md): The source note links to this decision directly.
- **`shares-code-with`** — [0430. Lightweight routine documentation translation](0430-lightweight-routine-documentation-translation.md): Shares source implementation: `.agents/skills/dsh-translate-docs/SKILL.md`, `docs/AGENTS.md`.
- **`shares-code-with`** — [0561. Plan mode --- a logged per-agent session mode](0561-plan-mode-a-logged-per-agent-session-mode.md): Shares source implementation: `docs/defensive-patterns.md`, `packages/README.md`.
- **`shares-code-with`** — [0630. Doc-sync enforcement](0630-doc-sync-enforcement.md): Shares source implementation: `docs/AGENTS.md`, `packages/AGENTS.md`.
- **`shares-code-with`** — [0398. GUI testing system --- the three-tier structure](0398-gui-testing-system-the-three-tier-structure.md): Shares source implementation: `docs/testing.md`.
- **`shares-code-with`** — [0384. Bilingual documentation via paired sibling files and a pairing gate](0384-bilingual-documentation-via-paired-sibling-files-and-a-pairing-gate.md): Shares source implementation: `.agents/skills/dsh-translate-docs/SKILL.md`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `docs/testing.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0386-documentation-structure-tiers-and-budgets.md`.
