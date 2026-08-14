---
id: "dsh-note-0433"
title: "Standardize Chinese contract terminology on 约定"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-09-chinese-contract-terminology.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/llm"
  - "domain/protocols"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
aliases:
  - "contract"
  - "doc-sync"
  - "git diff --check"
  - "Standardize Chinese contract terminology on 约定"
  - "process"
  - "boundary"
  - "compatibility"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "build release"
  - "llm"
  - "protocols"
  - "storage"
search_regex: "(?i)(contract|doc\\-sync|git[- ]diff[- ]\\-\\-check|Standardize[- ]Chinese[- ]contract[- ]terminology[- ]on[- ]约定|boundary|compatibility|evidence|lifecycle)"
---

# 0433. Standardize Chinese contract terminology on 约定 — implementation context

## Open this when

The Chinese documentation rendered English contract inconsistently as 契约 and 约定, sometimes within one file or paragraph. The terminology table prescribed 契约, while reviewed incremental proofreading selected the more natural engineering rendering 约定. Leaving the table and corpus split made either choice fail the repository's terminology rule and allowed later translations to reintroduce the disagreement. English convention also commonly renders as 约定.

## Source decision

The terminology source of truth defines contract as 约定 and adapter contract as 适配器约定（adapter contract） on first mention. Every active Chinese documentation pair follows that ruling; archived Agent Notes remain frozen. Unpaired bilingual calibration assets and the translation prompt's explanatory prose follow the same terms so they cannot teach the superseded rendering. The migration is semantic prose maintenance, not a rename of identifiers. Inline code, file paths, links, API names, English filenames containing contract, and machine-readable values remain unchanged.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-09-chinese-contract-terminology.md](../02-notes/implemented/process/2026-08-09-chinese-contract-terminology.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-09-chinese-contract-terminology.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-09-chinese-contract-terminology.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/api/gateway/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts) | package entry point | Defines `contract`, a construct named by the note. | `symbol-definition` |
| [`packages/extensions/tool-cordis/src/inspect.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/src/inspect.ts) | runtime implementation | Defines `contract`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts) | runtime implementation | Defines `contract`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `contract` | `const` | [`packages/api/gateway/src/client/index.ts:369`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts#L369) | `const contract = descriptor.cancellation === undefined` |
| `contract` | `const` | [`packages/extensions/tool-cordis/src/inspect.ts:241`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/src/inspect.ts#L241) | `const contract = documented.find(entry => entry.signature === signature)` |
| `contract` | `const` | [`packages/typert/generator/src/cordis-catalog.ts:768`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts#L768) | `const contract = parseJsDoc(method.jsDoc)` |
| `contract` | `const` | [`packages/typert/generator/src/cordis-catalog.ts:788`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts#L788) | `const contract = parseJsDoc(event.jsDoc)` |

### Tests and executable evidence

- [`apps/web/tests/support.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/support.ts) — A test under the owning area exercises or imports `convention`.
- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`scripts/test-invariants.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-invariants.ts) — A test under the owning area exercises or imports `convention`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `convention`.
- [`scripts/project-doc-site.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.spec.ts) — A test under the owning area exercises or imports `convention`.
- [`apps/web/tests/assembled-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/assembled-boot.ts) — A test under the owning area exercises or imports `convention`.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `convention`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- Source verification intent: The migration scans every active bilingual pair, updates each affected Chinese document, re-records its pairing sidecar, and leaves active prose with no 契约 occurrences. The pairing gate, full doc-sync, website build, translation prompt tests and snapshot, and git diff --check verify the resulting corpus and pipeline assets.

## How to read the implementation

1. Start with [`packages/api/gateway/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `domain/build-release`, `domain/llm`, `domain/protocols`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`
- Aliases: `contract`, `doc-sync`, `git diff --check`, `Standardize Chinese contract terminology on 约定`, `process`, `boundary`, `compatibility`, `evidence`, `lifecycle`, `ownership`, `build release`, `llm`, `protocols`, `storage`
- Regex: `(?i)(contract|doc\-sync|git[- ]diff[- ]\-\-check|Standardize[- ]Chinese[- ]contract[- ]terminology[- ]on[- ]约定|boundary|compatibility|evidence|lifecycle)`

```bash
rg -n --pcre2 "(?i)(contract|doc\\-sync|git[- ]diff[- ]\\-\\-check|Standardize[- ]Chinese[- ]contract[- ]terminology[- ]on[- ]\u7ea6\u5b9a|boundary|compatibility|evidence|lifecycle)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0435. Concrete prose names actors and recorded facts](0435-concrete-prose-names-actors-and-recorded-facts.md): The source note links to this decision directly.
- **`shares-code-with`** — [0359. Pre-Plugin Theme Bootstrap](0359-pre-plugin-theme-bootstrap.md): Shares source implementation: `apps/web/tests/assembled-boot.ts`, `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0687. Agent Notes](0687-agent-notes.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `apps/web/tests/support.ts`.
- **`shares-code-with`** — [0382. Classify Agent Notes by kind via path-encoded subdirectories](0382-classify-agent-notes-by-kind-via-path-encoded-subdirectories.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `apps/web/tests/support.ts`.
- **`shares-code-with`** — [0630. Doc-sync enforcement](0630-doc-sync-enforcement.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `packages/typert/generator/src/cordis-catalog.ts`.
- **`shares-code-with`** — [0276. Per-Model Reasoning Declarations in llm-pi-ai](0276-per-model-reasoning-declarations-in-llm-pi-ai.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0684. Adopt execa for hand-rolled test subprocess plumbing](0684-adopt-execa-for-hand-rolled-test-subprocess-plumbing.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `apps/web/tests/smoke-real.e2e.ts`.
- **`shares-code-with`** — [0393. Project canonical documentation into the website](0393-project-canonical-documentation-into-the-website.md): Shares source implementation: `scripts/project-doc-site.spec.ts`, `scripts/run-gates.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0433-standardize-chinese-contract-terminology-on-contract.md`.
