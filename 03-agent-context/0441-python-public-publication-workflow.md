---
id: "dsh-note-0441"
title: "Python public publication workflow"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-11-python-publication-workflow.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "json"
  - "python-release-dry-run"
  - "python-v<repository-version>"
  - "github.repository"
  - "PYPI_PUBLISHER_REPOSITORY"
  - "PUBLIC_PYPI_RELEASE_ENABLED=true"
  - "pypi-runtime"
  - "id-token: write"
  - "SHA256SUMS"
  - "platforms.json"
  - "Python public publication workflow"
  - "process"
  - "compatibility"
  - "discovery routing"
search_regex: "(?i)(json|python\\-release\\-dry\\-run|python\\-v<repository\\-version>|github\\.repository|PYPI_PUBLISHER_REPOSITORY|PUBLIC_PYPI_RELEASE_ENABLED=true|pypi\\-runtime|id\\-token:[- ]write)"
---

# 0441. Python public publication workflow — implementation context

## Open this when

The Python SDK comprises one platform-independent client wheel and three native runtime wheels that must carry one version and become installable as a set. Public PyPI uploads expose package metadata and files immediately, cannot replace an uploaded filename, and create a temporarily unusable SDK if its exact runtime dependency has not arrived. The private repository needs to exercise the complete native build and validation sequence without publishing any artifact externally.

## Source decision

The Release (Python) GitHub workflow exposes credential-free validation to pull requests labeled python-release-dry-run and to manual runs with publish=false. Both paths call the native wheel builder for all three platforms, install the Linux release set on Python 3.10 and 3.14, download the four resulting artifacts, verify their exact filenames and package metadata, enforce PyPI's default per-file size limit, record SHA-256 hashes, and retain one aggregate release candidate.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-11-python-publication-workflow.md](../02-notes/implemented/process/2026-08-11-python-publication-workflow.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-11-python-publication-workflow.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-11-python-publication-workflow.md)
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

- [`scripts/clean.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/clean.spec.ts) — A test under the owning area exercises or imports `repository`.
- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `repository`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `python-release-dry-run`. A test under the owning area exercises or imports `repository`.
- [`scripts/release/families.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/release/families.spec.ts) — A test under the owning area exercises or imports `Release`.
- [`scripts/project-doc-site.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.spec.ts) — A test under the owning area exercises or imports `repository`.
- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — A test under the owning area exercises or imports `repository`.
- [`scripts/client-bundle-purity.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-purity.spec.ts) — A test under the owning area exercises or imports `repository`.
- [`scripts/lint-rule-fingerprint.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/lint-rule-fingerprint.spec.ts) — A test under the owning area exercises or imports `repository`.

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

- Tags: `class/process`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `json`, `python-release-dry-run`, `python-v<repository-version>`, `github.repository`, `PYPI_PUBLISHER_REPOSITORY`, `PUBLIC_PYPI_RELEASE_ENABLED=true`, `pypi-runtime`, `id-token: write`, `SHA256SUMS`, `platforms.json`, `Python public publication workflow`, `process`, `compatibility`, `discovery routing`
- Regex: `(?i)(json|python\-release\-dry\-run|python\-v<repository\-version>|github\.repository|PYPI_PUBLISHER_REPOSITORY|PUBLIC_PYPI_RELEASE_ENABLED=true|pypi\-runtime|id\-token:[- ]write)`

```bash
rg -n --pcre2 "(?i)(json|python\\-release\\-dry\\-run|python\\-v<repository\\-version>|github\\.repository|PYPI_PUBLISHER_REPOSITORY|PUBLIC_PYPI_RELEASE_ENABLED=true|pypi\\-runtime|id\\-token:[- ]write)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0411. Provision CI pnpm via pnpm/action-setup](0411-provision-ci-pnpm-via-pnpm-action-setup.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`.
- **`shares-code-with`** — [0640. doc-sync through the gate scheduler](0640-doc-sync-through-the-gate-scheduler.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`.
- **`shares-code-with`** — [0377. Mechanical quality gates over prose guidelines](0377-mechanical-quality-gates-over-prose-guidelines.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`.
- **`shares-code-with`** — [0417. Oxlint as the repository linter](0417-oxlint-as-the-repository-linter.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/layout.ts`.
- **`shares-code-with`** — [0424. Exact uncovered locations on coverage failure](0424-exact-uncovered-locations-on-coverage-failure.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/client/ui-trajectory/src/client/layout.ts`.
- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `packages/client/ui-trajectory/src/client/layout.ts`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0404. Portable pull-request CI recovery boundary](0404-portable-pull-request-ci-recovery-boundary.md): Shares source implementation: `packages/client/ui-trajectory/src/client/layout.ts`, `scripts/ci-workflow.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0441-python-public-publication-workflow.md`.
