---
id: "dsh-note-0563"
title: "Repeat-tool-call guard plugin"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-08-repeat-tool-guard.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/archived"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "next"
  - "block"
  - "PostToolDecision"
  - "additionalContexts"
  - "thresholds"
  - "argumentsPreviewChars"
  - "Scenario"
  - "include"
  - "<system-reminder>"
  - "tools/post-execute"
  - "context/message"
  - "@deepseek-ai/dsh-repeat-tool-guard"
  - "packages/guard/repeat-tool-guard/"
  - "guard/"
search_regex: "(?i)(next|block|PostToolDecision|additionalContexts|thresholds|argumentsPreviewChars|Scenario|include)"
---

# 0563. Repeat-tool-call guard plugin — implementation context

## Open this when

A model stuck in a loop re-issues the same tool call with byte-identical arguments --- re-running a failing grep, re-reading an unchanged file, polling a command that already gave its answer --- and each round trip burns tokens, wall-clock, and (for paid APIs) money without adding information. The harness has nothing that notices: the loop has no step budget, no plugin tracks call repetition, and the model only escapes when it happens to vary its own behavior.

## Source decision

The guard is a loop-hygiene plugin, not a model-facing tool. It counts consecutive calls to the same tool with identical canonical arguments and injects advisory reminders at configured thresholds. It never delays, blocks, or rewrites a call; the model decides whether to retry differently or finish. The plugin is @deepseek-ai/dsh-repeat-tool-guard at packages/guard/repeat-tool-guard/, opening the guard/ group for loop-hygiene plugins (single-package groups have precedent: the todo-write Agent Note shipped todo/tool-todo).

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-08-repeat-tool-guard.md](../02-notes/archived/feature/2026-07-08-repeat-tool-guard.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-08-repeat-tool-guard.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-08-repeat-tool-guard.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/include/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/include`. | `named-package-member` |
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/core/agent/src/runtime-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/runtime-types.ts) | runtime implementation | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent-loop/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/core/agent-loop/src/tool-calls.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/tool-calls.ts) | runtime implementation | Core file in the package named by the note: `packages/core/agent-loop`. Defines `next`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`vendor/include`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/vendor/include) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/agent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/agent-loop`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `include`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `next` | `let` | [`packages/core/agent-loop/src/tool-calls.ts:82`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/tool-calls.ts#L82) | `let next = 0` |
| `block` | `const` | [`packages/core/session/src/index.ts:343`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L343) | `const block = content[0]` |
| `PostToolDecision` | `type` | [`packages/core/tools/src/index.ts:597`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L597) | `export type PostToolDecision =` |
| `additionalContexts` | `const` | [`packages/core/tools/src/index.ts:1760`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1760) | `const additionalContexts = [` |
| `thresholds` | `const` | [`packages/guard/repeat-tool-reminder/src/index.ts:164`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/guard/repeat-tool-reminder/src/index.ts#L164) | `const thresholds = validateThresholds(config.thresholds as number[])` |
| `argumentsPreviewChars` | `const` | [`packages/guard/repeat-tool-reminder/src/index.ts:168`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/guard/repeat-tool-reminder/src/index.ts#L168) | `const argumentsPreviewChars = config.argumentsPreviewChars as number` |
| `Scenario` | `interface` | [`packages/test-support/acp-snapshot/src/suite.ts:66`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts#L66) | `export interface Scenario {` |
| `include` | `const` | [`vendor/hmr/src/index.ts:250`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L250) | `const include = entry.subtree as Include \| undefined` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/core/agent/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/agent.spec.ts) — A test under the owning area exercises or imports `WeakMap`.
- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `additionalContexts`. A test under the owning area exercises or imports `PostToolDecision`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `additionalContexts`. A test under the owning area exercises or imports `PostToolDecision`.
- [`packages/core/agent-loop/tests/tool-calls.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/tool-calls.spec.ts) — A test under the owning area exercises or imports `additionalContexts`. A test under the owning area exercises or imports `PostToolDecision`.
- [`packages/core/agent-loop/tests/tool-order.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/tool-order.spec.ts) — A test under the owning area exercises or imports `toolOrder`.
- Source verification intent: Unit: A real loop with a scripted adapter covers counting and reset rules, untracked transparency, disposal cleanup, per-agent isolation, canonical argument key order, escalation, denied calls, no-agent execution, wildcard escaping, invalid config, and downstream block or replacement decisions at per-file 100% coverage. Snapshot: The keyless repeat-tool-guard scenario makes five identical todo_write calls and pins the gentle third-call and detailed fifth-call reminders in both ACP output and the session log. The plugin is loaded in the live example but remains inert in other scenarios.

## How to read the implementation

1. Start with [`vendor/include/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/archived`, `mechanism/adapter`, `mechanism/policy`, `mechanism/registry`
- Aliases: `next`, `block`, `PostToolDecision`, `additionalContexts`, `thresholds`, `argumentsPreviewChars`, `Scenario`, `include`, `<system-reminder>`, `tools/post-execute`, `context/message`, `@deepseek-ai/dsh-repeat-tool-guard`, `packages/guard/repeat-tool-guard/`, `guard/`
- Regex: `(?i)(next|block|PostToolDecision|additionalContexts|thresholds|argumentsPreviewChars|Scenario|include)`

```bash
rg -n --pcre2 "(?i)(next|block|PostToolDecision|additionalContexts|thresholds|argumentsPreviewChars|Scenario|include)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0162. Persisted same-session goal domain](0162-persisted-same-session-goal-domain.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/agent-loop/src/invariant.ts`.
- **`shares-code-with`** — [0164. Same-session goal-round driver](0164-same-session-goal-round-driver.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0004. Microkernel --- extension via Cordis event taxonomy, one concrete loop](0004-microkernel-extension-via-cordis-event-taxonomy-one-concrete-loop.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/agent-loop/src/invariant.ts`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0159. Fresh-agent Ralph workflow tool](0159-fresh-agent-ralph-workflow-tool.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0135. ACP subagent backend (out-of-process delegation)](0135-acp-subagent-backend-out-of-process-delegation.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0047. Package-owned invariant service contract](0047-package-owned-invariant-service-contract.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/agent-loop/src/invariant.ts`.
- **`shares-code-with`** — [0363. Bounded background job admission](0363-bounded-background-job-admission.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0563-repeat-tool-call-guard-plugin.md`.
