---
id: "dsh-note-0186"
title: "Spilling the durable copy of Code Mode sub-dispatch results"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-26-code-dispatch-log-spill.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/recovery"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "RunCodeBridgeOptions"
  - "CodeDispatchLog"
  - "maxParallelSubCalls"
  - "maxInlineBytes"
  - "spillStore"
  - "tool/code-dispatch"
  - "run_code"
  - "tools/code-dispatch-log"
  - "shapeDispatchLog"
  - "tool/result"
  - "dsh-spill-policy"
  - "ctx.spillStore"
  - "code-mode.ts"
  - "tools/post-execute"
search_regex: "(?i)(RunCodeBridgeOptions|CodeDispatchLog|maxParallelSubCalls|maxInlineBytes|spillStore|tool/code\\-dispatch|run_code|tools/code\\-dispatch\\-log)"
---

# 0186. Spilling the durable copy of Code Mode sub-dispatch results — implementation context

## Open this when

After full-content dispatch logging was added, a run_code program that reads a large file wrote the complete rendered text into the session log without a limit or spill policy, while native results were limited to maxInlineBytes before logging. This treated the most likely large results differently: sub-calls are intended for bulk data work, and each affected turn added megabytes to the JSONL.

## Source decision

A tools/code-dispatch-log waterfall on the registry, with spill policy as its first listener. Extension point: tools/code-dispatch-log is a scope-filtered waterfall that the bridge runs over each settled sub-dispatch before appending tool/code-dispatch. The bridge receives the registry's private shapeDispatchLog invoker as a capability closure in RunCodeBridgeOptions; the waterfall is the public contract, and the invoker does not add a service method. If a listener throws, the invoker reports any thrown value safely and uses the original settled content.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-26-code-dispatch-log-spill.md](../02-notes/implemented/feature/2026-07-26-code-dispatch-log-spill.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-26-code-dispatch-log-spill.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-26-code-dispatch-log-spill.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/spill/spill-policy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/spill/spill-policy`. Defines `maxInlineBytes`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/spill/spill-policy/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/spill/spill-policy`. | `named-package-member` |
| [`packages/spill/spill-policy/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/spill/spill-policy`. | `named-package-member` |
| [`packages/core/agent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/spill/spill-policy`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `CodeDispatchLog`, a construct named by the note. Defines `maxParallelSubCalls`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/code-mode.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts) | runtime implementation | Defines `RunCodeBridgeOptions`, a construct named by the note. Contains the exact code literal `tools/code-dispatch-log` named by the note. | `exact-code-occurrence, symbol-definition` |
| [`packages/core/agent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `RunCodeBridgeOptions` | `interface` | [`packages/core/tools/src/code-mode.ts:267`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L267) | `export interface RunCodeBridgeOptions {` |
| `CodeDispatchLog` | `interface` | [`packages/core/tools/src/index.ts:357`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L357) | `export interface CodeDispatchLog {` |
| `maxParallelSubCalls` | `const` | [`packages/core/tools/src/index.ts:776`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L776) | `const maxParallelSubCalls = value ?? 10` |
| `maxInlineBytes` | `const` | [`packages/spill/spill-policy/src/index.ts:111`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/src/index.ts#L111) | `const maxInlineBytes = config.maxInlineBytes` |
| `spillStore` | `const` | [`packages/spill/spill-policy/src/index.ts:142`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/src/index.ts#L142) | `const spillStore = ctx.get('spillStore')` |

### Tests and executable evidence

- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `maxParallelSubCalls`. Contains the exact code literal `tools/code-dispatch-log` named by the note.
- [`packages/spill/spill-policy/tests/spill-policy.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/tests/spill-policy.spec.ts) — A test under the owning area exercises or imports `maxInlineBytes`. A test under the owning area exercises or imports `maxParallelSubCalls`.

## How to read the implementation

1. Start with [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
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

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/recovery`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/storage`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `RunCodeBridgeOptions`, `CodeDispatchLog`, `maxParallelSubCalls`, `maxInlineBytes`, `spillStore`, `tool/code-dispatch`, `run_code`, `tools/code-dispatch-log`, `shapeDispatchLog`, `tool/result`, `dsh-spill-policy`, `ctx.spillStore`, `code-mode.ts`, `tools/post-execute`
- Regex: `(?i)(RunCodeBridgeOptions|CodeDispatchLog|maxParallelSubCalls|maxInlineBytes|spillStore|tool/code\-dispatch|run_code|tools/code\-dispatch\-log)`

```bash
rg -n --pcre2 "(?i)(RunCodeBridgeOptions|CodeDispatchLog|maxParallelSubCalls|maxInlineBytes|spillStore|tool/code\\-dispatch|run_code|tools/code\\-dispatch\\-log)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0187. Code Mode UI foundation --- run_code description and native-parity dispatch logging](0187-code-mode-ui-foundation-run-code-description-and-native-parity-dispatch.md): The source note links to this decision directly.
- **`source-link`** — [0189. Code Mode live dispatch lifecycle and native-contract parallelism](0189-code-mode-live-dispatch-lifecycle-and-native-contract-parallelism.md): The source note links to this decision directly.
- **`shares-code-with`** — [0159. Fresh-agent Ralph workflow tool](0159-fresh-agent-ralph-workflow-tool.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0144. The approval seam --- one-shot permission decisions over a waterfall of answerers](0144-the-approval-seam-one-shot-permission-decisions-over-a-waterfall-of-answ.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0021. Event-domain semantics --- session is the fact log, agent is the live event channel](0021-event-domain-semantics-session-is-the-fact-log-agent-is-the-live-event-c.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0138. dsh-hooks-claude-code + dsh-hooks-codex --- the Claude Code / Codex hook bridges](0138-dsh-hooks-claude-code-dsh-hooks-codex-the-claude-code-codex-hook-bridges.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0180. Model-facing session query tools](0180-model-facing-session-query-tools.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0140. Interception extension points --- the typed-Decision surface a hook programs against](0140-interception-extension-points-the-typed-decision-surface-a-hook-programs.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0186-spilling-the-durable-copy-of-code-mode-sub-dispatch-results.md`.
