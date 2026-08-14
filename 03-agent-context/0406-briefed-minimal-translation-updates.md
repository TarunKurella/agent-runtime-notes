---
id: "dsh-note-0406"
title: "Briefed minimal translation updates"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-26-briefed-minimal-translation-updates.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "git cat-file"
  - "pnpm run gen-translation-brief [--apply] [pair...]"
  - "--apply"
  - "verify-translation-pairing [pair...]"
  - "doc-sync"
  - "--write"
  - "--write --all"
  - "git hash-object -w --stdin"
  - "refs/dsh/translation-pairing/snapshots/"
  - "--all"
  - "--write <pair>"
  - "--list"
  - "Briefed minimal translation updates"
  - "process"
search_regex: "(?i)(git[- ]cat\\-file|pnpm[- ]run[- ]gen\\-translation\\-brief[- ]\\[\\-\\-apply\\][- ]\\[pair\\.\\.\\.\\]|\\-\\-apply|verify\\-translation\\-pairing[- ]\\[pair\\.\\.\\.\\]|doc\\-sync|\\-\\-write|\\-\\-write[- ]\\-\\-all|git[- ]hash\\-object[- ]\\-w[- ]\\-\\-stdin)"
---

# 0406. Briefed minimal translation updates — implementation context

## Open this when

The bilingual pairing contract already prescribed minimal counterpart updates --- diff the edited side against its last-confirmed state, patch the counterpart, never re-translate --- but the committed workflow made every update pay whole-document overheads. The translating subagent loaded the full guidance corpus (skill, pairing contract, translation rules, the 192-line terminology table, style samples, prose standard) before touching a two-line diff; it re-derived the last-confirmed diff by hand through git cat-file; and each iteration re-ran the corpus-wide pairing gate, which parses every pair in the tree.

## Source decision

The extended manual workflow runs pair updates on a generated briefing instead of the guidance corpus; new pairs in that workflow still use the unchanged whole-document path. Routine agent work uses the direct path defined by the lightweight-translation decision. pnpm run gen-translation-brief [--apply] [pair...] (scripts/gen-translation-brief.ts, assembly in scripts/translation-brief.ts) prints, per out-of-sync pair, the authored side's diff from its recorded last-confirmed blob to the working tree plus the change mapped at the narrowest safely aligned granularity, deterministically widening on mapping.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-26-briefed-minimal-translation-updates.md](../02-notes/implemented/process/2026-07-26-briefed-minimal-translation-updates.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-26-briefed-minimal-translation-updates.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-26-briefed-minimal-translation-updates.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/translation-brief.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-brief.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/translation-prompt.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-prompt.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/gen-translation-brief.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-translation-brief.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`.agents/skills/dsh-translate-docs/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-translate-docs/SKILL.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/i18n/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/i18n/README.md) | package contract and examples | Contains the exact code literal `refs/dsh/translation-pairing/snapshots/` named by the note. | `exact-code-occurrence` |
| [`docs/i18n/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/i18n/README.zh.md) | package contract and examples | Contains the exact code literal `refs/dsh/translation-pairing/snapshots/` named by the note. | `exact-code-occurrence` |
| [`scripts/snapshots/translation-prompt-v4/request-response.expected.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/snapshots/translation-prompt-v4/request-response.expected.json) | repository automation | Contains the exact code literal `refs/dsh/translation-pairing/snapshots/` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/translation-brief.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-brief.spec.ts) — The source note names this file directly.
- [`scripts/translation-pairing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing.spec.ts) — The source note names this file directly. Contains the exact code literal `refs/dsh/translation-pairing/snapshots/` named by the note.
- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- Source verification intent: scripts/translation-brief.spec.ts pins unit and section span extraction (container-scoped kinds, depth-only section alignment so translated heading text still maps, preamble), alignment and changed-index detection, the mechanical code splice and each of its refusal conditions, terminology row matching in both directions with word-boundary and plural-inflection discipline, first-occurrence movement tracking, fence escalation, and the rendered briefing's contract (unit bundles with three-way context, mechanical/sections/document scopes, per-direction digests, scoped finish commands).

## How to read the implementation

1. Start with [`scripts/translation-brief.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-brief.ts) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `domain/llm`, `domain/multi-agent`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`
- Aliases: `git cat-file`, `pnpm run gen-translation-brief [--apply] [pair...]`, `--apply`, `verify-translation-pairing [pair...]`, `doc-sync`, `--write`, `--write --all`, `git hash-object -w --stdin`, `refs/dsh/translation-pairing/snapshots/`, `--all`, `--write <pair>`, `--list`, `Briefed minimal translation updates`, `process`
- Regex: `(?i)(git[- ]cat\-file|pnpm[- ]run[- ]gen\-translation\-brief[- ]\[\-\-apply\][- ]\[pair\.\.\.\]|\-\-apply|verify\-translation\-pairing[- ]\[pair\.\.\.\]|doc\-sync|\-\-write|\-\-write[- ]\-\-all|git[- ]hash\-object[- ]\-w[- ]\-\-stdin)`

```bash
rg -n --pcre2 "(?i)(git[- ]cat\\-file|pnpm[- ]run[- ]gen\\-translation\\-brief[- ]\\[\\-\\-apply\\][- ]\\[pair\\.\\.\\.\\]|\\-\\-apply|verify\\-translation\\-pairing[- ]\\[pair\\.\\.\\.\\]|doc\\-sync|\\-\\-write|\\-\\-write[- ]\\-\\-all|git[- ]hash\\-object[- ]\\-w[- ]\\-\\-stdin)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0384. Bilingual documentation via paired sibling files and a pairing gate](0384-bilingual-documentation-via-paired-sibling-files-and-a-pairing-gate.md): The source note links to this decision directly.
- **`source-link`** — [0430. Lightweight routine documentation translation](0430-lightweight-routine-documentation-translation.md): The source note links to this decision directly.
- **`shares-code-with`** — [0392. Package Model Experience contract](0392-package-model-experience-contract.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0687. Agent Notes](0687-agent-notes.md): Shares source implementation: `docs/i18n/README.md`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0393. Project canonical documentation into the website](0393-project-canonical-documentation-into-the-website.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0425. The documentation site carries its own images](0425-the-documentation-site-carries-its-own-images.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/translation-pairing.spec.ts`.
- **`shares-code-with`** — [0428. Automatically compose translation pairing records](0428-automatically-compose-translation-pairing-records.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0405. Calibrated translation prompt v4 contract](0405-calibrated-translation-prompt-v4-contract.md): Shares source implementation: `scripts/translation-brief.spec.ts`, `scripts/translation-prompt.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0406-briefed-minimal-translation-updates.md`.
