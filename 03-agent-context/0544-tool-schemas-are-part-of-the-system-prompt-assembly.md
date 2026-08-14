---
id: "dsh-note-0544"
title: "Tool schemas are part of the system-prompt assembly"
status: "archived"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/architecture/2026-06-11-tool-schemas-in-prompt-assembly.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/tools"
  - "lifecycle/archived"
  - "mechanism/registry"
aliases:
  - "sections"
  - "extras"
  - "PromptAssembly { sections, tools }"
  - "system-prompt/assemble"
  - "Tool schemas are part of the system-prompt assembly"
  - "architecture"
  - "boundary"
  - "discovery routing"
  - "evidence"
  - "build release"
  - "context"
  - "extensions"
  - "llm"
  - "tools"
search_regex: "(?i)(sections|extras|PromptAssembly[- ]\\{[- ]sections,[- ]tools[- ]\\}|system\\-prompt/assemble|Tool[- ]schemas[- ]are[- ]part[- ]of[- ]the[- ]system\\-prompt[- ]assembly|architecture|boundary|discovery[- ]routing)"
---

# 0544. Tool schemas are part of the system-prompt assembly — implementation context

## Open this when

On the wire, tool schemas travel in a dedicated tools field of the model request, not in prompt text. Architecturally, though, "what the model is told it can do" is one coherent concern: prompt sections and the tool list are assembled from the same plugin contributions and consumed at the same moment.

## Source decision

PromptAssembly { sections, tools }: the system-prompt service collects ordered text sections AND tool schemas (the tool registry auto-contributes a provider). The loop consumes one assembly per step; adapters map sections to the provider's system slot and tools to the wire tools field. The system-prompt/assemble waterfall is therefore a single interception point for everything the model is told up front --- tool filtering (ToolSearch / progressive disclosure) is an assembly rewrite, same as prompt edits.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/architecture/2026-06-11-tool-schemas-in-prompt-assembly.md](../02-notes/archived/architecture/2026-06-11-tool-schemas-in-prompt-assembly.md)
- Pinned source: [.agents/notes/archived/architecture/2026-06-11-tool-schemas-in-prompt-assembly.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/architecture/2026-06-11-tool-schemas-in-prompt-assembly.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools) | package or module directory | The note names this package or capability. | `named-package` |
| [`scripts/gen-translation-brief.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-translation-brief.ts) | repository automation | Defines `extras`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent-loop/src/agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts) | runtime implementation | Defines `sections`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `system-prompt/assemble` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Contains the exact code literal `system-prompt/assemble` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.zh.md) | package contract and examples | Contains the exact code literal `system-prompt/assemble` named by the note. | `exact-code-occurrence` |
| [`docs/event-producer-consumer.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/event-producer-consumer.md) | package contract and examples | Contains the exact code literal `system-prompt/assemble` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `sections` | `const` | [`packages/core/agent-loop/src/agent.ts:232`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L232) | `const sections = renderContextSections(assembly)` |
| `extras` | `const` | [`scripts/gen-translation-brief.ts:141`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-translation-brief.ts#L141) | `const extras = new Set(extraIndices)` |

### Tests and executable evidence

- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `extras`.

## How to read the implementation

1. Start with [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/tools`, `lifecycle/archived`, `mechanism/registry`
- Aliases: `sections`, `extras`, `PromptAssembly { sections, tools }`, `system-prompt/assemble`, `Tool schemas are part of the system-prompt assembly`, `architecture`, `boundary`, `discovery routing`, `evidence`, `build release`, `context`, `extensions`, `llm`, `tools`
- Regex: `(?i)(sections|extras|PromptAssembly[- ]\{[- ]sections,[- ]tools[- ]\}|system\-prompt/assemble|Tool[- ]schemas[- ]are[- ]part[- ]of[- ]the[- ]system\-prompt[- ]assembly|architecture|boundary|discovery[- ]routing)`

```bash
rg -n --pcre2 "(?i)(sections|extras|PromptAssembly[- ]\\{[- ]sections,[- ]tools[- ]\\}|system\\-prompt/assemble|Tool[- ]schemas[- ]are[- ]part[- ]of[- ]the[- ]system\\-prompt[- ]assembly|architecture|boundary|discovery[- ]routing)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0051. Unified JSON-value schema DSL](0051-unified-json-value-schema-dsl.md): Shares source implementation: `packages/core/tools`, `packages/core/tools/README.md`.
- **`shares-code-with`** — [0558. Rich ACP bash rendering --- the terminal card via the `_meta` convention](0558-rich-acp-bash-rendering-the-terminal-card-via-the-meta-convention.md): Shares source implementation: `packages/core/tools/README.md`, `packages/core/tools/package.json`.
- **`shares-code-with`** — [0165. Typed tool returns in Code Mode](0165-typed-tool-returns-in-code-mode.md): Shares source implementation: `packages/core/tools`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0259. Per-agent tool presentation, and the `code` preset](0259-per-agent-tool-presentation-and-the-code-preset.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0137. The `todo_write` tool --- model task list as event-sourced session state](0137-the-todo-write-tool-model-task-list-as-event-sourced-session-state.md): Shares source implementation: `packages/core/tools`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0525. Periodic human-review maintenance for dsh-code-review](0525-periodic-human-review-maintenance-for-dsh-code-review.md): Shares source implementation: `packages/core/tools`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0188. Code Mode chat rendering --- sub-calls as native rows under the parent](0188-code-mode-chat-rendering-sub-calls-as-native-rows-under-the-parent.md): Shares source implementation: `packages/core/tools`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0187. Code Mode UI foundation --- run_code description and native-parity dispatch logging](0187-code-mode-ui-foundation-run-code-description-and-native-parity-dispatch.md): Shares source implementation: `packages/core/tools`, `packages/core/tools/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0544-tool-schemas-are-part-of-the-system-prompt-assembly.md`.
