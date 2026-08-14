---
id: "dsh-note-0531"
title: "Mutation testing as the coverage counterweight"
status: "proposed"
class: "testing"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/testing/2026-06-11-mutation-testing.md"
implementation_evidence: "lead-only"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/testing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/llm"
  - "domain/testing"
  - "lifecycle/proposed"
  - "mechanism/policy"
aliases:
  - "@stryker-mutator/vitest-runner"
  - "packages/*/src"
  - "/* v8 ignore */"
  - "Mutation testing as the coverage counterweight"
  - "testing"
  - "evidence"
  - "lifecycle"
  - "build release"
  - "configuration"
  - "llm"
  - "proposed"
  - "policy"
search_regex: "(?i)(@stryker\\-mutator/vitest\\-runner|packages/\\*/src|/\\*[- ]v8[- ]ignore[- ]\\*/|Mutation[- ]testing[- ]as[- ]the[- ]coverage[- ]counterweight|testing|evidence|lifecycle|build[- ]release)"
---

# 0531. Mutation testing as the coverage counterweight — implementation context

## Open this when

The per-file 100% coverage gate (the quality-gates decision) proves every line executes under test --- not that any assertion would notice if the line were wrong. Under agent-written tests, coverage pressure can produce execution-without-assertion. Mutation testing measures what coverage cannot: whether the suite kills deliberately injected bugs.

## Source decision

Stryker (@stryker-mutator/vitest-runner) over packages//src: PR-scoped incremental runs (changed files only) as a CI job --- fast enough to gate merges once tuned. Nightly full runs with a tracked mutation score; start by recording, then set the threshold at the observed baseline and ratchet upward (same policy as coverage: thresholds only ever tighten). Surviving mutants are work items: an agent picks a survivor, writes the killing test, repeats --- a well-shaped autonomous loop. Equivalent mutants (provably behavior-preserving) get annotated exclusions with reasons, mirroring the / v8 ignore / policy.

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/testing/2026-06-11-mutation-testing.md](../02-notes/proposed/testing/2026-06-11-mutation-testing.md)
- Pinned source: [.agents/notes/proposed/testing/2026-06-11-mutation-testing.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/testing/2026-06-11-mutation-testing.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.md) | package contract and examples | Path shares title concepts: testing. | `title-path-lead` |
| [`docs/testing.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.zh.md) | package contract and examples | Path shares title concepts: testing. | `title-path-lead` |
| [`docs/testing.i18n.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.i18n.yaml) | composition and configuration | Path shares title concepts: testing. | `title-path-lead` |
| [`scripts/coverage-exempt.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/coverage-exempt.ts) | repository automation | Path shares title concepts: coverage. | `title-path-lead` |
| [`packages/core/tools/src/testing.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/testing.ts) | runtime implementation | Path shares title concepts: testing. | `title-path-lead` |
| [`scripts/coverage-uncovered-locations.cjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/coverage-uncovered-locations.cjs) | repository automation | Path shares title concepts: coverage. | `title-path-lead` |
| [`packages/client/ui-tool/src/client/tool/toolviews/file-mutation-row.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/toolviews/file-mutation-row.tsx) | runtime implementation | Path shares title concepts: mutation. | `title-path-lead` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/coverage-exempt.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/coverage-exempt.spec.ts) — Path shares title concepts: coverage.
- [`packages/hooks/hooks-codex/tests/coverage-cases.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/tests/coverage-cases.ts) — Path shares title concepts: coverage.
- [`packages/core/agent-loop/tests/coverage-edges.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/coverage-edges.spec.ts) — Path shares title concepts: coverage.
- [`packages/hooks/hooks-codex/tests/coverage-prompt.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/tests/coverage-prompt.spec.ts) — Path shares title concepts: coverage.
- [`packages/hooks/hooks-claude-code/tests/coverage-cases.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-claude-code/tests/coverage-cases.ts) — Path shares title concepts: coverage.
- [`packages/hooks/hooks-codex/tests/coverage-post-tool.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/tests/coverage-post-tool.spec.ts) — Path shares title concepts: coverage.
- [`packages/client/ui-tool/tests/coverage-tails.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/coverage-tails.client.spec.tsx) — Path shares title concepts: coverage.
- [`packages/hooks/hooks-claude-code/tests/coverage-stop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-claude-code/tests/coverage-stop.spec.ts) — Path shares title concepts: coverage.
- Source verification intent: A Stryker config runs over packages//src with the vitest runner; a nightly job records the mutation score, and a ratcheting threshold fails the run when the score drops below the recorded baseline. PR-scoped incremental runs gate merges once runtime is acceptable --- or are explicitly kept nightly-only, with that outcome recorded here. Equivalent mutants carry annotated exclusions with reasons, mirroring the / v8 ignore / policy.

## How to read the implementation

1. Start with [`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.md) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **Status gate** — Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/testing`, `concern/evidence`, `concern/lifecycle`, `domain/build-release`, `domain/configuration`, `domain/llm`, `domain/testing`, `lifecycle/proposed`, `mechanism/policy`
- Aliases: `@stryker-mutator/vitest-runner`, `packages/*/src`, `/* v8 ignore */`, `Mutation testing as the coverage counterweight`, `testing`, `evidence`, `lifecycle`, `build release`, `configuration`, `llm`, `proposed`, `policy`
- Regex: `(?i)(@stryker\-mutator/vitest\-runner|packages/\*/src|/\*[- ]v8[- ]ignore[- ]\*/|Mutation[- ]testing[- ]as[- ]the[- ]coverage[- ]counterweight|testing|evidence|lifecycle|build[- ]release)`

```bash
rg -n --pcre2 "(?i)(@stryker\\-mutator/vitest\\-runner|packages/\\*/src|/\\*[- ]v8[- ]ignore[- ]\\*/|Mutation[- ]testing[- ]as[- ]the[- ]coverage[- ]counterweight|testing|evidence|lifecycle|build[- ]release)" source-deepseek-harness
rg -l --fixed-strings "class/testing" 03-agent-context
```

## Connected notes

- **`source-link`** — [0377. Mechanical quality gates over prose guidelines](0377-mechanical-quality-gates-over-prose-guidelines.md): The source note links to this decision directly.
- **`same-design-pressure`** — [0398. GUI testing system --- the three-tier structure](0398-gui-testing-system-the-three-tier-structure.md): Shares design concerns: `concern/evidence`, `concern/lifecycle`, `domain/build-release`.
- **`same-design-pressure`** — [0494. Property-based testing for protocol-shaped code](0494-property-based-testing-for-protocol-shaped-code.md): Shares design concerns: `concern/evidence`, `concern/lifecycle`, `domain/build-release`.
- **`same-design-pressure`** — [0528. Prune dead public API and result fields](0528-prune-dead-public-api-and-result-fields.md): Shares design concerns: `concern/evidence`, `concern/lifecycle`, `domain/build-release`.
- **`same-design-pressure`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares design concerns: `concern/evidence`, `concern/lifecycle`, `domain/build-release`.
- **`same-design-pressure`** — [0518. Recallable compaction --- index checkpoints, a state checkpoint, and in-session history recall](0518-recallable-compaction-index-checkpoints-a-state-checkpoint-and-in-sessio.md): Shares design concerns: `concern/evidence`, `concern/lifecycle`, `domain/build-release`.
- **`same-design-pressure`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares design concerns: `concern/evidence`, `concern/lifecycle`, `domain/build-release`.
- **`same-design-pressure`** — [0514. Cordis Host/Client Dynamic Plugin Runtime](0514-cordis-host-client-dynamic-plugin-runtime.md): Shares design concerns: `concern/evidence`, `concern/lifecycle`, `domain/build-release`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0531-mutation-testing-as-the-coverage-counterweight.md`.
