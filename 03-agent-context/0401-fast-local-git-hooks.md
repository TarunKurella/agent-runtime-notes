---
id: "dsh-note-0401"
title: "Fast local Git hooks"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-22-fast-local-git-hooks.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/jobs-tasks"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
aliases:
  - "git diff --cached --check"
  - "check-all"
  - "stage_fixed"
  - "Fast local Git hooks"
  - "process"
  - "boundary"
  - "discovery routing"
  - "evidence"
  - "performance"
  - "recovery"
  - "schema types"
  - "simplification"
  - "build release"
  - "extensions"
search_regex: "(?i)(git[- ]diff[- ]\\-\\-cached[- ]\\-\\-check|check\\-all|stage_fixed|Fast[- ]local[- ]Git[- ]hooks|boundary|discovery[- ]routing|evidence|performance)"
---

# 0401. Fast local Git hooks — implementation context

## Open this when

An agent already runs the tests and checks that exercise its change, while commit, push, and CI can each repeat increasingly broad subsets of the same work. A full pre-push suite therefore delays every publication, amplifies unrelated local flakes, and gives no new signal when CI immediately runs the exhaustive matrix again. Fast hooks still need to reject cheap, high-confidence defects before work leaves the machine.

## Source decision

lefthook.yml keeps both hooks as bounded local checkpoints. Pre-commit runs sequentially: a project-free Oxlint profile validates changed JavaScript and TypeScript, applies safe fixes with a bounded retry, and re-stages them; git diff --cached --check rejects staged whitespace errors, and the vendor manifest guard checks vendored-source metadata. Pre-push runs pnpm run typecheck, which prepares the generated Host Typert contracts before the Client incremental typecheck. Pre-commit does not run type analysis, tests, snapshots, documentation checks, builds, hygiene, or the gate scheduler.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-22-fast-local-git-hooks.md](../02-notes/implemented/process/2026-07-22-fast-local-git-hooks.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-22-fast-local-git-hooks.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-22-fast-local-git-hooks.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`lefthook.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/lefthook.yml) | composition and configuration | The source note names this file directly. | `named-file` |
| [`scripts/run-gates.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts) | repository automation | The source note names this file directly. | `named-file` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `check-all`.

## How to read the implementation

1. Start with [`lefthook.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/lefthook.yml) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/extensions`, `domain/jobs-tasks`, `domain/testing`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`
- Aliases: `git diff --cached --check`, `check-all`, `stage_fixed`, `Fast local Git hooks`, `process`, `boundary`, `discovery routing`, `evidence`, `performance`, `recovery`, `schema types`, `simplification`, `build release`, `extensions`
- Regex: `(?i)(git[- ]diff[- ]\-\-cached[- ]\-\-check|check\-all|stage_fixed|Fast[- ]local[- ]Git[- ]hooks|boundary|discovery[- ]routing|evidence|performance)`

```bash
rg -n --pcre2 "(?i)(git[- ]diff[- ]\\-\\-cached[- ]\\-\\-check|check\\-all|stage_fixed|Fast[- ]local[- ]Git[- ]hooks|boundary|discovery[- ]routing|evidence|performance)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0377. Mechanical quality gates over prose guidelines](0377-mechanical-quality-gates-over-prose-guidelines.md): The source note links to this decision directly.
- **`source-link`** — [0390. Parallel pre-push gates](0390-parallel-pre-push-gates.md): The source note links to this decision directly.
- **`source-link`** — [0417. Oxlint as the repository linter](0417-oxlint-as-the-repository-linter.md): The source note links to this decision directly.
- **`source-link`** — [0437. Oxlint-only fix workflow](0437-oxlint-only-fix-workflow.md): The source note links to this decision directly.
- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `scripts/run-gates.spec.ts`, `scripts/run-gates.ts`.
- **`shares-code-with`** — [0637. Parallel GitHub CI gates](0637-parallel-github-ci-gates.md): Shares source implementation: `scripts/run-gates.spec.ts`, `scripts/run-gates.ts`.
- **`shares-code-with`** — [0404. Portable pull-request CI recovery boundary](0404-portable-pull-request-ci-recovery-boundary.md): Shares source implementation: `scripts/run-gates.spec.ts`, `scripts/run-gates.ts`.
- **`shares-code-with`** — [0503. Required CI gate for web browser expected outputs](0503-required-ci-gate-for-web-browser-expected-outputs.md): Shares source implementation: `scripts/run-gates.spec.ts`, `scripts/run-gates.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0401-fast-local-git-hooks.md`.
