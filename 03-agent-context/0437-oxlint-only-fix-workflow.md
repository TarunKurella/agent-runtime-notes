---
id: "dsh-note-0437"
title: "Oxlint-only fix workflow"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-09-oxlint-only-fix-workflow.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/observability"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/streaming"
aliases:
  - "@stylistic/eslint-plugin"
  - "@typescript-eslint/parser"
  - "object-curly-spacing"
  - "scripts/run-oxlint.ts"
  - "--fix"
  - "--fix-suggestions"
  - "--fix-dangerously"
  - "oxlint-tsgolint"
  - "eslint-plugin-sonarjs"
  - "Oxlint-only fix workflow"
  - "process"
  - "boundary"
  - "compatibility"
  - "concurrency"
search_regex: "(?i)(@stylistic/eslint\\-plugin|@typescript\\-eslint/parser|object\\-curly\\-spacing|scripts/run\\-oxlint\\.ts|\\-\\-fix|\\-\\-fix\\-suggestions|\\-\\-fix\\-dangerously|oxlint\\-tsgolint)"
---

# 0437. Oxlint-only fix workflow — implementation context

## Open this when

The repository linter migration retained a formatting-only ESLint invocation because Oxlint's JavaScript-plugin bridge was treated as validation-only. The pinned Oxlint toolchain executes the safe fixers supplied by @stylistic/eslint-plugin, so the separate formatter duplicates a configuration boundary, command startup, and direct eslint plus @typescript-eslint/parser dependencies. A single Oxlint invocation is not an equivalent replacement.

## Source decision

All repository lint and fix workflows invoke Oxlint through scripts/run-oxlint.ts. Normal validation remains one process with inherited output. An invocation containing --fix, --fix-suggestions, or --fix-dangerously captures the first Oxlint result; success emits its stdout and stderr on their original channels, while a completed non-zero run discards its potentially obsolete diagnostics and runs the same command once more with inherited output. The runner re-raises a child signal instead of retrying or converting it to an exit code, and the second process completion is final.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-09-oxlint-only-fix-workflow.md](../02-notes/implemented/process/2026-08-09-oxlint-only-fix-workflow.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-09-oxlint-only-fix-workflow.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-09-oxlint-only-fix-workflow.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/run-oxlint.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-oxlint.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/package.json) | composition and configuration | Contains the exact code literal `scripts/run-oxlint.ts` named by the note. | `exact-code-occurrence` |
| [`lefthook.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/lefthook.yml) | composition and configuration | Contains the exact code literal `scripts/run-oxlint.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/clean.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/clean.spec.ts) — A test under the owning area exercises or imports `any`.
- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — A test under the owning area exercises or imports `eslint`. A test under the owning area exercises or imports `semi`.
- [`scripts/gen-client-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-client-catalog.spec.ts) — A test under the owning area exercises or imports `any`.
- [`scripts/translation-pairing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing.spec.ts) — A test under the owning area exercises or imports `any`.
- [`scripts/client-bundle-purity.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-purity.spec.ts) — A test under the owning area exercises or imports `any`.
- [`scripts/lint-rule-fingerprint.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/lint-rule-fingerprint.spec.ts) — A test under the owning area exercises or imports `eslint`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `any`.
- Source verification intent: The executable lint contract drives a deliberately overlapping style violation through the repository runner and requires a successful exit plus exact final bytes. The same contract pins the complete Stylistic rule set, project-free TypeGraph fixture coverage, the package scripts, the staged hook command, the deleted formatter configuration, and the absence of direct ESLint parser and runner dependencies. Existing executable probes continue to cover the Stylistic and SonarJS compatibility plugins, project-free staged validation, and type-aware project discovery.

## How to read the implementation

1. Start with [`scripts/run-oxlint.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-oxlint.ts) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/observability`, `domain/testing`, `lifecycle/implemented`, `mechanism/streaming`
- Aliases: `@stylistic/eslint-plugin`, `@typescript-eslint/parser`, `object-curly-spacing`, `scripts/run-oxlint.ts`, `--fix`, `--fix-suggestions`, `--fix-dangerously`, `oxlint-tsgolint`, `eslint-plugin-sonarjs`, `Oxlint-only fix workflow`, `process`, `boundary`, `compatibility`, `concurrency`
- Regex: `(?i)(@stylistic/eslint\-plugin|@typescript\-eslint/parser|object\-curly\-spacing|scripts/run\-oxlint\.ts|\-\-fix|\-\-fix\-suggestions|\-\-fix\-dangerously|oxlint\-tsgolint)`

```bash
rg -n --pcre2 "(?i)(@stylistic/eslint\\-plugin|@typescript\\-eslint/parser|object\\-curly\\-spacing|scripts/run\\-oxlint\\.ts|\\-\\-fix|\\-\\-fix\\-suggestions|\\-\\-fix\\-dangerously|oxlint\\-tsgolint)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0417. Oxlint as the repository linter](0417-oxlint-as-the-repository-linter.md): The source note links to this decision directly.
- **`shares-code-with`** — [0415. Make Lefthook installation worktree-local](0415-make-lefthook-installation-worktree-local.md): Shares source implementation: `package.json`, `scripts/oxlint-contract.spec.ts`.
- **`shares-code-with`** — [0373. Unlink fixture junctions before recursive deletion](0373-unlink-fixture-junctions-before-recursive-deletion.md): Shares source implementation: `scripts/clean.spec.ts`, `scripts/oxlint-contract.spec.ts`.
- **`shares-code-with`** — [0425. The documentation site carries its own images](0425-the-documentation-site-carries-its-own-images.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/translation-pairing.spec.ts`.
- **`shares-code-with`** — [0406. Briefed minimal translation updates](0406-briefed-minimal-translation-updates.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/translation-pairing.spec.ts`.
- **`shares-code-with`** — [0526. Remove the packed-session fixture branch migrator](0526-remove-the-packed-session-fixture-branch-migrator.md): Shares source implementation: `package.json`, `scripts/translation-pairing.spec.ts`.
- **`shares-code-with`** — [0585. TUI file-reference autocomplete](0585-tui-file-reference-autocomplete.md): Shares source implementation: `scripts/gen-client-catalog.spec.ts`, `scripts/translation-pairing.spec.ts`.
- **`shares-code-with`** — [0646. Wine-run Windows blocking gates on Linux runners](0646-wine-run-windows-blocking-gates-on-linux-runners.md): Shares source implementation: `scripts/clean.spec.ts`, `scripts/oxlint-contract.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0437-oxlint-only-fix-workflow.md`.
