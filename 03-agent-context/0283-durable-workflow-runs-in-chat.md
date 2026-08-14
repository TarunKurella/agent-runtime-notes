---
id: "dsh-note-0283"
title: "Durable workflow runs in Chat"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-10-durable-workflow-runs-in-chat.md"
implementation_evidence: "medium"
target_anchor: "repository tests and release policy"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "DisclosureRow"
  - "StateDot"
  - "phases"
  - "phase"
  - "parentId"
  - "runId"
  - "workflow/*"
  - "dsh-tool-workflow"
  - "tool-workflow/run-start"
  - "tool-workflow/run-end"
  - "run.dispose"
  - "@deepseek-ai/dsh-workflow/types"
  - "@deepseek-ai/dsh-tool-workflow/types"
  - "ui-workflow-run"
search_regex: "(?i)(DisclosureRow|StateDot|phases|phase|parentId|runId|workflow/\\*|dsh\\-tool\\-workflow)"
---

# 0283. Durable workflow runs in Chat — implementation context

## Open this when

The ordinary workflow tool row owns the model call and final tool result, but those two records do not explain which members actually started, how they were grouped, whether each member completed, failed, or was cancelled, or what remained unfinished when a process stopped. Live workflow/ events expose those facts only inside the current process, so a refresh or later Session open loses the run history. The Web Client already assembles business-owned Conversation Nodes from durable Session events.

## Source decision

dsh-tool-workflow projects every top-level accepted run into the calling Agent's Session. tool-workflow/run-start records the stable runId and validated name; matching workflow member events record the member sequence, exact label, optional exact phase, child Session id, and outcome; tool-workflow/run-end records the stop reason only after the result exists and run.dispose() has reached quiescence. Nested transport executions run normally but write no workflow record because they do not own an independent Chat row. Recording is observational.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-10-durable-workflow-runs-in-chat.md](../02-notes/implemented/feature/2026-08-10-durable-workflow-runs-in-chat.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-10-durable-workflow-runs-in-chat.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-10-durable-workflow-runs-in-chat.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/client/ui-tool/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-tool`. | `named-package-member` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/workflow/workflow/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow/src/index.ts) | package entry point | Core file in the package named by the note: `packages/workflow/workflow`. | `named-package-member` |
| [`packages/workflow/workflow/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/workflow/workflow`. | `named-package-member` |
| [`packages/client/ui-tool/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-tool`. | `named-package-member` |
| [`packages/core/agent/src/runtime-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/runtime-types.ts) | runtime implementation | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/workflow/workflow/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/workflow/workflow`. | `named-package-member` |
| [`packages/client/ui-workflow-run/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workflow-run/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-workflow-run`. | `named-package-member` |
| [`packages/workflow/tool-workflow/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/index.ts) | package entry point | Core file in the package named by the note: `packages/workflow/tool-workflow`. | `named-package-member` |
| [`packages/workflow/tool-workflow/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/workflow/tool-workflow`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `DisclosureRow` | `function` | [`packages/client/ui-primitives/src/DisclosureRow.tsx:33`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DisclosureRow.tsx#L33) | `export function DisclosureRow({` |
| `StateDot` | `function` | [`packages/client/ui-primitives/src/StateDot.tsx:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/StateDot.tsx#L24) | `export function StateDot({ state, size = 10, className }: {` |
| `phases` | `const` | [`packages/client/ui-workflow-run/src/client/workflow-definition.ts:97`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workflow-run/src/client/workflow-definition.ts#L97) | `const phases = new Map<string, { phase: string \| null; members: WorkflowRunMemberData[] }>()` |
| `phase` | `const` | [`packages/client/ui-workflow-run/src/client/workflow-definition.ts:99`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workflow-run/src/client/workflow-definition.ts#L99) | `const phase = member.phase === undefined ? null : member.phase` |
| `parentId` | `const` | [`packages/sdk/client/src/client.ts:365`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/client.ts#L365) | `const parentId = params.parentSessionId` |
| `runId` | `const` | [`packages/workflow/tool-workflow/src/invariant.ts:59`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/invariant.ts#L59) | `const runId = stringId(data.runId, \`${event.type} runId\`, fail)` |
| `runId` | `const` | [`packages/workflow/tool-workflow/src/invariant.ts:78`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/invariant.ts#L78) | `const runId = stringId(data.runId, \`${event.type} runId\`, fail)` |

### Tests and executable evidence

- [`packages/client/ui-tool/tests/tool-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/tool-row.client.spec.tsx) — A test under the owning area exercises or imports `StateDot`.
- [`packages/workflow/tool-workflow/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/tests/invariant.spec.ts) — A test under the owning area exercises or imports `dsh-tool-workflow`. A test under the owning area exercises or imports `runId`.
- [`packages/client/ui-tool/tests/diff-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/diff-card.client.spec.tsx) — A test under the owning area exercises or imports `StateDot`.
- [`packages/workflow/tool-workflow/tests/tool-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/tests/tool-workflow.spec.ts) — A test under the owning area exercises or imports `dsh-tool-workflow`. A test under the owning area exercises or imports `runId`.
- [`packages/client/ui-tool/tests/terminal-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/terminal-card.client.spec.tsx) — A test under the owning area exercises or imports `StateDot`.
- [`packages/client/ui-tool/tests/toolview-slot.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/toolview-slot.client.spec.tsx) — A test under the owning area exercises or imports `ui-tool`.
- [`packages/client/ui-tool/tests/tool-details-render.client.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/tool-details-render.client.tsx) — A test under the owning area exercises or imports `ui-tool`.
- [`packages/client/ui-primitives/tests/state-dot.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/state-dot.client.spec.tsx) — A test under the owning area exercises or imports `StateDot`.
- Source verification intent: Package tests cover top-level and nested eligibility, zero-member and concurrent runs, disposal-before-ending order, all four append-failure prefixes, and cold/live invariant rejection. Conversation tests compare complete replace, update-only prepend, and live append; they cover exact phase identity, terminal and interrupted status, disclosure state, list-fact navigation, and HMR removal and re-registration.

## How to read the implementation

1. Start with [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and release policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `DisclosureRow`, `StateDot`, `phases`, `phase`, `parentId`, `runId`, `workflow/*`, `dsh-tool-workflow`, `tool-workflow/run-start`, `tool-workflow/run-end`, `run.dispose`, `@deepseek-ai/dsh-workflow/types`, `@deepseek-ai/dsh-tool-workflow/types`, `ui-workflow-run`
- Regex: `(?i)(DisclosureRow|StateDot|phases|phase|parentId|runId|workflow/\*|dsh\-tool\-workflow)`

```bash
rg -n --pcre2 "(?i)(DisclosureRow|StateDot|phases|phase|parentId|runId|workflow/\\*|dsh\\-tool\\-workflow)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0296. Status-driven disclosure for workflow runs](0296-status-driven-disclosure-for-workflow-runs.md): The source note links to this decision directly.
- **`shares-code-with`** — [0159. Fresh-agent Ralph workflow tool](0159-fresh-agent-ralph-workflow-tool.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0363. Bounded background job admission](0363-bounded-background-job-admission.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0135. ACP subagent backend (out-of-process delegation)](0135-acp-subagent-backend-out-of-process-delegation.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0164. Same-session goal-round driver](0164-same-session-goal-round-driver.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0042. Explicit turn cancellation capability](0042-explicit-turn-cancellation-capability.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0038. Initiating Agent scope over AsyncLocalStorage](0038-initiating-agent-scope-over-asynclocalstorage.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0264. Continuable subagent current-turn interrupt](0264-continuable-subagent-current-turn-interrupt.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0283-durable-workflow-runs-in-chat.md`.
