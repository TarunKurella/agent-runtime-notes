---
id: "dsh-note-0650"
title: "Drop the unconsumed `llm/adapter-change` event"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-06-20-drop-unconsumed-llm-adapter-change-event.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/simplification"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/archived"
  - "mechanism/adapter"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "llm/adapter-change"
  - "LlmService.registerAdapter"
  - "packages/*/src"
  - "examples/*/src"
  - "tools/change"
  - "system-prompt/change"
  - "llm/stream"
  - "registerAdapter"
  - "dsh-llm"
  - "interface Events"
  - "ctx.emit"
  - "Drop the unconsumed `llm/adapter-change` event"
  - "simplification"
  - "boundary"
search_regex: "(?i)(llm/adapter\\-change|LlmService\\.registerAdapter|packages/\\*/src|examples/\\*/src|tools/change|system\\-prompt/change|llm/stream|registerAdapter)"
---

# 0650. Drop the unconsumed `llm/adapter-change` event — implementation context

## Open this when

LlmService.registerAdapter() emits llm/adapter-change on registration and disposal (packages/llm/llm/src/index.ts). Grepping llm/adapter-change across packages//src and examples//src finds only the declaration, emit sites, docs, and tests; no production listener subscribes to it. This differs from tools/change and system-prompt/change. Those two events are also unconsumed today, but they are plausible registry-change signals for future live tool/prompt UIs.

## Source decision

Only llm/adapter-change is removed: the declaration in dsh-llm's interface Events, the ctx.emit('llm/adapter-change') calls, and the "Emits llm/adapter-change on registration and disposal" sentence in LlmService.registerAdapter's JSDoc. registerAdapter()'s effect generator keeps the mutation and rollback disposer for HMR/disposal but sheds the listener-throw rollback ordering that existed only for the removed event. The adapter-disposer test asserts the returned disposer removes the adapter without subscribing to the event; the listener-throw rollback test is gone with its subject.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-06-20-drop-unconsumed-llm-adapter-change-event.md](../02-notes/archived/simplification/2026-06-20-drop-unconsumed-llm-adapter-change-event.md)
- Pinned source: [.agents/notes/archived/simplification/2026-06-20-drop-unconsumed-llm-adapter-change-event.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-06-20-drop-unconsumed-llm-adapter-change-event.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `llm/stream` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/llm/llm/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/llm/llm`. | `named-file, named-package-member` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | The source note names this file directly. Core file in the package named by the note: `packages/llm/llm`. | `named-file, named-package-member` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/llm/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/package.json) | composition and configuration | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-llm` named by the note. | `exact-code-occurrence` |
| [`docs/glossary.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/glossary.md) | package contract and examples | Contains the exact code literal `dsh-llm` named by the note. | `exact-code-occurrence` |
| [`tsconfig.base.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.base.json) | composition and configuration | Contains the exact code literal `dsh-llm` named by the note. | `exact-code-occurrence` |
| [`docs/glossary.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/glossary.zh.md) | package contract and examples | Contains the exact code literal `dsh-llm` named by the note. | `exact-code-occurrence` |
| [`examples/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/package.json) | composition and configuration | Contains the exact code literal `dsh-llm` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `registerAdapter`.
- [`packages/llm/llm/tests/topology.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/topology.spec.ts) — A test under the owning area exercises or imports `registerAdapter`.
- [`packages/llm/llm/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/invariant.spec.ts) — A test under the owning area exercises or imports `registerAdapter`.
- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — Contains the exact code literal `tools/change` named by the note.
- [`packages/core/tools/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/invariant.spec.ts) — Contains the exact code literal `tools/change` named by the note.
- [`packages/typert/registry/tests/typert.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/tests/typert.spec.ts) — Contains the exact code literal `tools/change` named by the note.
- [`packages/core/system-prompt/tests/system-prompt.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/tests/system-prompt.spec.ts) — Contains the exact code literal `system-prompt/change` named by the note.
- [`packages/subagent/tool-subagent-report/tests/tool-subagent-report.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-report/tests/tool-subagent-report.spec.ts) — Contains the exact code literal `system-prompt/change` named by the note.
- Source verification intent: llm/adapter-change and its emits are gone and the regenerated cordis catalog is fresh; HMR-safety holds (disposing a contributing fiber removes the adapter); tools/change and system-prompt/change remain documented and tested; and the ACP snapshots plus the keyless Headless Loader smoke pin the unchanged production paths.

## How to read the implementation

1. Start with [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/simplification`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/testing`, `domain/web-retrieval`, `lifecycle/archived`, `mechanism/adapter`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `llm/adapter-change`, `LlmService.registerAdapter`, `packages/*/src`, `examples/*/src`, `tools/change`, `system-prompt/change`, `llm/stream`, `registerAdapter`, `dsh-llm`, `interface Events`, `ctx.emit`, `Drop the unconsumed `llm/adapter-change` event`, `simplification`, `boundary`
- Regex: `(?i)(llm/adapter\-change|LlmService\.registerAdapter|packages/\*/src|examples/\*/src|tools/change|system\-prompt/change|llm/stream|registerAdapter)`

```bash
rg -n --pcre2 "(?i)(llm/adapter\\-change|LlmService\\.registerAdapter|packages/\\*/src|examples/\\*/src|tools/change|system\\-prompt/change|llm/stream|registerAdapter)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0056. Adapter-owned reasoning effort capabilities](0056-adapter-owned-reasoning-effort-capabilities.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0001. Provider-neutral content-block vocabulary owned by dsh-llm](0001-provider-neutral-content-block-vocabulary-owned-by-dsh-llm.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0669. TUI titles come from the session-title service](0669-tui-titles-come-from-the-session-title-service.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0623. TUI model-context resolution defers on the adapter-registration race](0623-tui-model-context-resolution-defers-on-the-adapter-registration-race.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0572. Auto-title on by default, re-derived on resume](0572-auto-title-on-by-default-re-derived-on-resume.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0181. Per-provider request retry policies](0181-per-provider-request-retry-policies.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0567. Dedicated full-screen TUI front door](0567-dedicated-full-screen-tui-front-door.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0432. Unified GitHub label taxonomy](0432-unified-github-label-taxonomy.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0650-drop-the-unconsumed-llm-adapter-change-event.md`.
