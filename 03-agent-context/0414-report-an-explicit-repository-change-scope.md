---
id: "dsh-note-0414"
title: "Report an explicit repository change scope"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-27-explicit-change-scope-report.md"
implementation_evidence: "lead-only"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "origin/<current-branch>"
  - "origin/master"
  - "change-scope"
  - "--base <ref>"
  - "--head <ref>"
  - "HEAD"
  - "--head"
  - "Report an explicit repository change scope"
  - "process"
  - "boundary"
  - "concurrency"
  - "discovery routing"
  - "evidence"
  - "human control"
search_regex: "(?i)(origin/<current\\-branch>|origin/master|change\\-scope|\\-\\-base[- ]<ref>|\\-\\-head[- ]<ref>|HEAD|\\-\\-head|Report[- ]an[- ]explicit[- ]repository[- ]change[- ]scope)"
---

# 0414. Report an explicit repository change scope — implementation context

## Open this when

The pre-push workflow needs the diff against the actual base, but constructing origin/ fails for a new worktree branch that tracks origin/master before its first push and misstates a stacked branch whose PR targets another feature branch. The code-review and documentation-audit workflows need the same current-base judgment. An incorrect range undermines evidence selection because it can omit affected paths. A three-dot committed diff also says nothing about Git's separate staged, unstaged, and untracked layers.

## Source decision

The root change-scope command requires --base , accepts --head with HEAD as the default, and writes one versioned JSON report. It resolves both inputs to commits with ambiguity detection and requires one merge base before rendering. The report records the repository root without normalizing legal path whitespace, input refs, resolved base, head, and merge-base commit IDs, plus sorted committed, staged, unstaged, and untracked path sets. Path records are split at raw NUL bytes; the repository root and every path are decoded as strict UTF-8.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-27-explicit-change-scope-report.md](../02-notes/implemented/process/2026-07-27-explicit-change-scope-report.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-27-explicit-change-scope-report.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-27-explicit-change-scope-report.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`.agents/skills/dsh-code-review/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-code-review/SKILL.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`.agents/skills/dsh-doc-standards/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-doc-standards/SKILL.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`.agents/skills/dsh-pre-push-checks/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-pre-push-checks/SKILL.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/cookbook/maintaining-dsh-code-review.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cookbook/maintaining-dsh-code-review.md) | package contract and examples | Contains the exact code literal `origin/master` named by the note. | `exact-code-occurrence` |
| [`docs/cookbook/maintaining-dsh-code-review.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cookbook/maintaining-dsh-code-review.zh.md) | package contract and examples | Contains the exact code literal `origin/master` named by the note. | `exact-code-occurrence` |
| [`.agents/skills/dsh-find-simplifications/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-find-simplifications/SKILL.md) | package contract and examples | Contains the exact code literal `origin/master` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/change-scope.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/change-scope.spec.ts) — A test under the owning area exercises or imports `change-scope`. A test under the owning area exercises or imports `HEAD`.
- [`apps/web/tests/navigation-panes.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/navigation-panes.e2e.ts) — A test under the owning area exercises or imports `HEAD`.
- [`scripts/translation-pairing-merge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing-merge.spec.ts) — A test under the owning area exercises or imports `HEAD`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `HEAD`.
- [`packages/host/apiproxy/tests/session-export.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/session-export.spec.ts) — A test under the owning area exercises or imports `HEAD`.
- [`packages/spill/spill-policy/tests/spill-policy.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/tests/spill-policy.spec.ts) — A test under the owning area exercises or imports `HEAD`.
- [`packages/host/frontend-static/tests/frontend-static.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/frontend-static/tests/frontend-static.spec.ts) — A test under the owning area exercises or imports `HEAD`.
- [`packages/session-query/session-log-export/tests/controller.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-log-export/tests/controller.client.spec.ts) — A test under the owning area exercises or imports `HEAD`.

## How to read the implementation

1. Start with [`.agents/skills/dsh-code-review/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-code-review/SKILL.md) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/filesystem`, `domain/llm`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`
- Aliases: `origin/<current-branch>`, `origin/master`, `change-scope`, `--base <ref>`, `--head <ref>`, `HEAD`, `--head`, `Report an explicit repository change scope`, `process`, `boundary`, `concurrency`, `discovery routing`, `evidence`, `human control`
- Regex: `(?i)(origin/<current\-branch>|origin/master|change\-scope|\-\-base[- ]<ref>|\-\-head[- ]<ref>|HEAD|\-\-head|Report[- ]an[- ]explicit[- ]repository[- ]change[- ]scope)`

```bash
rg -n --pcre2 "(?i)(origin/<current\\-branch>|origin/master|change\\-scope|\\-\\-base[- ]<ref>|\\-\\-head[- ]<ref>|HEAD|\\-\\-head|Report[- ]an[- ]explicit[- ]repository[- ]change[- ]scope)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0644. Personal staging maintenance skills](0644-personal-staging-maintenance-skills.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/translation-pairing-merge.spec.ts`.
- **`shares-code-with`** — [0525. Periodic human-review maintenance for dsh-code-review](0525-periodic-human-review-maintenance-for-dsh-code-review.md): Shares source implementation: `.agents/skills/dsh-code-review/SKILL.md`.
- **`shares-code-with`** — [0043. Cooperative tool cancellation at the registry boundary](0043-cooperative-tool-cancellation-at-the-registry-boundary.md): Shares source implementation: `packages/core/tools/tests/code-mode.spec.ts`.
- **`shares-code-with`** — [0526. Remove the packed-session fixture branch migrator](0526-remove-the-packed-session-fixture-branch-migrator.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/translation-pairing-merge.spec.ts`.
- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `scripts/change-scope.spec.ts`.
- **`shares-code-with`** — [0415. Make Lefthook installation worktree-local](0415-make-lefthook-installation-worktree-local.md): Shares source implementation: `scripts/change-scope.spec.ts`.
- **`shares-code-with`** — [0421. Coverage-exempt heavy suites](0421-coverage-exempt-heavy-suites.md): Shares source implementation: `scripts/change-scope.spec.ts`.
- **`shares-code-with`** — [0422. Native GitHub stacks and optional PR rebases](0422-native-github-stacks-and-optional-pr-rebases.md): Shares source implementation: `.agents/skills/dsh-pre-push-checks/SKILL.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0414-report-an-explicit-repository-change-scope.md`.
