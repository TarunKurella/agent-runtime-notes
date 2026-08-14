---
id: "dsh-note-0430"
title: "Lightweight routine documentation translation"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-08-lightweight-routine-documentation-translation.md"
implementation_evidence: "lead-only"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
aliases:
  - "disable-model-invocation: true"
  - "user-invocable: true"
  - "SKILL.md"
  - "policy.allow_implicit_invocation: false"
  - "agents/openai.yaml"
  - ".claude/skills"
  - "doc-sync"
  - "dsh-translate-docs"
  - "/dsh-translate-docs"
  - "$dsh-translate-docs"
  - "Lightweight routine documentation translation"
  - "process"
  - "boundary"
  - "discovery routing"
search_regex: "(?i)(disable\\-model\\-invocation:[- ]true|user\\-invocable:[- ]true|SKILL\\.md|policy\\.allow_implicit_invocation:[- ]false|agents/openai\\.yaml|\\.claude/skills|doc\\-sync|dsh\\-translate\\-docs)"
---

# 0430. Lightweight routine documentation translation — implementation context

## Open this when

Routine bilingual edits automatically selected the full translation skill. Even after the briefed-update optimization, a small documentation change could still load a specialized workflow, generate a briefing, delegate prose to a subagent, and perform a separate verification pass. That orchestration consumed more time, context, and model tokens than translating the changed text itself, and automatic skill discovery exposed the workflow on ordinary documentation turns.

## Source decision

Routine translation is one shot and one pass. The active agent loads terminology.md, translates only the changed content directly, moves a terminology annotation when the true first occurrence crosses the edit boundary, otherwise preserves reviewed counterpart prose outside the change, and re-records the pair. It does not invoke a translation skill, generate a briefing, start a separate translation-review pass, or delegate translation to a subagent. The extended workflow is manual-only. dsh-translate-docs retains its briefing, delegated prose, whole-document, and scoped-verification paths.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-08-lightweight-routine-documentation-translation.md](../02-notes/implemented/process/2026-08-08-lightweight-routine-documentation-translation.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-08-lightweight-routine-documentation-translation.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-08-lightweight-routine-documentation-translation.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/i18n/terminology.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/i18n/terminology.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`.agents/skills/dsh-translate-docs/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-translate-docs/SKILL.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`CLAUDE.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/CLAUDE.md) | package contract and examples | Contains the exact code literal `dsh-translate-docs` named by the note. | `exact-code-occurrence` |
| [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) | package contract and examples | Contains the exact code literal `dsh-translate-docs` named by the note. | `exact-code-occurrence` |
| [`docs/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/AGENTS.md) | package contract and examples | Contains the exact code literal `dsh-translate-docs` named by the note. | `exact-code-occurrence` |
| [`scripts/translation-brief.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-brief.ts) | repository automation | Contains the exact code literal `dsh-translate-docs` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-translation-brief.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-translation-brief.ts) | repository automation | Contains the exact code literal `dsh-translate-docs` named by the note. | `exact-code-occurrence` |
| [`scripts/verify-skill-invocation-metadata.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-skill-invocation-metadata.ts) | repository automation | Contains the exact code literal `agents/openai.yaml` named by the note. | `exact-code-occurrence` |
| [`.agents/skills/dsh-trim-cot-leakage/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-trim-cot-leakage/SKILL.md) | package contract and examples | Contains the exact code literal `dsh-translate-docs` named by the note. | `exact-code-occurrence` |
| [`scripts/snapshots/translation-prompt-v4/request-response.expected.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/snapshots/translation-prompt-v4/request-response.expected.json) | repository automation | Contains the exact code literal `dsh-translate-docs` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`scripts/translation-prompt.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-prompt.spec.ts) — A test under the owning area exercises or imports `dsh-translate-docs`. Contains the exact code literal `dsh-translate-docs` named by the note.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`scripts/verify-skill-invocation-metadata.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-skill-invocation-metadata.spec.ts) — Contains the exact code literal `agents/openai.yaml` named by the note.

## How to read the implementation

1. Start with [`docs/i18n/terminology.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/i18n/terminology.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/testing`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`
- Aliases: `disable-model-invocation: true`, `user-invocable: true`, `SKILL.md`, `policy.allow_implicit_invocation: false`, `agents/openai.yaml`, `.claude/skills`, `doc-sync`, `dsh-translate-docs`, `/dsh-translate-docs`, `$dsh-translate-docs`, `Lightweight routine documentation translation`, `process`, `boundary`, `discovery routing`
- Regex: `(?i)(disable\-model\-invocation:[- ]true|user\-invocable:[- ]true|SKILL\.md|policy\.allow_implicit_invocation:[- ]false|agents/openai\.yaml|\.claude/skills|doc\-sync|dsh\-translate\-docs)`

```bash
rg -n --pcre2 "(?i)(disable\\-model\\-invocation:[- ]true|user\\-invocable:[- ]true|SKILL\\.md|policy\\.allow_implicit_invocation:[- ]false|agents/openai\\.yaml|\\.claude/skills|doc\\-sync|dsh\\-translate\\-docs)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0406. Briefed minimal translation updates](0406-briefed-minimal-translation-updates.md): The source note links to this decision directly.
- **`shares-code-with`** — [0523. Supply chain checks and vendor drift verification](0523-supply-chain-checks-and-vendor-drift-verification.md): Shares source implementation: `AGENTS.md`, `CLAUDE.md`.
- **`shares-code-with`** — [0393. Project canonical documentation into the website](0393-project-canonical-documentation-into-the-website.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0384. Bilingual documentation via paired sibling files and a pairing gate](0384-bilingual-documentation-via-paired-sibling-files-and-a-pairing-gate.md): Shares source implementation: `.agents/skills/dsh-translate-docs/SKILL.md`, `docs/i18n/terminology.md`.
- **`shares-code-with`** — [0392. Package Model Experience contract](0392-package-model-experience-contract.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0428. Automatically compose translation pairing records](0428-automatically-compose-translation-pairing-records.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0386. Documentation structure, tiers, and budgets](0386-documentation-structure-tiers-and-budgets.md): Shares source implementation: `.agents/skills/dsh-translate-docs/SKILL.md`, `docs/AGENTS.md`.
- **`shares-code-with`** — [0687. Agent Notes](0687-agent-notes.md): Shares source implementation: `docs/AGENTS.md`, `scripts/run-gates.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0430-lightweight-routine-documentation-translation.md`.
