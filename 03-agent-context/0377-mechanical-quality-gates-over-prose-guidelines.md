---
id: "dsh-note-0377"
title: "Mechanical quality gates over prose guidelines"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-06-11-quality-gates.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/protocols"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
aliases:
  - "json"
  - "noUncheckedIndexedAccess"
  - "exactOptionalPropertyTypes"
  - "tsconfig.json"
  - "packages/*/*/src"
  - "/* v8 ignore */"
  - "Mechanical quality gates over prose guidelines"
  - "process"
  - "boundary"
  - "compatibility"
  - "evidence"
  - "ownership"
  - "performance"
  - "recovery"
search_regex: "(?i)(json|noUncheckedIndexedAccess|exactOptionalPropertyTypes|tsconfig\\.json|packages/\\*/\\*/src|/\\*[- ]v8[- ]ignore[- ]\\*/|Mechanical[- ]quality[- ]gates[- ]over[- ]prose[- ]guidelines|boundary)"
---

# 0377. Mechanical quality gates over prose guidelines — implementation context

## Open this when

This codebase is developed primarily by coding agents. Agents follow enforced gates far more reliably than prose conventions, and "a lot of work" is not a cost argument when agents do the labor. Early evidence: tests that didn't typecheck shipped (vitest doesn't typecheck) and were only caught by a review.

## Source decision

Every mechanically checkable AGENTS.md promise gets a command that exits non-zero. CI invokes the exhaustive set, while Git hooks reserve their latency budget for cheap local defects: Max-strict TypeScript (noUncheckedIndexedAccess, exactOptionalPropertyTypes, …); examples, tests, and scripts typecheck in CI via the root no-emit tsconfig.json while package/vendor code stays behind its own project-reference boundary. Oxlint with type-aware TypeScript rules plus the @stylistic and SonarJS compatibility plugins, enforcing the house style and file-local duplicated-logic checks; vendored code excluded.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-06-11-quality-gates.md](../02-notes/implemented/process/2026-06-11-quality-gates.md)
- Pinned source: [.agents/notes/implemented/process/2026-06-11-quality-gates.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-06-11-quality-gates.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/package-graph.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-graph.ts) | repository automation | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/layout.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts) | runtime implementation | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/extensions/cordis-host-runner/src/guard.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/guard.ts) | runtime implementation | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx) | runtime implementation | Defines `json`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `json` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx:1516`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx#L1516) | `const json = parseJsonContainer(value)` |
| `json` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:851`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L851) | `const json = JSON.stringify(value, null, 2)` |
| `json` | `const` | [`packages/extensions/cordis-host-runner/src/guard.ts:525`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/guard.ts#L525) | `const json = JSON.stringify(value)` |
| `json` | `const` | [`scripts/package-graph.ts:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-graph.ts#L37) | `const json = JSON.parse(readFileSync(resolve(root, rel), 'utf8')) as {` |

### Tests and executable evidence

- [`packages/mcp/mcp-client/tests/mcp-client.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/tests/mcp-client.e2e.ts) — A test under the owning area exercises or imports `exactOptionalPropertyTypes`.
- [`packages/client/web/tests/base-styles.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/tests/base-styles.client.spec.ts) — A test under the owning area exercises or imports `noUncheckedIndexedAccess`.
- [`packages/storage/storage-domain/tests/domain.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-domain/tests/domain.spec.ts) — A test under the owning area exercises or imports `exactOptionalPropertyTypes`.
- [`packages/subagent/tool-subagent/tests/tool-subagent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/tests/tool-subagent.spec.ts) — A test under the owning area exercises or imports `exactOptionalPropertyTypes`.
- [`packages/client/ui-theme/tests/scrollbar-styles.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/scrollbar-styles.client.spec.ts) — A test under the owning area exercises or imports `noUncheckedIndexedAccess`.

## How to read the implementation

1. Start with [`scripts/package-graph.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-graph.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/process`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/protocols`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`
- Aliases: `json`, `noUncheckedIndexedAccess`, `exactOptionalPropertyTypes`, `tsconfig.json`, `packages/*/*/src`, `/* v8 ignore */`, `Mechanical quality gates over prose guidelines`, `process`, `boundary`, `compatibility`, `evidence`, `ownership`, `performance`, `recovery`
- Regex: `(?i)(json|noUncheckedIndexedAccess|exactOptionalPropertyTypes|tsconfig\.json|packages/\*/\*/src|/\*[- ]v8[- ]ignore[- ]\*/|Mechanical[- ]quality[- ]gates[- ]over[- ]prose[- ]guidelines|boundary)`

```bash
rg -n --pcre2 "(?i)(json|noUncheckedIndexedAccess|exactOptionalPropertyTypes|tsconfig\\.json|packages/\\*/\\*/src|/\\*[- ]v8[- ]ignore[- ]\\*/|Mechanical[- ]quality[- ]gates[- ]over[- ]prose[- ]guidelines|boundary)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0401. Fast local Git hooks](0401-fast-local-git-hooks.md): The source note links to this decision directly.
- **`source-link`** — [0417. Oxlint as the repository linter](0417-oxlint-as-the-repository-linter.md): The source note links to this decision directly.
- **`source-link`** — [0437. Oxlint-only fix workflow](0437-oxlint-only-fix-workflow.md): The source note links to this decision directly.
- **`source-link`** — [0531. Mutation testing as the coverage counterweight](0531-mutation-testing-as-the-coverage-counterweight.md): The source note links to this decision directly.
- **`shares-code-with`** — [0441. Python public publication workflow](0441-python-public-publication-workflow.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`.
- **`shares-code-with`** — [0411. Provision CI pnpm via pnpm/action-setup](0411-provision-ci-pnpm-via-pnpm-action-setup.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`.
- **`shares-code-with`** — [0640. doc-sync through the gate scheduler](0640-doc-sync-through-the-gate-scheduler.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`.
- **`shares-code-with`** — [0424. Exact uncovered locations on coverage failure](0424-exact-uncovered-locations-on-coverage-failure.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/layout.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0377-mechanical-quality-gates-over-prose-guidelines.md`.
