---
id: "dsh-note-0644"
title: "Personal staging maintenance skills"
status: "archived"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/process/2026-07-23-personal-staging-maintenance-skills.md"
implementation_evidence: "lead-only"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "dsh-customize"
  - "dsh-upgrade"
  - "dsh-upstream-customization"
  - "skills/"
  - ".agents/merge.lock"
  - "dsh-staging-<timestamp>"
  - "dsh-upgrade/prepare-<timestamp>"
  - "dsh-staging/<timestamp>"
  - "Personal staging maintenance skills"
  - "process"
  - "boundary"
  - "concurrency"
  - "discovery routing"
  - "evidence"
search_regex: "(?i)(dsh\\-customize|dsh\\-upgrade|dsh\\-upstream\\-customization|skills/|\\.agents/merge\\.lock|dsh\\-staging\\-<timestamp>|dsh\\-upgrade/prepare\\-<timestamp>|dsh\\-staging/<timestamp>)"
---

# 0644. Personal staging maintenance skills — implementation context

## Open this when

Personal dsh customizations need a repeatable way to locate the installed source, isolate task work, serialize integration, and incorporate upstream changes without rewriting the checkout used by running sessions. User-local instructions solve this for one installation but cannot guide other users or remain synchronized with repository installer behavior.

## Source decision

The repository distributes dsh-customize, dsh-upgrade, and dsh-upstream-customization from its root skills/ directory. Their descriptions name both the operation and user requests that select it. The shipped TUI supplies that directory to the local skill provider at startup, below project and user roots in discovery priority. The workflows derive the active checkout and staging branch from the installed launcher rather than a user-specific path or branch name, defer to repository-local instructions, require task worktrees, and serialize staging mutations with the staging worktree's established.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/process/2026-07-23-personal-staging-maintenance-skills.md](../02-notes/archived/process/2026-07-23-personal-staging-maintenance-skills.md)
- Pinned source: [.agents/notes/archived/process/2026-07-23-personal-staging-maintenance-skills.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/process/2026-07-23-personal-staging-maintenance-skills.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

No implementation path could be confirmed from the pinned tree. Use the search handles below; do not invent a source location.

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `master`.
- [`scripts/change-scope.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/change-scope.spec.ts) — A test under the owning area exercises or imports `master`.
- [`scripts/translation-pairing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing.spec.ts) — A test under the owning area exercises or imports `master`.
- [`scripts/translation-pairing-merge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing-merge.spec.ts) — A test under the owning area exercises or imports `master`.
- [`packages/client/ui-primitives/tests/markdown-dom-parity.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown-dom-parity.client.spec.tsx) — A test under the owning area exercises or imports `master`.

## How to read the implementation

1. Start from the source note and run the regex below across the pinned repository.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/policy`, `mechanism/projection`
- Aliases: `dsh-customize`, `dsh-upgrade`, `dsh-upstream-customization`, `skills/`, `.agents/merge.lock`, `dsh-staging-<timestamp>`, `dsh-upgrade/prepare-<timestamp>`, `dsh-staging/<timestamp>`, `Personal staging maintenance skills`, `process`, `boundary`, `concurrency`, `discovery routing`, `evidence`
- Regex: `(?i)(dsh\-customize|dsh\-upgrade|dsh\-upstream\-customization|skills/|\.agents/merge\.lock|dsh\-staging\-<timestamp>|dsh\-upgrade/prepare\-<timestamp>|dsh\-staging/<timestamp>)`

```bash
rg -n --pcre2 "(?i)(dsh\\-customize|dsh\\-upgrade|dsh\\-upstream\\-customization|skills/|\\.agents/merge\\.lock|dsh\\-staging\\-<timestamp>|dsh\\-upgrade/prepare\\-<timestamp>|dsh\\-staging/<timestamp>)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0526. Remove the packed-session fixture branch migrator](0526-remove-the-packed-session-fixture-branch-migrator.md): Shares source implementation: `packages/client/ui-primitives/tests/markdown-dom-parity.client.spec.tsx`, `scripts/change-scope.spec.ts`.
- **`shares-code-with`** — [0415. Make Lefthook installation worktree-local](0415-make-lefthook-installation-worktree-local.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0414. Report an explicit repository change scope](0414-report-an-explicit-repository-change-scope.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/translation-pairing-merge.spec.ts`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0428. Automatically compose translation pairing records](0428-automatically-compose-translation-pairing-records.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/translation-pairing-merge.spec.ts`.
- **`shares-code-with`** — [0425. The documentation site carries its own images](0425-the-documentation-site-carries-its-own-images.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/translation-pairing.spec.ts`.
- **`shares-code-with`** — [0416. Per-subsystem generated cordis-surface regions](0416-per-subsystem-generated-cordis-surface-regions.md): Shares source implementation: `scripts/translation-pairing.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0644-personal-staging-maintenance-skills.md`.
