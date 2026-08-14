---
id: "dsh-note-0428"
title: "Automatically compose translation pairing records"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-08-automatic-translation-pairing-merges.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/shell-terminal"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/projection"
aliases:
  - "MERGE_HEAD"
  - "*.i18n.yaml"
  - "dsh-translation-pairing"
  - "merge.default"
  - "pnpm run resolve-translation-pairing-conflicts"
  - "pre-merge-commit"
  - "pre-commit"
  - ".i18n.yaml"
  - "doc-sync"
  - "node scripts/install-lefthook.mjs"
  - "UU"
  - "git merge --abort"
  - "post-merge"
  - "merge.dsh-translation-pairing.*"
search_regex: "(?i)(MERGE_HEAD|\\*\\.i18n\\.yaml|dsh\\-translation\\-pairing|merge\\.default|pnpm[- ]run[- ]resolve\\-translation\\-pairing\\-conflicts|pre\\-merge\\-commit|pre\\-commit|\\.i18n\\.yaml)"
---

# 0428. Automatically compose translation pairing records — implementation context

## Open this when

A bilingual consistency record contains the two owner files' exact blob hashes. Two branches that independently update different parts of the same confirmed pair therefore conflict on both hash lines even when Git cleanly composes both Markdown owners. Selecting either side leaves stale hashes, while regenerating the record by hand repeats a deterministic operation and prevents an otherwise automatic merge.

## Source decision

.i18n.yaml uses the repository-owned dsh-translation-pairing merge driver. The worktree-local Git installer registers its command alongside Lefthook setup; Git configuration remains local because a tracked attribute can name a driver but cannot carry its executable command. The installer loads the exact Node/tsx entrypoint before publishing worktree integration. Git invokes a checked-in shell launcher that does not require Node and repeats this probe before every driver execution.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-08-automatic-translation-pairing-merges.md](../02-notes/implemented/process/2026-08-08-automatic-translation-pairing-merges.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-08-automatic-translation-pairing-merges.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-08-automatic-translation-pairing-merges.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/install-lefthook.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.mjs) | repository automation | The source note names this file directly. Contains the exact code literal `dsh-translation-pairing` named by the note. | `exact-code-occurrence, named-file` |
| [`scripts/slot-walk.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/slot-walk.ts) | repository automation | Defines `MERGE_HEAD`, a construct named by the note. | `symbol-definition` |
| [`scripts/cordis-walk.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-walk.ts) | repository automation | Defines `MERGE_HEAD`, a construct named by the note. | `symbol-definition` |
| [`docs/development.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/development.md) | package contract and examples | Contains the exact code literal `dsh-translation-pairing` named by the note. | `exact-code-occurrence` |
| [`docs/i18n/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/i18n/README.md) | package contract and examples | Contains the exact code literal `dsh-translation-pairing` named by the note. | `exact-code-occurrence` |
| [`docs/development.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/development.zh.md) | package contract and examples | Contains the exact code literal `dsh-translation-pairing` named by the note. | `exact-code-occurrence` |
| [`docs/i18n/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/i18n/README.zh.md) | package contract and examples | Contains the exact code literal `dsh-translation-pairing` named by the note. | `exact-code-occurrence` |
| [`scripts/snapshots/translation-prompt-v4/request-response.expected.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/snapshots/translation-prompt-v4/request-response.expected.json) | repository automation | Contains the exact code literal `dsh-translation-pairing` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `MERGE_HEAD` | `const` | [`scripts/cordis-walk.ts:14`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-walk.ts#L14) | `const MERGE_HEAD = /declare module ['"](?:@deepseek-ai\/cordis\|\.\/context\.ts)['"]/` |
| `MERGE_HEAD` | `const` | [`scripts/slot-walk.ts:18`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/slot-walk.ts#L18) | `const MERGE_HEAD = /declare module ['"]@deepseek-ai\/dsh-client-ui-slots['"]/` |

### Tests and executable evidence

- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `pre-merge-commit`. A test under the owning area exercises or imports `pre-commit`.
- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — A test under the owning area exercises or imports `dsh-translation-pairing`. A test under the owning area exercises or imports `pre-merge-commit`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`scripts/translation-pairing-merge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing-merge.spec.ts) — A test under the owning area exercises or imports `dsh-translation-pairing`. A test under the owning area exercises or imports `pre-merge-commit`.
- [`packages/core/agent-loop/tests/coverage-edges.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/coverage-edges.spec.ts) — A test under the owning area exercises or imports `pre-commit`.
- [`packages/core/agent-loop/tests/contract-regressions.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/contract-regressions.spec.ts) — A test under the owning area exercises or imports `pre-commit`.
- Source verification intent: Script tests exercise clean composition through the installed launcher, missing-runtime and broken-entrypoint text fallback, installer probe rollback, a rejecting pre-merge-commit hook, explicit recovery from an unresolved index, mixed safe and owner-conflicted pairs, edited sidecars, non-text default merge configuration, record parsing, and worktree-local installation. The existing corpus verifier continues to prove that a committed record matches its two owners.

## How to read the implementation

1. Start with [`scripts/install-lefthook.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.mjs) because it has the strongest evidence link to the note.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `domain/extensions`, `domain/shell-terminal`, `domain/testing`, `lifecycle/implemented`, `mechanism/projection`
- Aliases: `MERGE_HEAD`, `*.i18n.yaml`, `dsh-translation-pairing`, `merge.default`, `pnpm run resolve-translation-pairing-conflicts`, `pre-merge-commit`, `pre-commit`, `.i18n.yaml`, `doc-sync`, `node scripts/install-lefthook.mjs`, `UU`, `git merge --abort`, `post-merge`, `merge.dsh-translation-pairing.*`
- Regex: `(?i)(MERGE_HEAD|\*\.i18n\.yaml|dsh\-translation\-pairing|merge\.default|pnpm[- ]run[- ]resolve\-translation\-pairing\-conflicts|pre\-merge\-commit|pre\-commit|\.i18n\.yaml)`

```bash
rg -n --pcre2 "(?i)(MERGE_HEAD|\\*\\.i18n\\.yaml|dsh\\-translation\\-pairing|merge\\.default|pnpm[- ]run[- ]resolve\\-translation\\-pairing\\-conflicts|pre\\-merge\\-commit|pre\\-commit|\\.i18n\\.yaml)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/install-lefthook.spec.ts`.
- **`shares-code-with`** — [0415. Make Lefthook installation worktree-local](0415-make-lefthook-installation-worktree-local.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/install-lefthook.mjs`.
- **`shares-code-with`** — [0644. Personal staging maintenance skills](0644-personal-staging-maintenance-skills.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/translation-pairing-merge.spec.ts`.
- **`shares-code-with`** — [0406. Briefed minimal translation updates](0406-briefed-minimal-translation-updates.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0430. Lightweight routine documentation translation](0430-lightweight-routine-documentation-translation.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0393. Project canonical documentation into the website](0393-project-canonical-documentation-into-the-website.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0428-automatically-compose-translation-pairing-records.md`.
