---
id: "dsh-note-0411"
title: "Provision CI pnpm via pnpm/action-setup"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-26-pnpm-action-setup-for-symmetric-ci-caching.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/performance"
  - "concern/recovery"
  - "concern/trust"
  - "domain/build-release"
  - "domain/llm"
  - "domain/security"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "json"
  - "landlock-run.yml"
  - "pnpm store path --silent >> $GITHUB_OUTPUT"
  - "actions/cache@v4"
  - "pnpm-lock.yaml"
  - "e2e.yml"
  - "docs-pages.yml"
  - "pi-ai-provider-e2e.yml"
  - "build-exe-for-python-sdk.yml"
  - "ci.yml"
  - "pnpm/action-setup@v4"
  - "packageManager"
  - "actions/setup-node"
  - "@yarnpkg/cli-dist"
search_regex: "(?i)(json|landlock\\-run\\.yml|pnpm[- ]store[- ]path[- ]\\-\\-silent[- ]>>[- ]\\$GITHUB_OUTPUT|actions/cache@v4|pnpm\\-lock\\.yaml|e2e\\.yml|docs\\-pages\\.yml|pi\\-ai\\-provider\\-e2e\\.yml)"
---

# 0411. Provision CI pnpm via pnpm/action-setup — implementation context

## Open this when

Outside landlock-run.yml, each workflow that installed pnpm hand-provisioned it with corepack enable, and five of them further repeated a hand-rolled cache setup --- pnpm store path --silent >> $GITHUB_OUTPUT, then actions/cache@v4 keyed on pnpm-lock.yaml: e2e.yml, docs-pages.yml, pi-ai-provider-e2e.yml, build-exe-for-python-sdk.yml, and the node-compat, serial-linux, and benchmark jobs of ci.yml.

## Source decision

pnpm/action-setup@v4 is the only pnpm provisioning mechanism in CI: no workflow runs corepack enable. The root dev dependency on @yarnpkg/cli-dist separately supplies the modern Yarn CLI exercised by the generated-project e2e; package-manager coverage therefore does not inherit the runner image's Yarn Classic. Caching remains per-job policy on top of pnpm provisioning, in three deliberate shapes: Symmetric cache (restore and save): actions/setup-node with cache: pnpm --- e2e.yml, docs-pages.yml, pi-ai-provider-e2e.yml, build-exe-for-python-sdk.yml, and the node-compat and two benchmark jobs of ci.yml.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-26-pnpm-action-setup-for-symmetric-ci-caching.md](../02-notes/implemented/process/2026-07-26-pnpm-action-setup-for-symmetric-ci-caching.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-26-pnpm-action-setup-for-symmetric-ci-caching.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-26-pnpm-action-setup-for-symmetric-ci-caching.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/package-graph.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-graph.ts) | repository automation | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/layout.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts) | runtime implementation | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/extensions/cordis-host-runner/src/guard.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/guard.ts) | runtime implementation | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx) | runtime implementation | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`.github/workflows/ci.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/ci.yml) | repository automation | Contains the exact code literal `actions/setup-node` named by the note. Contains the exact code literal `actions/cache` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/e2e.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/e2e.yml) | repository automation | Contains the exact code literal `actions/setup-node` named by the note. Contains the exact code literal `pnpm/action-setup` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/e2b-e2e.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/e2b-e2e.yml) | repository automation | Contains the exact code literal `actions/setup-node` named by the note. Contains the exact code literal `pnpm/action-setup` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/sandbox.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/sandbox.yml) | repository automation | Contains the exact code literal `actions/setup-node` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/landlock-run.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/landlock-run.yml) | repository automation | Contains the exact code literal `actions/setup-node` named by the note. Contains the exact code literal `pnpm/action-setup` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/release-vendor.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/release-vendor.yml) | repository automation | Contains the exact code literal `actions/setup-node` named by the note. Contains the exact code literal `pnpm/action-setup` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/landlock-run-release.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/landlock-run-release.yml) | repository automation | Contains the exact code literal `actions/setup-node` named by the note. Contains the exact code literal `pnpm/action-setup` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `json` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx:1516`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx#L1516) | `const json = parseJsonContainer(value)` |
| `json` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:851`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L851) | `const json = JSON.stringify(value, null, 2)` |
| `json` | `const` | [`packages/extensions/cordis-host-runner/src/guard.ts:525`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/guard.ts#L525) | `const json = JSON.stringify(value)` |
| `json` | `const` | [`scripts/package-graph.ts:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-graph.ts#L37) | `const json = JSON.parse(readFileSync(resolve(root, rel), 'utf8')) as {` |

### Tests and executable evidence

- [`scripts/dev-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `yml`. Contains the exact code literal `pnpm/action-setup` named by the note.
- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`scripts/cordis-config-files.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-config-files.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`scripts/verify-config-source-ownership.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-config-source-ownership.spec.ts) — A test under the owning area exercises or imports `yml`.

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

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/performance`, `concern/recovery`, `concern/trust`, `domain/build-release`, `domain/llm`, `domain/security`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `json`, `landlock-run.yml`, `pnpm store path --silent >> $GITHUB_OUTPUT`, `actions/cache@v4`, `pnpm-lock.yaml`, `e2e.yml`, `docs-pages.yml`, `pi-ai-provider-e2e.yml`, `build-exe-for-python-sdk.yml`, `ci.yml`, `pnpm/action-setup@v4`, `packageManager`, `actions/setup-node`, `@yarnpkg/cli-dist`
- Regex: `(?i)(json|landlock\-run\.yml|pnpm[- ]store[- ]path[- ]\-\-silent[- ]>>[- ]\$GITHUB_OUTPUT|actions/cache@v4|pnpm\-lock\.yaml|e2e\.yml|docs\-pages\.yml|pi\-ai\-provider\-e2e\.yml)`

```bash
rg -n --pcre2 "(?i)(json|landlock\\-run\\.yml|pnpm[- ]store[- ]path[- ]\\-\\-silent[- ]>>[- ]\\$GITHUB_OUTPUT|actions/cache@v4|pnpm\\-lock\\.yaml|e2e\\.yml|docs\\-pages\\.yml|pi\\-ai\\-provider\\-e2e\\.yml)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0441. Python public publication workflow](0441-python-public-publication-workflow.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`.
- **`shares-code-with`** — [0317. Isolate pnpm setup per GitHub Actions runner](0317-isolate-pnpm-setup-per-github-actions-runner.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/cordis-config-files.spec.ts`.
- **`shares-code-with`** — [0377. Mechanical quality gates over prose guidelines](0377-mechanical-quality-gates-over-prose-guidelines.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`.
- **`shares-code-with`** — [0415. Make Lefthook installation worktree-local](0415-make-lefthook-installation-worktree-local.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/dev-web.spec.ts`.
- **`shares-code-with`** — [0640. doc-sync through the gate scheduler](0640-doc-sync-through-the-gate-scheduler.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`.
- **`shares-code-with`** — [0417. Oxlint as the repository linter](0417-oxlint-as-the-repository-linter.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/layout.ts`.
- **`shares-code-with`** — [0424. Exact uncovered locations on coverage failure](0424-exact-uncovered-locations-on-coverage-failure.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/layout.ts`.
- **`shares-code-with`** — [0646. Wine-run Windows blocking gates on Linux runners](0646-wine-run-windows-blocking-gates-on-linux-runners.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/dev-web.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0411-provision-ci-pnpm-via-pnpm-action-setup.md`.
