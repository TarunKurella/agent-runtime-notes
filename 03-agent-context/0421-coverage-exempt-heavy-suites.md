---
id: "dsh-note-0421"
title: "Coverage-exempt heavy suites"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-31-coverage-exempt-heavy-suites.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/shell-terminal"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "env"
  - "include"
  - "packages/typert/generator/tests/type-model.spec.ts"
  - "ci-coverage"
  - "DSH_COVERAGE_EXEMPT_HEAVY=1"
  - "vitest.config.ts"
  - "Gate.env"
  - "test:coverage-exempt-heavy"
  - "scripts/coverage-exempt.ts"
  - "coverage.include"
  - "typert-registry"
  - "tool-cordis"
  - "scripts/install-lefthook.spec.ts"
  - "scripts/oxlint-contract.spec.ts"
search_regex: "(?i)(include|packages/typert/generator/tests/type\\-model\\.spec\\.ts|ci\\-coverage|DSH_COVERAGE_EXEMPT_HEAVY=1|vitest\\.config\\.ts|Gate\\.env|test:coverage\\-exempt\\-heavy|scripts/coverage\\-exempt\\.ts)"
---

# 0421. Coverage-exempt heavy suites — implementation context

## Open this when

The CI coverage lane (check:ci:coverage) had its wall clock pinned by a handful of heavy test files: in a local 6-worker full-suite profile, 555 test files aggregated 1595 seconds, with packages/typert/generator/tests/type-model.spec.ts alone at 885 seconds and the top 10 files holding 84% of the aggregate. These suites share one shape --- every case performs whole-workspace compiler analysis or drives real subprocess fixtures --- and v8 instrumentation multiplies exactly that kind of runtime.

## Source decision

The ci-coverage aggregate splits into two parallel gates; every test still runs, and only the heavy suites stop paying the instrumentation tax: Instrumented gate (test:coverage): sets DSH_COVERAGE_EXEMPT_HEAVY=1, which makes vitest.config.ts drop the exempt suites from both projects' excludes; every remaining file runs instrumented and carries the entire threshold proof. The variable is injected through the gate's own env (the existing Gate.env mechanism), not the workflow-global environment, so the uninstrumented gate beside it and any local vitest run never see it and behave unchanged.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-31-coverage-exempt-heavy-suites.md](../02-notes/implemented/process/2026-07-31-coverage-exempt-heavy-suites.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-31-coverage-exempt-heavy-suites.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-31-coverage-exempt-heavy-suites.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/coverage-exempt.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/coverage-exempt.ts) | repository automation | The source note names this file directly. Contains the exact code literal `scripts/install-lefthook.spec.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`vendor/include/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/include`. | `named-package-member` |
| [`packages/typert/registry/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/src/index.ts) | package entry point | Core file in the package named by the note: `packages/typert/registry`. | `named-package-member` |
| [`packages/typert/registry/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/typert/registry`. | `named-package-member` |
| [`packages/typert/registry/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/typert/registry`. | `named-package-member` |
| [`packages/extensions/tool-cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/src/index.ts) | package entry point | Core file in the package named by the note: `packages/extensions/tool-cordis`. | `named-package-member` |
| [`packages/extensions/tool-cordis/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/extensions/tool-cordis`. | `named-package-member` |
| [`vendor/include`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/vendor/include) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/typert/registry`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/extensions/tool-cordis`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `include`, a construct named by the note. | `symbol-definition` |
| [`packages/subprocess/subprocess/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/index.ts) | package entry point | Defines `env`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `env` | `const` | [`packages/subprocess/subprocess/src/index.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/index.ts#L61) | `const env: Record<string, string> = {}` |
| `include` | `const` | [`vendor/hmr/src/index.ts:250`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L250) | `const include = entry.subtree as Include \| undefined` |

### Tests and executable evidence

- [`scripts/change-scope.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/change-scope.spec.ts) — The source note names this file directly.
- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — The source note names this file directly.
- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — The source note names this file directly.
- [`packages/typert/generator/tests/type-model.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/type-model.spec.ts) — The source note names this file directly.
- [`packages/extensions/tool-cordis/tests/cordis-lifecycle.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/tests/cordis-lifecycle.spec.ts) — A test under the owning area exercises or imports `tool-cordis`.
- Source verification intent: Measured on CI (16-core runner): the gate segment went from 424 seconds to the two gates in parallel --- test:coverage 95.9 s + test:coverage-exempt-heavy 71.1 s --- with the lane converging on the slower at about 96 seconds; the instrumented gate reported zero threshold errors both before and after the split. vitest list verifies the env toggle adds and removes exactly the exempt set; run-gates.spec.ts covers the aggregate graph construction.

## How to read the implementation

1. Start with [`scripts/coverage-exempt.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/coverage-exempt.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/process`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/jobs-tasks`, `domain/llm`, `domain/shell-terminal`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `env`, `include`, `packages/typert/generator/tests/type-model.spec.ts`, `ci-coverage`, `DSH_COVERAGE_EXEMPT_HEAVY=1`, `vitest.config.ts`, `Gate.env`, `test:coverage-exempt-heavy`, `scripts/coverage-exempt.ts`, `coverage.include`, `typert-registry`, `tool-cordis`, `scripts/install-lefthook.spec.ts`, `scripts/oxlint-contract.spec.ts`
- Regex: `(?i)(include|packages/typert/generator/tests/type\-model\.spec\.ts|ci\-coverage|DSH_COVERAGE_EXEMPT_HEAVY=1|vitest\.config\.ts|Gate\.env|test:coverage\-exempt\-heavy|scripts/coverage\-exempt\.ts)`

```bash
rg -n --pcre2 "(?i)(include|packages/typert/generator/tests/type\\-model\\.spec\\.ts|ci\\-coverage|DSH_COVERAGE_EXEMPT_HEAVY=1|vitest\\.config\\.ts|Gate\\.env|test:coverage\\-exempt\\-heavy|scripts/coverage\\-exempt\\.ts)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0415. Make Lefthook installation worktree-local](0415-make-lefthook-installation-worktree-local.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/install-lefthook.spec.ts`.
- **`shares-code-with`** — [0143. Skill system --- progressive disclosure instructions for agents](0143-skill-system-progressive-disclosure-instructions-for-agents.md): Shares source implementation: `packages/typert/registry/src/index.ts`, `packages/typert/registry/src/invariant.ts`.
- **`shares-code-with`** — [0067. Compiler-independent Typert type model](0067-compiler-independent-typert-type-model.md): Shares source implementation: `packages/typert/registry/src/index.ts`, `packages/typert/registry/src/types.ts`.
- **`shares-code-with`** — [0372. Resolve Microsoft Store pwsh aliases](0372-resolve-microsoft-store-pwsh-aliases.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/install-lefthook.spec.ts`.
- **`shares-code-with`** — [0499. Keep supported-platform tests semantic](0499-keep-supported-platform-tests-semantic.md): Shares source implementation: `scripts/install-lefthook.spec.ts`, `scripts/oxlint-contract.spec.ts`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `scripts/install-lefthook.spec.ts`.
- **`shares-code-with`** — [0439. Private npm publication as three independent sequences](0439-private-npm-publication-as-three-independent-sequences.md): Shares source implementation: `vendor/include/src/index.ts`.
- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `scripts/change-scope.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0421-coverage-exempt-heavy-suites.md`.
