---
id: "dsh-note-0434"
title: "Cite committed artifacts, never design-session ordinals"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-09-committed-artifact-citations.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "packages/client"
  - "design §4.7"
  - "plan §1.4"
  - "T2/T5/T9"
  - "P-I"
  - "scripts/gen-doc-graphs.ts"
  - "scripts/gen-tool-catalog.ts"
  - "\\(audit [A-Z]\\d"
  - "\\bT\\d\\b"
  - "--hidden"
  - ".agents/"
  - "verify-type-equiv"
  - "gen-*"
  - "verify-translation-pairing"
search_regex: "(?i)(packages/client|design[- ]§4\\.7|plan[- ]§1\\.4|T2/T5/T9|scripts/gen\\-doc\\-graphs\\.ts|scripts/gen\\-tool\\-catalog\\.ts|\\\\\\(audit[- ]\\[A\\-Z\\]\\\\d|\\\\bT\\\\d\\\\b)"
---

# 0434. Cite committed artifacts, never design-session ordinals — implementation context

## Open this when

Large design and review sessions leave working shorthand --- decision ordinals, audit item codes, plan section numbers, task and stack ordinals, reviewer rulings --- that reads naturally while the session transcript is open and resolves to nothing after it closes. A repo-wide audit found the pattern concentrated in packages/client: bare (decision 12/16/19/20/21) citations of which only decision 21 had a committed owner, (audit C2/S1/S3/S7) codes with no audit document anywhere, design §4.7 / web2 §0 / plan §1.4 references to uncommitted drafts, plan-phase labels (T2/T5/T9, P-I, W5), stack positions ("a later PR.

## Source decision

Durable prose --- comments, JSDoc, docs, notes, test comments and titles --- cites only committed artifacts, resolvable in-repo without grep archaeology: Name the owning Agent Note (its path at least once per file, a searchable name inline), the doc page path, or a GitHub issue number. PR, commit, branch, and stack positions stay banned in docs and code per the documentation standard; issues are durable and citable, and Agent Notes and postmortems may cite merged PRs and issues as evidence, per the documentation standard's change-story routing.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-09-committed-artifact-citations.md](../02-notes/implemented/process/2026-08-09-committed-artifact-citations.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-09-committed-artifact-citations.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-09-committed-artifact-citations.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/AGENTS.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | The source note names this file directly. Contains the exact code literal `scripts/gen-doc-graphs.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`scripts/gen-tool-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-tool-catalog.ts) | repository automation | The source note names this file directly. Contains the exact code literal `scripts/gen-tool-catalog.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`.agents/skills/dsh-trim-cot-leakage/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-trim-cot-leakage/SKILL.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/client/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/client`. | `named-directory-member` |
| [`packages/client`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/package.json) | composition and configuration | Contains the exact code literal `packages/client` named by the note. Contains the exact code literal `scripts/gen-doc-graphs.ts` named by the note. | `exact-code-occurrence` |
| [`tsconfig.host.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.host.json) | composition and configuration | Contains the exact code literal `packages/client` named by the note. | `exact-code-occurrence` |
| [`docs/graph-atlas.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/graph-atlas.md) | package contract and examples | Contains the exact code literal `scripts/gen-doc-graphs.ts` named by the note. | `exact-code-occurrence` |
| [`tsconfig.client.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.client.json) | composition and configuration | Contains the exact code literal `packages/client` named by the note. | `exact-code-occurrence` |
| [`docs/module-graph.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/module-graph.md) | package contract and examples | Contains the exact code literal `packages/client` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) | package contract and examples | Contains the exact code literal `scripts/gen-tool-catalog.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/translation-brief.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-brief.spec.ts) — A test under the owning area exercises or imports `verify-translation-pairing`.
- [`scripts/translation-pairing-merge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing-merge.spec.ts) — A test under the owning area exercises or imports `verify-translation-pairing`.
- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — Contains the exact code literal `packages/client` named by the note.
- [`packages/core/tools/tests/gen-tool-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/gen-tool-catalog.spec.ts) — Contains the exact code literal `scripts/gen-tool-catalog.ts` named by the note.
- Source verification intent: The audit's grep batteries (English and Chinese, comments and prose, --hidden for .agents/) return no design-ordinal citations outside recorded fixtures, archived notes, the trim skill's own files, and this note's quoted evidence; verify-type-equiv, the gen- freshness checks, and verify-translation-pairing pin the regenerated and re-recorded surfaces. Coverage gap: no gate rejects a new ordinal citation --- review owns the rule.

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

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
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

- Tags: `class/process`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/simplification`, `domain/build-release`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `packages/client`, `design §4.7`, `plan §1.4`, `T2/T5/T9`, `P-I`, `scripts/gen-doc-graphs.ts`, `scripts/gen-tool-catalog.ts`, `\(audit [A-Z]\d`, `\bT\d\b`, `--hidden`, `.agents/`, `verify-type-equiv`, `gen-*`, `verify-translation-pairing`
- Regex: `(?i)(packages/client|design[- ]§4\.7|plan[- ]§1\.4|T2/T5/T9|scripts/gen\-doc\-graphs\.ts|scripts/gen\-tool\-catalog\.ts|\\\(audit[- ]\[A\-Z\]\\d|\\bT\\d\\b)`

```bash
rg -n --pcre2 "(?i)(packages/client|design[- ]\u00a74\\.7|plan[- ]\u00a71\\.4|T2/T5/T9|scripts/gen\\-doc\\-graphs\\.ts|scripts/gen\\-tool\\-catalog\\.ts|\\\\\\(audit[- ]\\[A\\-Z\\]\\\\d|\\\\bT\\\\d\\\\b)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): The source note links to this decision directly.
- **`shares-code-with`** — [0526. Remove the packed-session fixture branch migrator](0526-remove-the-packed-session-fixture-branch-migrator.md): Shares source implementation: `package.json`, `scripts/translation-pairing-merge.spec.ts`.
- **`shares-code-with`** — [0381. Markdown cross-link validity linting](0381-markdown-cross-link-validity-linting.md): Shares source implementation: `docs/AGENTS.md`, `scripts/gen-doc-graphs.ts`.
- **`shares-code-with`** — [0406. Briefed minimal translation updates](0406-briefed-minimal-translation-updates.md): Shares source implementation: `scripts/translation-brief.spec.ts`.
- **`shares-code-with`** — [0676. Explicit-config dsh entrypoint](0676-explicit-config-dsh-entrypoint.md): Shares source implementation: `tsconfig.host.json`.
- **`shares-code-with`** — [0358. The minimal preset owns the complete RL agent composition](0358-the-minimal-preset-owns-the-complete-rl-agent-composition.md): Shares source implementation: `scripts/gen-doc-graphs.ts`.
- **`shares-code-with`** — [0687. Agent Notes](0687-agent-notes.md): Shares source implementation: `docs/AGENTS.md`.
- **`shares-code-with`** — [0179. Web todo display --- snapshot side-effect channel + two render surfaces](0179-web-todo-display-snapshot-side-effect-channel-two-render-surfaces.md): Shares source implementation: `scripts/gen-doc-graphs.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0434-cite-committed-artifacts-never-design-session-ordinals.md`.
