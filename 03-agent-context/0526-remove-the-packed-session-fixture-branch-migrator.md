---
id: "dsh-note-0526"
title: "Remove the packed-session fixture branch migrator"
status: "proposed"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/process/2026-07-26-remove-packed-session-fixture-migrator.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/proposed"
  - "mechanism/policy"
aliases:
  - "pnpm run migrate:packed-session-fixtures"
  - "scripts/migrate-packed-session-fixtures.ts"
  - "migrate:packed-session-fixtures"
  - "scripts/session-fixture-layout.snapshot.ts"
  - "scripts/session-fixture-layout.ts"
  - "pnpm run doc-sync"
  - "Remove the packed-session fixture branch migrator"
  - "process"
  - "compatibility"
  - "evidence"
  - "ownership"
  - "schema types"
  - "simplification"
  - "build release"
search_regex: "(?i)(pnpm[- ]run[- ]migrate:packed\\-session\\-fixtures|scripts/migrate\\-packed\\-session\\-fixtures\\.ts|migrate:packed\\-session\\-fixtures|scripts/session\\-fixture\\-layout\\.snapshot\\.ts|scripts/session\\-fixture\\-layout\\.ts|pnpm[- ]run[- ]doc\\-sync|Remove[- ]the[- ]packed\\-session[- ]fixture[- ]branch[- ]migrator|compatibility)"
---

# 0526. Remove the packed-session fixture branch migrator — implementation context

## Open this when

The repository's default writers and snapshot check keep session fixtures in the canonical packed-row layout. pnpm run migrate:packed-session-fixtures remains alongside that permanent enforcement only so in-flight branches carrying older fixture edits can merge current master and mechanically converge without re-recording model output. Once every such branch is merged, closed, or already canonical, the write command and its branch-convergence instructions have no continuing owner.

## Source decision

Remove the temporary scripts/migrate-packed-session-fixtures.ts CLI and the root migrate:packed-session-fixtures package command after a live inventory confirms that no open pull request still needs to convert session-format JSONL. Remove the transitional command links from the testing policy, the ACP snapshot README, and the implemented packed-row Agent Note in the same change; replace the command-specific remediation text in scripts/session-fixture-layout.snapshot.ts with command-independent canonical-layout guidance. Retain scripts/session-fixture-layout.ts, its unit tests, and scripts/session-fixture-layout.

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/process/2026-07-26-remove-packed-session-fixture-migrator.md](../02-notes/proposed/process/2026-07-26-remove-packed-session-fixture-migrator.md)
- Pinned source: [.agents/notes/proposed/process/2026-07-26-remove-packed-session-fixture-migrator.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/process/2026-07-26-remove-packed-session-fixture-migrator.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/session-fixture-layout.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/session-fixture-layout.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/migrate-packed-session-fixtures.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/migrate-packed-session-fixtures.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/session-fixture-layout.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/session-fixture-layout.snapshot.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/package.json) | composition and configuration | Contains the exact code literal `scripts/migrate-packed-session-fixtures.ts` named by the note. | `exact-code-occurrence` |
| [`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.md) | package contract and examples | Contains the exact code literal `scripts/migrate-packed-session-fixtures.ts` named by the note. | `exact-code-occurrence` |
| [`docs/testing.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.zh.md) | package contract and examples | Contains the exact code literal `scripts/migrate-packed-session-fixtures.ts` named by the note. | `exact-code-occurrence` |
| [`packages/test-support/acp-snapshot/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/README.md) | package contract and examples | Contains the exact code literal `scripts/migrate-packed-session-fixtures.ts` named by the note. | `exact-code-occurrence` |
| [`packages/test-support/acp-snapshot/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/README.zh.md) | package contract and examples | Contains the exact code literal `scripts/migrate-packed-session-fixtures.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `master`.
- [`scripts/change-scope.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/change-scope.spec.ts) — A test under the owning area exercises or imports `master`.
- [`scripts/translation-pairing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing.spec.ts) — A test under the owning area exercises or imports `master`.
- [`scripts/translation-pairing-merge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing-merge.spec.ts) — A test under the owning area exercises or imports `master`.
- [`packages/client/ui-primitives/tests/markdown-dom-parity.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown-dom-parity.client.spec.tsx) — A test under the owning area exercises or imports `master`.
- Source verification intent: A live open-PR inventory finds no branch with session-format JSONL changes that still depends on the temporary migration command. The temporary CLI, root package command, every branch-convergence link, and the command-specific gate diagnostic are absent; the permanent canonicalizer, unit tests, and snapshot check remain. pnpm run test:snapshot, pnpm run doc-sync, lint, and whitespace validation pass without the temporary command. Current documentation describes only the packed default and permanent canonical-layout enforcement.

## How to read the implementation

1. Start with [`scripts/session-fixture-layout.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/session-fixture-layout.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **Status gate** — Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/compatibility`, `concern/evidence`, `concern/ownership`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/proposed`, `mechanism/policy`
- Aliases: `pnpm run migrate:packed-session-fixtures`, `scripts/migrate-packed-session-fixtures.ts`, `migrate:packed-session-fixtures`, `scripts/session-fixture-layout.snapshot.ts`, `scripts/session-fixture-layout.ts`, `pnpm run doc-sync`, `Remove the packed-session fixture branch migrator`, `process`, `compatibility`, `evidence`, `ownership`, `schema types`, `simplification`, `build release`
- Regex: `(?i)(pnpm[- ]run[- ]migrate:packed\-session\-fixtures|scripts/migrate\-packed\-session\-fixtures\.ts|migrate:packed\-session\-fixtures|scripts/session\-fixture\-layout\.snapshot\.ts|scripts/session\-fixture\-layout\.ts|pnpm[- ]run[- ]doc\-sync|Remove[- ]the[- ]packed\-session[- ]fixture[- ]branch[- ]migrator|compatibility)`

```bash
rg -n --pcre2 "(?i)(pnpm[- ]run[- ]migrate:packed\\-session\\-fixtures|scripts/migrate\\-packed\\-session\\-fixtures\\.ts|migrate:packed\\-session\\-fixtures|scripts/session\\-fixture\\-layout\\.snapshot\\.ts|scripts/session\\-fixture\\-layout\\.ts|pnpm[- ]run[- ]doc\\-sync|Remove[- ]the[- ]packed\\-session[- ]fixture[- ]branch[- ]migrator|compatibility)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0644. Personal staging maintenance skills](0644-personal-staging-maintenance-skills.md): Shares source implementation: `packages/client/ui-primitives/tests/markdown-dom-parity.client.spec.tsx`, `scripts/change-scope.spec.ts`.
- **`shares-code-with`** — [0415. Make Lefthook installation worktree-local](0415-make-lefthook-installation-worktree-local.md): Shares source implementation: `package.json`, `scripts/change-scope.spec.ts`.
- **`shares-code-with`** — [0065. Make packed chunk rows the default JSONL layout](0065-make-packed-chunk-rows-the-default-jsonl-layout.md): Shares source implementation: `scripts/migrate-packed-session-fixtures.ts`, `scripts/session-fixture-layout.snapshot.ts`.
- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0434. Cite committed artifacts, never design-session ordinals](0434-cite-committed-artifacts-never-design-session-ordinals.md): Shares source implementation: `package.json`, `scripts/translation-pairing-merge.spec.ts`.
- **`shares-code-with`** — [0425. The documentation site carries its own images](0425-the-documentation-site-carries-its-own-images.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/translation-pairing.spec.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `docs/testing.md`.
- **`shares-code-with`** — [0437. Oxlint-only fix workflow](0437-oxlint-only-fix-workflow.md): Shares source implementation: `package.json`, `scripts/translation-pairing.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0526-remove-the-packed-session-fixture-branch-migrator.md`.
