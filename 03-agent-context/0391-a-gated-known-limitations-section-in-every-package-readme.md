---
id: "dsh-note-0391"
title: "A gated Known-Limitations section in every package README"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-10-readme-known-limitations-gate.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/evidence"
  - "domain/build-release"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "NO_LIMITATIONS"
  - "packages/<group>/<pkg>/package.json"
  - "## Known Limitations and Deferred Work"
  - "verify-package-readme-limitations"
  - "doc-sync"
  - "A gated Known-Limitations section in every package README"
  - "process"
  - "boundary"
  - "evidence"
  - "build release"
  - "storage"
  - "implemented"
  - "policy"
search_regex: "(?i)(NO_LIMITATIONS|packages/<group>/<pkg>/package\\.json|\\#\\#[- ]Known[- ]Limitations[- ]and[- ]Deferred[- ]Work|verify\\-package\\-readme\\-limitations|doc\\-sync|A[- ]gated[- ]Known\\-Limitations[- ]section[- ]in[- ]every[- ]package[- ]README|boundary|evidence)"
---

# 0391. A gated Known-Limitations section in every package README — implementation context

## Open this when

The documentation standard assigns limitations to package READMEs. Without a shared shape, an omitted section cannot distinguish an audited absence from forgotten documentation, and variant headings prevent a repository-wide search.

## Source decision

Every package manifest under packages///package.json has a sibling README with the canonical ## Known Limitations and Deferred Work section. Its bullets record durable consumer gaps and non-obvious maintainer constraints owned by that package; ordinary cleanup remains in its source TODO or owning Agent Note. The verify-package-readme-limitations gate derives the package set from manifests, rejects missing READMEs, and requires exactly one canonical h2 with at least one top-level bullet. Near-miss headings such as "Limitations," "Deferred," "What is NOT here," or "Non-goals" fail.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-10-readme-known-limitations-gate.md](../02-notes/implemented/process/2026-07-10-readme-known-limitations-gate.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-10-readme-known-limitations-gate.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-10-readme-known-limitations-gate.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/AGENTS.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/AGENTS.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`.agents/skills/dsh-prose-standard/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-prose-standard/SKILL.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`scripts/verify-package-readme-limitations.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-package-readme-limitations.ts) | repository automation | The source note names this file directly. Defines `NO_LIMITATIONS`, a construct named by the note. | `named-file, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `NO_LIMITATIONS` | `const` | [`scripts/verify-package-readme-limitations.ts:18`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-package-readme-limitations.ts#L18) | `const NO_LIMITATIONS: Readonly<Record<string, string>> = {` |

### Tests and executable evidence

- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `doc-sync`.

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

- Tags: `class/process`, `concern/boundary`, `concern/evidence`, `domain/build-release`, `domain/storage`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `NO_LIMITATIONS`, `packages/<group>/<pkg>/package.json`, `## Known Limitations and Deferred Work`, `verify-package-readme-limitations`, `doc-sync`, `A gated Known-Limitations section in every package README`, `process`, `boundary`, `evidence`, `build release`, `storage`, `implemented`, `policy`
- Regex: `(?i)(NO_LIMITATIONS|packages/<group>/<pkg>/package\.json|\#\#[- ]Known[- ]Limitations[- ]and[- ]Deferred[- ]Work|verify\-package\-readme\-limitations|doc\-sync|A[- ]gated[- ]Known\-Limitations[- ]section[- ]in[- ]every[- ]package[- ]README|boundary|evidence)`

```bash
rg -n --pcre2 "(?i)(NO_LIMITATIONS|packages/<group>/<pkg>/package\\.json|\\#\\#[- ]Known[- ]Limitations[- ]and[- ]Deferred[- ]Work|verify\\-package\\-readme\\-limitations|doc\\-sync|A[- ]gated[- ]Known\\-Limitations[- ]section[- ]in[- ]every[- ]package[- ]README|boundary|evidence)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0430. Lightweight routine documentation translation](0430-lightweight-routine-documentation-translation.md): Shares source implementation: `docs/AGENTS.md`, `scripts/gen-third-party-notices.spec.ts`.
- **`shares-code-with`** — [0630. Doc-sync enforcement](0630-doc-sync-enforcement.md): Shares source implementation: `docs/AGENTS.md`, `packages/AGENTS.md`.
- **`shares-code-with`** — [0392. Package Model Experience contract](0392-package-model-experience-contract.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0387. One gated in-file format for Agent Notes](0387-one-gated-in-file-format-for-agent-notes.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0390. Parallel pre-push gates](0390-parallel-pre-push-gates.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0406. Briefed minimal translation updates](0406-briefed-minimal-translation-updates.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0687. Agent Notes](0687-agent-notes.md): Shares source implementation: `docs/AGENTS.md`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0393. Project canonical documentation into the website](0393-project-canonical-documentation-into-the-website.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0391-a-gated-known-limitations-section-in-every-package-readme.md`.
