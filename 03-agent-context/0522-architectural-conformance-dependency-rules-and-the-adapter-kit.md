---
id: "dsh-note-0522"
title: "Architectural conformance --- dependency rules and the adapter kit"
status: "proposed"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/process/2026-06-11-architectural-conformance.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/testing"
  - "lifecycle/proposed"
  - "mechanism/adapter"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "finish"
  - "packages/*"
  - "@deepseek-ai/dsh-agent-loop"
  - "@deepseek-ai/dsh-*/src/..."
  - "vendor/*"
  - "@deepseek-ai/dsh-llm/conformance"
  - "block-end"
  - "tool-call-delta"
  - "strictAdapter"
  - "dsh-*"
  - "Architectural conformance --- dependency rules and the adapter kit"
  - "process"
  - "boundary"
  - "evidence"
search_regex: "(?i)(finish|packages/\\*|@deepseek\\-ai/dsh\\-agent\\-loop|@deepseek\\-ai/dsh\\-\\*/src/\\.\\.\\.|vendor/\\*|@deepseek\\-ai/dsh\\-llm/conformance|block\\-end|tool\\-call\\-delta)"
---

# 0522. Architectural conformance --- dependency rules and the adapter kit — implementation context

## Open this when

Two architectural guarantees currently live only in prose: (1) nothing depends on the concrete loop package (the microkernel promise), and (2) every LlmAdapter speaks the chunk protocol correctly. Both should be mechanical (the quality-gates principle).

## Source decision

dependency-cruiser with rules: packages/ (except agent-loop's own tests and examples/) must not import @deepseek-ai/dsh-agent-loop. No cross-package deep imports (@deepseek-ai/dsh-/src/... paths) --- public entry points only. No import cycles anywhere in packages/. vendor/ must not import from packages/. Layering: dsh-llm imports nothing from other dsh packages; dsh-session only dsh-llm; etc. (the dependency table in packages/README.md, enforced).

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/process/2026-06-11-architectural-conformance.md](../02-notes/proposed/process/2026-06-11-architectural-conformance.md)
- Pinned source: [.agents/notes/proposed/process/2026-06-11-architectural-conformance.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/process/2026-06-11-architectural-conformance.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages`. The source note names this file directly. | `named-directory-member, named-file` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/agent-loop/src/agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages`. Core file in the package named by the note: `packages/core/agent-loop`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/agent-loop/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/core/tools/src/ts-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/ts-types.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages`. | `named-directory-member` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages`. | `named-directory-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `finish` | `const` | [`packages/core/agent-loop/src/agent.ts:353`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L353) | `const finish = assembler.finish` |

### Tests and executable evidence

- [`packages/llm/llm/tests/assembler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/assembler.spec.ts) — A test under the owning area exercises or imports `tool-call-delta`.
- [`packages/llm/llm/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/invariant.spec.ts) — A test under the owning area exercises or imports `tool-call-delta`.
- [`packages/llm/llm/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/properties.spec.ts) — A test under the owning area exercises or imports `tool-call-delta`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `tool-call-delta`.
- [`packages/core/agent-loop/tests/mock-adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/mock-adapter.ts) — A test under the owning area exercises or imports `tool-call-delta`.
- [`packages/core/session/tests/chunk-rows.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/chunk-rows.spec.ts) — A test under the owning area exercises or imports `tool-call-delta`.
- [`packages/core/agent-loop/tests/tool-calls.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/tool-calls.spec.ts) — A test under the owning area exercises or imports `block-end`.
- Source verification intent: dependency-cruiser runs in CI with the rule families above; a violating import fails the build. The conformance kit runs against the mock adapter and both shipping adapters, and a new adapter package inherits the suite by invoking it with its factory.

## How to read the implementation

1. Start with [`packages/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/README.md) because it has the strongest evidence link to the note.
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
- **Status gate** — Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/evidence`, `concern/ownership`, `domain/build-release`, `domain/configuration`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/testing`, `lifecycle/proposed`, `mechanism/adapter`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `finish`, `packages/*`, `@deepseek-ai/dsh-agent-loop`, `@deepseek-ai/dsh-*/src/...`, `vendor/*`, `@deepseek-ai/dsh-llm/conformance`, `block-end`, `tool-call-delta`, `strictAdapter`, `dsh-*`, `Architectural conformance --- dependency rules and the adapter kit`, `process`, `boundary`, `evidence`
- Regex: `(?i)(finish|packages/\*|@deepseek\-ai/dsh\-agent\-loop|@deepseek\-ai/dsh\-\*/src/\.\.\.|vendor/\*|@deepseek\-ai/dsh\-llm/conformance|block\-end|tool\-call\-delta)`

```bash
rg -n --pcre2 "(?i)(finish|packages/\\*|@deepseek\\-ai/dsh\\-agent\\-loop|@deepseek\\-ai/dsh\\-\\*/src/\\.\\.\\.|vendor/\\*|@deepseek\\-ai/dsh\\-llm/conformance|block\\-end|tool\\-call\\-delta)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0002. Source-owned session immutability and dev-mode invariants](0002-source-owned-session-immutability-and-dev-mode-invariants.md): The source note links to this decision directly.
- **`source-link`** — [0004. Microkernel --- extension via Cordis event taxonomy, one concrete loop](0004-microkernel-extension-via-cordis-event-taxonomy-one-concrete-loop.md): The source note links to this decision directly.
- **`source-link`** — [0377. Mechanical quality gates over prose guidelines](0377-mechanical-quality-gates-over-prose-guidelines.md): The source note links to this decision directly.
- **`shares-code-with`** — [0025. Every LLM request is reconstructable from the session log](0025-every-llm-request-is-reconstructable-from-the-session-log.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/core/agent-loop/src/index.ts`.
- **`shares-code-with`** — [0298. Whole-page image drop, projected intake limits, and thumbnail tiling](0298-whole-page-image-drop-projected-intake-limits-and-thumbnail-tiling.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0498. Per-session snapshot replay for nested agents](0498-per-session-snapshot-replay-for-nested-agents.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0522-architectural-conformance-dependency-rules-and-the-adapter-kit.md`.
