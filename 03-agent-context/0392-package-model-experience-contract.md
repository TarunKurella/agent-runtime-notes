---
id: "dsh-note-0392"
title: "Package Model Experience contract"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-12-package-model-experience-contract.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "markdown"
  - "NO_MODEL_EXPERIENCE_SECTION"
  - "## Known Limitations and Deferred Work"
  - "KV Cache effect"
  - "verify-package-readme-model-experience"
  - "doc-sync"
  - "Package Model Experience contract"
  - "process"
  - "boundary"
  - "concurrency"
  - "discovery routing"
  - "evidence"
  - "lifecycle"
  - "ownership"
search_regex: "(?i)(markdown|NO_MODEL_EXPERIENCE_SECTION|\\#\\#[- ]Known[- ]Limitations[- ]and[- ]Deferred[- ]Work|KV[- ]Cache[- ]effect|verify\\-package\\-readme\\-model\\-experience|doc\\-sync|Package[- ]Model[- ]Experience[- ]contract|boundary)"
---

# 0392. Package Model Experience contract — implementation context

## Open this when

A package README can explain APIs and runtime mechanics without answering the questions that dominate an agent harness's behavior and cost: what from this package reaches a model request, under which conditions, how long those tokens remain, and whether later requests preserve a reusable KV-cache prefix. The omission is especially hard to audit in a plugin architecture.

## Source decision

Every workspace package README with a model-facing or model-adjacent contract ends with the canonical Model Experience section, immediately before ## Known Limitations and Deferred Work; a package on the no-limitations allowlist ends with Model Experience itself. An audited model-agnostic generic package omits the section through NO_MODEL_EXPERIENCE_SECTION. Packages with direct, conditional, capped, lifetime, multi-surface, or auxiliary-model effects use one H3 per context surface.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-12-package-model-experience-contract.md](../02-notes/implemented/process/2026-07-12-package-model-experience-contract.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-12-package-model-experience-contract.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-12-package-model-experience-contract.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/cookbook/adding-a-package.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cookbook/adding-a-package.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`scripts/rescope-vendor.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/rescope-vendor.ts) | repository automation | Defines `markdown`, a construct named by the note. | `symbol-definition` |
| [`scripts/project-doc-site.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.ts) | repository automation | Defines `markdown`, a construct named by the note. | `symbol-definition` |
| [`scripts/verify-package-readme-model-experience.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-package-readme-model-experience.ts) | repository automation | Defines `NO_MODEL_EXPERIENCE_SECTION`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx) | runtime implementation | Defines `markdown`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `markdown` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx:933`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx#L933) | `const markdown = cell.kind === 'user' \|\| cell.kind === 'context'` |
| `markdown` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx:1544`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx#L1544) | `const markdown = (` |
| `markdown` | `const` | [`scripts/project-doc-site.ts:194`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.ts#L194) | `const markdown = resolve(dirname(sourceAbs), \`${decoded}.md\`)` |
| `markdown` | `const` | [`scripts/project-doc-site.ts:430`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.ts#L430) | `const markdown = readFileSync(sourceAbs, 'utf8')` |
| `markdown` | `const` | [`scripts/rescope-vendor.ts:532`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/rescope-vendor.ts#L532) | `const markdown = file.endsWith('.md')` |
| `NO_MODEL_EXPERIENCE_SECTION` | `const` | [`scripts/verify-package-readme-model-experience.ts:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-package-readme-model-experience.ts#L32) | `const NO_MODEL_EXPERIENCE_SECTION: Readonly<Record<string, string>> = {` |

### Tests and executable evidence

- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `doc-sync`.

## How to read the implementation

1. Start with [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) because it has the strongest evidence link to the note.
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
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/process`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`
- Aliases: `markdown`, `NO_MODEL_EXPERIENCE_SECTION`, `## Known Limitations and Deferred Work`, `KV Cache effect`, `verify-package-readme-model-experience`, `doc-sync`, `Package Model Experience contract`, `process`, `boundary`, `concurrency`, `discovery routing`, `evidence`, `lifecycle`, `ownership`
- Regex: `(?i)(markdown|NO_MODEL_EXPERIENCE_SECTION|\#\#[- ]Known[- ]Limitations[- ]and[- ]Deferred[- ]Work|KV[- ]Cache[- ]effect|verify\-package\-readme\-model\-experience|doc\-sync|Package[- ]Model[- ]Experience[- ]contract|boundary)`

```bash
rg -n --pcre2 "(?i)(markdown|NO_MODEL_EXPERIENCE_SECTION|\\#\\#[- ]Known[- ]Limitations[- ]and[- ]Deferred[- ]Work|KV[- ]Cache[- ]effect|verify\\-package\\-readme\\-model\\-experience|doc\\-sync|Package[- ]Model[- ]Experience[- ]contract|boundary)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0393. Project canonical documentation into the website](0393-project-canonical-documentation-into-the-website.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/project-doc-site.ts`.
- **`shares-code-with`** — [0126. Repository naming contract and pre-release rename ledger](0126-repository-naming-contract-and-pre-release-rename-ledger.md): Shares source implementation: `docs/cookbook/adding-a-package.md`.
- **`shares-code-with`** — [0406. Briefed minimal translation updates](0406-briefed-minimal-translation-updates.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0640. doc-sync through the gate scheduler](0640-doc-sync-through-the-gate-scheduler.md): Shares source implementation: `packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`, `scripts/gen-third-party-notices.spec.ts`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0390. Parallel pre-push gates](0390-parallel-pre-push-gates.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0430. Lightweight routine documentation translation](0430-lightweight-routine-documentation-translation.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `scripts/run-gates.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0392-package-model-experience-contract.md`.
