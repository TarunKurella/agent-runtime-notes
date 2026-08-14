---
id: "dsh-note-0417"
title: "Oxlint as the repository linter"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-29-oxlint-linter.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "json"
  - "off"
  - ".oxlintrc.json"
  - ".oxlintrc.staged.json"
  - "scripts/run-oxlint.ts"
  - "options.typeAware"
  - "typeAware"
  - "oxlint-tsgolint"
  - "tsconfig.host.json"
  - "scripts/client-bundle-purity.spec.ts"
  - "tsconfig.client.json"
  - "--tsconfig"
  - "typescript/no-unnecessary-condition"
  - "@stylistic/eslint-plugin"
search_regex: "(?i)(json|\\.oxlintrc\\.json|\\.oxlintrc\\.staged\\.json|scripts/run\\-oxlint\\.ts|options\\.typeAware|typeAware|oxlint\\-tsgolint|tsconfig\\.host\\.json)"
---

# 0417. Oxlint as the repository linter — implementation context

## Open this when

The repository needs type-aware TypeScript correctness rules, consistent formatting, and file-local duplicate-logic checks across its owned source. ESLint supplied those checks through a JavaScript parser, a project service, and multiple plugins, but a clean lint run spent about one minute on the local migration baseline and required an 8 GiB Node heap, CI result caches, and separately tuned ESLint concurrency. A faster runner cannot justify losing rules.

## Source decision

The root .oxlintrc.json is the authoritative type-aware repository lint configuration. The project-free .oxlintrc.staged.json profile inherits its source rules, disables type analysis for the bounded pre-commit path, and re-includes preserved TypeGraph fixtures that the type-aware backend cannot analyze. The lint and lint:fix package scripts, gate scheduler, CI, and lefthook invoke Oxlint through scripts/run-oxlint.ts; the Oxlint-only fix workflow owns multipass plugin fixes and supersedes the separate formatting fallback. options.typeAware enables oxlint-tsgolint.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-29-oxlint-linter.md](../02-notes/implemented/process/2026-07-29-oxlint-linter.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-29-oxlint-linter.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-29-oxlint-linter.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`.oxlintrc.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.oxlintrc.json) | composition and configuration | The source note names this file directly. Contains the exact code literal `typescript/no-unnecessary-condition` named by the note. | `exact-code-occurrence, named-file` |
| [`.oxlintrc.staged.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.oxlintrc.staged.json) | composition and configuration | The source note names this file directly. | `named-file` |
| [`scripts/run-oxlint.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-oxlint.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/package-graph.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-graph.ts) | repository automation | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-layout/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/index.ts) | package entry point | Defines `off`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/layout.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts) | runtime implementation | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/extensions/cordis-host-runner/src/guard.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/guard.ts) | runtime implementation | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-plan/src/client/PlanModeControl.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-plan/src/client/PlanModeControl.tsx) | runtime implementation | Defines `off`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx) | runtime implementation | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/package.json) | composition and configuration | Contains the exact code literal `scripts/run-oxlint.ts` named by the note. | `exact-code-occurrence` |
| [`lefthook.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/lefthook.yml) | composition and configuration | Contains the exact code literal `scripts/run-oxlint.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `off` | `const` | [`packages/client/ui-layout/src/client/index.ts:150`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/index.ts#L150) | `const off = ctx.on('theme/change', (snapshot) => { presenter.apply(snapshot) })` |
| `off` | `const` | [`packages/client/ui-plan/src/client/PlanModeControl.tsx:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-plan/src/client/PlanModeControl.tsx#L36) | `const off = (): void => {` |
| `json` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx:1516`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx#L1516) | `const json = parseJsonContainer(value)` |
| `json` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:851`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L851) | `const json = JSON.stringify(value, null, 2)` |
| `json` | `const` | [`packages/extensions/cordis-host-runner/src/guard.ts:525`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/guard.ts#L525) | `const json = JSON.stringify(value)` |
| `json` | `const` | [`scripts/package-graph.ts:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-graph.ts#L37) | `const json = JSON.parse(readFileSync(resolve(root, rel), 'utf8')) as {` |

### Tests and executable evidence

- [`scripts/client-bundle-purity.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-purity.spec.ts) — The source note names this file directly.
- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `lint`. A test under the owning area exercises or imports `DSH_OXLINT_THREADS`.
- [`scripts/run-oxlint.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-oxlint.spec.ts) — A test under the owning area exercises or imports `DSH_OXLINT_THREADS`. A test under the owning area exercises or imports `GOMAXPROCS`.
- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — A test under the owning area exercises or imports `lint`. A test under the owning area exercises or imports `typeAware`.
- [`scripts/lint-rule-fingerprint.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/lint-rule-fingerprint.spec.ts) — A test under the owning area exercises or imports `lint`.
- [`packages/mcp/mcp-client/tests/apply.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/tests/apply.spec.ts) — A test under the owning area exercises or imports `lint`.
- [`packages/mcp/mcp-client/tests/reconnect.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/tests/reconnect.spec.ts) — A test under the owning area exercises or imports `lint`.
- [`packages/client/runtime/tests/fake-api.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/fake-api.client.ts) — A test under the owning area exercises or imports `lint`.
- Source verification intent: The migrated configuration reports the same clean owned-source baseline after resolving two analyzer differences: one redundant test assertion was removed, while one structural cast required by tsc carries a narrow Oxlint suppression. A one-time audit against the exact deleted ESLint configuration blob established source 88-to-88, examples 87-to-87, and tests 83-to-83 after the rule-name translations. The committed fingerprint pins those audited Oxlint profiles and the complete override shape; it neither executes the deleted configuration nor propagates later upstream preset changes.

## How to read the implementation

1. Start with [`.oxlintrc.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.oxlintrc.json) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/process`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/jobs-tasks`, `domain/llm`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `json`, `off`, `.oxlintrc.json`, `.oxlintrc.staged.json`, `scripts/run-oxlint.ts`, `options.typeAware`, `typeAware`, `oxlint-tsgolint`, `tsconfig.host.json`, `scripts/client-bundle-purity.spec.ts`, `tsconfig.client.json`, `--tsconfig`, `typescript/no-unnecessary-condition`, `@stylistic/eslint-plugin`
- Regex: `(?i)(json|\.oxlintrc\.json|\.oxlintrc\.staged\.json|scripts/run\-oxlint\.ts|options\.typeAware|typeAware|oxlint\-tsgolint|tsconfig\.host\.json)`

```bash
rg -n --pcre2 "(?i)(json|\\.oxlintrc\\.json|\\.oxlintrc\\.staged\\.json|scripts/run\\-oxlint\\.ts|options\\.typeAware|typeAware|oxlint\\-tsgolint|tsconfig\\.host\\.json)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0437. Oxlint-only fix workflow](0437-oxlint-only-fix-workflow.md): The source note links to this decision directly.
- **`shares-code-with`** — [0441. Python public publication workflow](0441-python-public-publication-workflow.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/layout.ts`.
- **`shares-code-with`** — [0377. Mechanical quality gates over prose guidelines](0377-mechanical-quality-gates-over-prose-guidelines.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/layout.ts`.
- **`shares-code-with`** — [0411. Provision CI pnpm via pnpm/action-setup](0411-provision-ci-pnpm-via-pnpm-action-setup.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/layout.ts`.
- **`shares-code-with`** — [0424. Exact uncovered locations on coverage failure](0424-exact-uncovered-locations-on-coverage-failure.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/layout.ts`.
- **`shares-code-with`** — [0640. doc-sync through the gate scheduler](0640-doc-sync-through-the-gate-scheduler.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/layout.ts`.
- **`shares-code-with`** — [0443. Face-named client test files](0443-face-named-client-test-files.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-layout/src/client/index.ts`.
- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `packages/client/ui-trajectory/src/client/layout.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0417-oxlint-as-the-repository-linter.md`.
