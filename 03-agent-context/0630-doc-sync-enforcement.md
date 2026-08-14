---
id: "dsh-note-0630"
title: "Doc-sync enforcement"
status: "archived"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/process/2026-06-11-doc-sync-enforcement.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "lifecycle/archived"
  - "mechanism/adapter"
  - "mechanism/policy"
aliases:
  - "declare"
  - "paragraph"
  - "registerAdapter"
  - "interface Events"
  - "scripts/"
  - "doc-typecheck"
  - ". The temp project reuses the source"
  - "verify-event-taxonomy"
  - "packages/*/src"
  - "docs/architecture.md"
  - "tools/change"
  - "llm/adapter-change"
  - "system-prompt/change"
  - "architecture.md"
search_regex: "(?i)(declare|paragraph|registerAdapter|interface[- ]Events|scripts/|doc\\-typecheck|\\.[- ]The[- ]temp[- ]project[- ]reuses[- ]the[- ]source|verify\\-event\\-taxonomy)"
---

# 0630. Doc-sync enforcement — implementation context

## Open this when

AGENTS.md promises that docs and code stay strictly in sync, but the promise was verified by eyeball. Review caught drift twice --- a cookbook example contradicting the type policy, and a README citing the wrong registerAdapter call. Out-of-sync docs are worse than no docs, and this codebase is built primarily by agents that follow gates far more reliably than prose (mechanical quality gates). Two classes of doc drift are mechanically checkable: code blocks that no longer compile, and the event-taxonomy table that duplicates the interface Events declarations.

## Source decision

Two gates, mirroring the existing scripts/ style (tsx ESM, one job each): doc-typecheck extracts every fenced ts block from README.md, docs/, and packages//README.md, writes them to a temp project extending the root tsconfig.json, and compiles it with tsc -b. The temp project reuses the source paths map and the root project references, so documentation examples see source while vendored code remains checked under its own tsconfig settings.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/process/2026-06-11-doc-sync-enforcement.md](../02-notes/archived/process/2026-06-11-doc-sync-enforcement.md)
- Pinned source: [.agents/notes/archived/process/2026-06-11-doc-sync-enforcement.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/process/2026-06-11-doc-sync-enforcement.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/AGENTS.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `packages/AGENTS.md` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/AGENTS.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/llm/llm-pi-ai/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts) | package entry point | Defines `declare`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts) | runtime implementation | Defines `paragraph`, a construct named by the note. | `symbol-definition` |
| [`CLAUDE.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/CLAUDE.md) | package contract and examples | Contains the exact code literal `docs/architecture.md` named by the note. Contains the exact code literal `packages/AGENTS.md` named by the note. | `exact-code-occurrence` |
| [`README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/README.md) | package contract and examples | Contains the exact code literal `docs/architecture.md` named by the note. | `exact-code-occurrence` |
| [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) | package contract and examples | Contains the exact code literal `docs/architecture.md` named by the note. Contains the exact code literal `packages/AGENTS.md` named by the note. | `exact-code-occurrence` |
| [`README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/README.zh.md) | package contract and examples | Contains the exact code literal `docs/architecture.md` named by the note. | `exact-code-occurrence` |
| [`docs/subsystems/tools.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/tools.md) | package contract and examples | Contains the exact code literal `tools/change` named by the note. | `exact-code-occurrence` |
| [`scripts/verify-mermaid.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-mermaid.ts) | repository automation | Contains the exact code literal `packages/AGENTS.md` named by the note. | `exact-code-occurrence` |
| [`scripts/verify-md-wrap.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-md-wrap.ts) | repository automation | Contains the exact code literal `packages/AGENTS.md` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `declare` | `const` | [`packages/llm/llm-pi-ai/src/index.ts:125`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts#L125) | `const declare = (provider: string, displayName: string): void => {` |
| `paragraph` | `let` | [`packages/typert/generator/src/cordis-catalog.ts:432`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts#L432) | `let paragraph: string[] = []` |

### Tests and executable evidence

- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `doc-typecheck`. A test under the owning area exercises or imports `doc-sync`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `registerAdapter`.
- [`apps/web/tests/schedule-after.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/schedule-after.e2e.ts) — A test under the owning area exercises or imports `registerAdapter`.
- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `registerAdapter`.
- [`packages/llm/llm/tests/topology.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/topology.spec.ts) — A test under the owning area exercises or imports `registerAdapter`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`packages/llm/llm/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/invariant.spec.ts) — A test under the owning area exercises or imports `registerAdapter`.
- [`packages/llm/llm-retry/tests/retry.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/tests/retry.spec.ts) — A test under the owning area exercises or imports `registerAdapter`.

## How to read the implementation

1. Start with [`docs/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/AGENTS.md) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `lifecycle/archived`, `mechanism/adapter`, `mechanism/policy`
- Aliases: `declare`, `paragraph`, `registerAdapter`, `interface Events`, `scripts/`, `doc-typecheck`, `. The temp project reuses the source`, `verify-event-taxonomy`, `packages/*/src`, `docs/architecture.md`, `tools/change`, `llm/adapter-change`, `system-prompt/change`, `architecture.md`
- Regex: `(?i)(declare|paragraph|registerAdapter|interface[- ]Events|scripts/|doc\-typecheck|\.[- ]The[- ]temp[- ]project[- ]reuses[- ]the[- ]source|verify\-event\-taxonomy)`

```bash
rg -n --pcre2 "(?i)(declare|paragraph|registerAdapter|interface[- ]Events|scripts/|doc\\-typecheck|\\.[- ]The[- ]temp[- ]project[- ]reuses[- ]the[- ]source|verify\\-event\\-taxonomy)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0632. Generated cordis events + services catalog](0632-generated-cordis-events-services-catalog.md): The source note links to this decision directly.
- **`source-link`** — [0521. API extractor reports](0521-api-extractor-reports.md): The source note links to this decision directly.
- **`shares-code-with`** — [0687. Agent Notes](0687-agent-notes.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `docs/AGENTS.md`.
- **`shares-code-with`** — [0433. Standardize Chinese contract terminology on 约定](0433-standardize-chinese-contract-terminology-on-contract.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `packages/typert/generator/src/cordis-catalog.ts`.
- **`shares-code-with`** — [0650. Drop the unconsumed `llm/adapter-change` event](0650-drop-the-unconsumed-llm-adapter-change-event.md): Shares source implementation: `docs/architecture.md`, `packages/llm/llm/tests/service.spec.ts`.
- **`shares-code-with`** — [0391. A gated Known-Limitations section in every package README](0391-a-gated-known-limitations-section-in-every-package-readme.md): Shares source implementation: `docs/AGENTS.md`, `packages/AGENTS.md`.
- **`shares-code-with`** — [0430. Lightweight routine documentation translation](0430-lightweight-routine-documentation-translation.md): Shares source implementation: `docs/AGENTS.md`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0386. Documentation structure, tiers, and budgets](0386-documentation-structure-tiers-and-budgets.md): Shares source implementation: `docs/AGENTS.md`, `packages/AGENTS.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0630-doc-sync-enforcement.md`.
