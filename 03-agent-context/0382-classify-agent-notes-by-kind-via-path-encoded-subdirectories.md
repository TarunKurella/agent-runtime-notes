---
id: "dsh-note-0382"
title: "Classify Agent Notes by kind via path-encoded subdirectories"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-06-20-agent-note-classification.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "proposed/"
  - "implemented/"
  - "rejected/"
  - "{lifecycle}/{class}/yyyy-mm-dd-topic.md"
  - "bug-fix"
  - "implemented/process/"
  - "doc-sync"
  - "verify-md-wrap"
  - "scripts/verify-agent-note-classification.ts"
  - ".md"
  - "INDEX.md"
  - "scripts/agent-note-tree.ts"
  - "scripts/verify-doc-refs.ts"
  - ".agents/notes/implemented/testing/2026-06-19-acp-snapshot-tests.md"
search_regex: "(?i)(proposed/|implemented/|rejected/|\\{lifecycle\\}/\\{class\\}/yyyy\\-mm\\-dd\\-topic\\.md|bug\\-fix|implemented/process/|doc\\-sync|verify\\-md\\-wrap)"
---

# 0382. Classify Agent Notes by kind via path-encoded subdirectories — implementation context

## Open this when

A lifecycle-only Agent Note tree --- proposed/ / implemented/ / rejected/ --- does not record what kind of decision each file contains. A reader browsing one lifecycle cannot distinguish a new capability from a removal or a tooling-policy change without opening each file. The repo's standing bias is mechanical quality gates over prose guidelines: a convention that isn't machine-checked rots. So a classification scheme here had to be enforceable, not an honor-system header.

## Source decision

Add a second axis --- the Agent Note's class --- and encode it in the path: {lifecycle}/{class}/yyyy-mm-dd-topic.md. The folder is the label. A file's location declares its class, the closed set is "these folders and no others," and the existing verify-md-links gate already protects the path rewrites the move required. The architecture / process line: architecture is about the source we ship; process is the surrounding tooling and workflow.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-06-20-agent-note-classification.md](../02-notes/implemented/process/2026-06-20-agent-note-classification.md)
- Pinned source: [.agents/notes/implemented/process/2026-06-20-agent-note-classification.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-06-20-agent-note-classification.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/agent-note-tree.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/agent-note-tree.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/verify-doc-refs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-doc-refs.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/verify-agent-note-classification.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-agent-note-classification.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`.agents/notes`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/.agents/notes) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/package.json) | composition and configuration | Contains the exact code literal `scripts/verify-agent-note-classification.ts` named by the note. Contains the exact code literal `scripts/verify-doc-refs.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/dev-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.spec.ts) — A test under the owning area exercises or imports `mkdir`.
- [`apps/web/tests/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/README.md) — A test under the owning area exercises or imports `testing`.
- [`apps/web/tests/support.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/support.ts) — A test under the owning area exercises or imports `testing`.
- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `testing`. A test under the owning area exercises or imports `mkdir`.
- [`apps/web/tests/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/README.zh.md) — A test under the owning area exercises or imports `testing`.
- [`scripts/change-scope.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/change-scope.spec.ts) — A test under the owning area exercises or imports `feature`.
- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — A test under the owning area exercises or imports `bug-fix`.

## How to read the implementation

1. Start with [`scripts/agent-note-tree.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/agent-note-tree.ts) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`
- Aliases: `proposed/`, `implemented/`, `rejected/`, `{lifecycle}/{class}/yyyy-mm-dd-topic.md`, `bug-fix`, `implemented/process/`, `doc-sync`, `verify-md-wrap`, `scripts/verify-agent-note-classification.ts`, `.md`, `INDEX.md`, `scripts/agent-note-tree.ts`, `scripts/verify-doc-refs.ts`, `.agents/notes/implemented/testing/2026-06-19-acp-snapshot-tests.md`
- Regex: `(?i)(proposed/|implemented/|rejected/|\{lifecycle\}/\{class\}/yyyy\-mm\-dd\-topic\.md|bug\-fix|implemented/process/|doc\-sync|verify\-md\-wrap)`

```bash
rg -n --pcre2 "(?i)(proposed/|implemented/|rejected/|\\{lifecycle\\}/\\{class\\}/yyyy\\-mm\\-dd\\-topic\\.md|bug\\-fix|implemented/process/|doc\\-sync|verify\\-md\\-wrap)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0687. Agent Notes](0687-agent-notes.md): The source note links to this decision directly.
- **`source-link`** — [0377. Mechanical quality gates over prose guidelines](0377-mechanical-quality-gates-over-prose-guidelines.md): The source note links to this decision directly.
- **`source-link`** — [0381. Markdown cross-link validity linting](0381-markdown-cross-link-validity-linting.md): The source note links to this decision directly.
- **`source-link`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): The source note links to this decision directly.
- **`shares-code-with`** — [0646. Wine-run Windows blocking gates on Linux runners](0646-wine-run-windows-blocking-gates-on-linux-runners.md): Shares source implementation: `apps/web/tests/README.md`, `apps/web/tests/README.zh.md`.
- **`shares-code-with`** — [0433. Standardize Chinese contract terminology on 约定](0433-standardize-chinese-contract-terminology-on-contract.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `apps/web/tests/support.ts`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `scripts/dev-web.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `scripts/run-gates.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0382-classify-agent-notes-by-kind-via-path-encoded-subdirectories.md`.
