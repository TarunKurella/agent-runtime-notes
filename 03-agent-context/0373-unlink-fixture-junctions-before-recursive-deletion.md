---
id: "dsh-note-0373"
title: "Unlink fixture junctions before recursive deletion"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-12-unlink-fixture-junctions-before-delete.md"
implementation_evidence: "high"
target_anchor: "QuickJS-to-Rust capability registry"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
aliases:
  - "scripts/"
  - "node_modules"
  - "scripts/test-fixture-cleanup.ts"
  - "unlinkFixtureLinks"
  - "removeFixtureSafely"
  - "afterEach"
  - "docs/defensive-patterns.md"
  - "rmSync"
  - "Unlink fixture junctions before recursive deletion"
  - "bug fix"
  - "boundary"
  - "compatibility"
  - "discovery routing"
  - "evidence"
search_regex: "(?i)(scripts/|node_modules|scripts/test\\-fixture\\-cleanup\\.ts|unlinkFixtureLinks|removeFixtureSafely|afterEach|docs/defensive\\-patterns\\.md|rmSync)"
---

# 0373. Unlink fixture junctions before recursive deletion — implementation context

## Open this when

The install-lefthook and translation-pairing fixtures junction the repository's real scripts/, node_modules, and tsx package directories into fixture trees so installer probes resolve through them. Windows recursive deletion can treat a junction (a MOUNT_POINT reparse point) as a directory and follow it into its target; Git's worktree remove did exactly that and deleted the repository's tracked scripts/ and tsx package (the incident's instrumentation pinned the deletion to that step). A fixture cleanup that trusts its deleter therefore deletes the repository's own sources instead of the fixture.

## Source decision

scripts/test-fixture-cleanup.ts owns junction-safe fixture teardown: unlinkFixtureLinks walks a tree and unlinks every reparse point before removeFixtureSafely removes the now link-free tree (with Windows async-handle retries). Every affected afterEach and the pre-worktree remove hook call it. The general rule lives in docs/defensive-patterns.md: remove link-shaped paths with unlink, reserve recursive rmSync for known real directories.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-12-unlink-fixture-junctions-before-delete.md](../02-notes/implemented/bug-fix/2026-08-12-unlink-fixture-junctions-before-delete.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-12-unlink-fixture-junctions-before-delete.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-12-unlink-fixture-junctions-before-delete.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/defensive-patterns.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/defensive-patterns.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`CLAUDE.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/CLAUDE.md) | package contract and examples | Contains the exact code literal `docs/defensive-patterns.md` named by the note. | `exact-code-occurrence` |
| [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) | package contract and examples | Contains the exact code literal `docs/defensive-patterns.md` named by the note. | `exact-code-occurrence` |
| [`docs/subsystems/code-runtime.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/code-runtime.md) | package contract and examples | Contains the exact code literal `docs/defensive-patterns.md` named by the note. | `exact-code-occurrence` |
| [`scripts/doc-budgets.manifest.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/doc-budgets.manifest.json) | repository automation | Contains the exact code literal `docs/defensive-patterns.md` named by the note. | `exact-code-occurrence` |
| [`docs/defensive-patterns.i18n.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/defensive-patterns.i18n.yaml) | composition and configuration | Contains the exact code literal `docs/defensive-patterns.md` named by the note. | `exact-code-occurrence` |
| [`docs/subsystems/code-runtime.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/code-runtime.zh.md) | package contract and examples | Contains the exact code literal `docs/defensive-patterns.md` named by the note. | `exact-code-occurrence` |
| [`.agents/skills/dsh-code-review/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-code-review/SKILL.md) | package contract and examples | Contains the exact code literal `docs/defensive-patterns.md` named by the note. | `exact-code-occurrence` |
| [`.agents/skills/dsh-find-simplifications/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-find-simplifications/SKILL.md) | package contract and examples | Contains the exact code literal `docs/defensive-patterns.md` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/test-fixture-cleanup.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-fixture-cleanup.ts) — The source note names this file directly. A test under the owning area exercises or imports `node_modules`.
- [`scripts/clean.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/clean.spec.ts) — A test under the owning area exercises or imports `node_modules`. A test under the owning area exercises or imports `afterEach`.
- [`scripts/dev-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.spec.ts) — A test under the owning area exercises or imports `node_modules`.
- [`scripts/publint-all.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.spec.ts) — A test under the owning area exercises or imports `afterEach`. A test under the owning area exercises or imports `rmSync`.
- [`apps/cli/tests/args.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/args.spec.ts) — A test under the owning area exercises or imports `afterEach`.
- [`scripts/change-scope.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/change-scope.spec.ts) — A test under the owning area exercises or imports `afterEach`. A test under the owning area exercises or imports `rmSync`.
- [`scripts/gen-doc-graphs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.spec.ts) — A test under the owning area exercises or imports `rmSync`.
- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — A test under the owning area exercises or imports `node_modules`.

## How to read the implementation

1. Start with [`docs/defensive-patterns.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/defensive-patterns.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** QuickJS-to-Rust capability registry.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/extensions`, `domain/storage`, `domain/testing`, `lifecycle/implemented`
- Aliases: `scripts/`, `node_modules`, `scripts/test-fixture-cleanup.ts`, `unlinkFixtureLinks`, `removeFixtureSafely`, `afterEach`, `docs/defensive-patterns.md`, `rmSync`, `Unlink fixture junctions before recursive deletion`, `bug fix`, `boundary`, `compatibility`, `discovery routing`, `evidence`
- Regex: `(?i)(scripts/|node_modules|scripts/test\-fixture\-cleanup\.ts|unlinkFixtureLinks|removeFixtureSafely|afterEach|docs/defensive\-patterns\.md|rmSync)`

```bash
rg -n --pcre2 "(?i)(scripts/|node_modules|scripts/test\\-fixture\\-cleanup\\.ts|unlinkFixtureLinks|removeFixtureSafely|afterEach|docs/defensive\\-patterns\\.md|rmSync)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0374. Unlink stale profile fallback links instead of rmSync](0374-unlink-stale-profile-fallback-links-instead-of-rmsync.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/clean.spec.ts`.
- **`shares-code-with`** — [0415. Make Lefthook installation worktree-local](0415-make-lefthook-installation-worktree-local.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/dev-web.spec.ts`.
- **`shares-code-with`** — [0646. Wine-run Windows blocking gates on Linux runners](0646-wine-run-windows-blocking-gates-on-linux-runners.md): Shares source implementation: `scripts/clean.spec.ts`, `scripts/dev-web.spec.ts`.
- **`shares-code-with`** — [0372. Resolve Microsoft Store pwsh aliases](0372-resolve-microsoft-store-pwsh-aliases.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/clean.spec.ts`.
- **`shares-code-with`** — [0499. Keep supported-platform tests semantic](0499-keep-supported-platform-tests-semantic.md): Shares source implementation: `scripts/oxlint-contract.spec.ts`, `scripts/publint-all.spec.ts`.
- **`shares-code-with`** — [0411. Provision CI pnpm via pnpm/action-setup](0411-provision-ci-pnpm-via-pnpm-action-setup.md): Shares source implementation: `scripts/dev-web.spec.ts`, `scripts/oxlint-contract.spec.ts`.
- **`shares-code-with`** — [0421. Coverage-exempt heavy suites](0421-coverage-exempt-heavy-suites.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/oxlint-contract.spec.ts`.
- **`shares-code-with`** — [0320. The approval takeover shares the composer's text cap](0320-the-approval-takeover-shares-the-composer-s-text-cap.md): Shares source implementation: `scripts/gen-doc-graphs.spec.ts`, `scripts/publint-all.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0373-unlink-fixture-junctions-before-recursive-deletion.md`.
