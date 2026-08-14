---
id: "dsh-note-0523"
title: "Supply chain checks and vendor drift verification"
status: "proposed"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/process/2026-06-11-supply-chain-and-vendor-drift.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/jobs-tasks"
  - "domain/testing"
  - "lifecycle/proposed"
  - "mechanism/policy"
aliases:
  - "license"
  - "vendor/*/src"
  - "vendor/README.md"
  - "vendor/"
  - "Supply chain checks and vendor drift verification"
  - "process"
  - "discovery routing"
  - "evidence"
  - "build release"
  - "configuration"
  - "extensions"
  - "jobs tasks"
  - "testing"
  - "proposed"
search_regex: "(?i)(license|vendor/\\*/src|vendor/README\\.md|vendor/|Supply[- ]chain[- ]checks[- ]and[- ]vendor[- ]drift[- ]verification|discovery[- ]routing|evidence|build[- ]release)"
---

# 0523. Supply chain checks and vendor drift verification — implementation context

## Open this when

The vendor manifest (the vendoring decision) is enforced at commit time in the forward direction (vendored change ⇒ manifest update) but nothing verifies the manifest's claims: that vendor/ actually equals upstream-at-SHA plus exactly the logged modifications. And the handful of true npm dependencies have no advisory monitoring or update cadence.

## Source decision

Vendor drift check (nightly CI): clone the upstream repos at the manifest SHAs (shallow), copy the corresponding package sources, and diff against vendor//src. The job fails unless the diff matches the logged local modifications (kept as a checked-in patch file per modification --- the log entries become verifiable artifacts rather than prose). Dependency advisories: osv-scanner (or pnpm audit) job on the lockfile, scheduled + on lockfile-touching PRs.

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/process/2026-06-11-supply-chain-and-vendor-drift.md](../02-notes/proposed/process/2026-06-11-supply-chain-and-vendor-drift.md)
- Pinned source: [.agents/notes/proposed/process/2026-06-11-supply-chain-and-vendor-drift.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/process/2026-06-11-supply-chain-and-vendor-drift.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`scripts/gen-third-party-notices.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.ts) | repository automation | Defines `license`, a construct named by the note. | `symbol-definition` |
| [`CLAUDE.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/CLAUDE.md) | package contract and examples | Contains the exact code literal `vendor/README.md` named by the note. | `exact-code-occurrence` |
| [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) | package contract and examples | Contains the exact code literal `vendor/README.md` named by the note. | `exact-code-occurrence` |
| [`lefthook.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/lefthook.yml) | composition and configuration | Contains the exact code literal `vendor/README.md` named by the note. | `exact-code-occurrence` |
| [`THIRD_PARTY_NOTICES.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/THIRD_PARTY_NOTICES.md) | package contract and examples | Contains the exact code literal `vendor/README.md` named by the note. | `exact-code-occurrence` |
| [`scripts/rescope-vendor.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/rescope-vendor.ts) | repository automation | Contains the exact code literal `vendor/README.md` named by the note. | `exact-code-occurrence` |
| [`scripts/publish-npm-baseline.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publish-npm-baseline.ts) | repository automation | Contains the exact code literal `vendor/README.md` named by the note. | `exact-code-occurrence` |
| [`scripts/check-expected-filenames.sh`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/check-expected-filenames.sh) | repository automation | Contains the exact code literal `vendor/README.md` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `license` | `const` | [`scripts/gen-third-party-notices.ts:296`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.ts#L296) | `const license = override?.license ?? manifest?.license` |
| `license` | `const` | [`scripts/gen-third-party-notices.ts:436`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.ts#L436) | `const license = readManifest(\`vendor/${dir}/package.json\`).license` |

### Tests and executable evidence

- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `license`.
- [`scripts/publint-all.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.spec.ts) — A test under the owning area exercises or imports `license`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `license`. Contains the exact code literal `vendor/README.md` named by the note.
- [`python/sdk/tests/test_release_version.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_release_version.py) — A test under the owning area exercises or imports `license`.
- [`scripts/verify-dsh-package-licenses.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-dsh-package-licenses.spec.ts) — A test under the owning area exercises or imports `license`.
- Source verification intent: The license inventory script runs in CI and fails on a missing LICENSE or a license field that contradicts the inventory in vendor/README.md. The nightly drift job reconstructs vendor/ from the manifest SHAs plus checked-in patch files and fails on any unexplained diff. Advisory scanning runs on the lockfile on schedule and on lockfile-touching PRs.

## How to read the implementation

1. Start with [`vendor/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/README.md) because it has the strongest evidence link to the note.
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
- **Status gate** — Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/discovery-routing`, `concern/evidence`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/jobs-tasks`, `domain/testing`, `lifecycle/proposed`, `mechanism/policy`
- Aliases: `license`, `vendor/*/src`, `vendor/README.md`, `vendor/`, `Supply chain checks and vendor drift verification`, `process`, `discovery routing`, `evidence`, `build release`, `configuration`, `extensions`, `jobs tasks`, `testing`, `proposed`
- Regex: `(?i)(license|vendor/\*/src|vendor/README\.md|vendor/|Supply[- ]chain[- ]checks[- ]and[- ]vendor[- ]drift[- ]verification|discovery[- ]routing|evidence|build[- ]release)`

```bash
rg -n --pcre2 "(?i)(license|vendor/\\*/src|vendor/README\\.md|vendor/|Supply[- ]chain[- ]checks[- ]and[- ]vendor[- ]drift[- ]verification|discovery[- ]routing|evidence|build[- ]release)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0378. Vendor Cordis as source, not npm dependencies](0378-vendor-cordis-as-source-not-npm-dependencies.md): The source note links to this decision directly.
- **`shares-code-with`** — [0430. Lightweight routine documentation translation](0430-lightweight-routine-documentation-translation.md): Shares source implementation: `AGENTS.md`, `CLAUDE.md`.
- **`shares-code-with`** — [0419. Generated third-party notices](0419-generated-third-party-notices.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/gen-third-party-notices.ts`.
- **`shares-code-with`** — [0390. Parallel pre-push gates](0390-parallel-pre-push-gates.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/publint-all.spec.ts`.
- **`shares-code-with`** — [0401. Fast local Git hooks](0401-fast-local-git-hooks.md): Shares source implementation: `lefthook.yml`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0437. Oxlint-only fix workflow](0437-oxlint-only-fix-workflow.md): Shares source implementation: `lefthook.yml`, `scripts/gen-third-party-notices.spec.ts`.
- **`shares-code-with`** — [0393. Project canonical documentation into the website](0393-project-canonical-documentation-into-the-website.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0392. Package Model Experience contract](0392-package-model-experience-contract.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0523-supply-chain-checks-and-vendor-drift-verification.md`.
