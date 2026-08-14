---
id: "dsh-note-0278"
title: "Web background-job display"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-08-web-background-job-display.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "sessionId"
  - "json"
  - "actions"
  - "SessionManager"
  - "SessionListState"
  - "jobs"
  - "SubagentCatalogAction"
  - "useSessions"
  - "list"
  - "JobView"
  - "ownerSession"
  - "detail"
  - "JobId"
  - "JobRegistry"
search_regex: "(?i)(sessionId|json|actions|SessionManager|SessionListState|jobs|SubagentCatalogAction|useSessions)"
---

# 0278. Web background-job display — implementation context

## Open this when

ctx.jobs already runs every long-lived piece of work the harness starts in the background --- bash, pwsh, pty-send, and one-shot background subagents --- but its only reader was the model. dsh-tool-jobs exposes job_list, job_output, and job_kill, and nothing else observed the registry. A human at the Web client therefore could not see that a build was running, could not distinguish a finished task from a stuck one, and could not stop one. The only trace was the run_in_background tool card that printed a job id somewhere earlier in the transcript, and that card never updates again.

## Source decision

Task state reaches the browser as one whole-snapshot mux frame per session, pushed at every registry commit point that changes what that session can see. The client keeps a last-wins mirror; a header action renders it. There is no RPC, no polling, and no client-side staleness bookkeeping. This ships the list alone. Per-task streamed output and a human-initiated cancellation are separate phases, and the channel is shaped so neither has to undo it.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-08-web-background-job-display.md](../02-notes/implemented/feature/2026-08-08-web-background-job-display.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-08-web-background-job-display.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-08-web-background-job-display.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/spill/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/jobs/jobs/src/brand.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/brand.ts) | runtime implementation | The source note names this file directly. Core file in the package named by the note: `packages/jobs/jobs`. | `named-file, named-package-member, symbol-definition` |
| [`packages/client/ui-jobs/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-jobs/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/client/ui-jobs`. | `exact-code-occurrence, named-file, named-package-member` |
| [`packages/jobs/tool-jobs/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/jobs/tool-jobs`. | `named-file, named-package-member` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | The source note names this file directly. Core file in the package named by the note: `packages/core/session`. | `named-file, named-package-member` |
| [`packages/client/ui-subagent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-subagent/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/client/ui-subagent`. | `exact-code-occurrence, named-file, named-package-member` |
| [`packages/host/apiproxy/src/api/jobs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/jobs.ts) | runtime implementation | The source note names this file directly. Defines `JobView`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/api/remotes/src/agent-lookup.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/src/agent-lookup.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/host/apiproxy/src/api/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/events.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/client/runtime/src/client/sessions/manager.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts) | runtime implementation | The source note names this file directly. Defines `SessionManager`, a construct named by the note. | `exact-code-occurrence, named-file, symbol-definition` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `sessionId` | `const` | [`packages/acp/acp/src/index.ts:254`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L254) | `const sessionId = SessionId(randomUUID())` |
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `actions` | `const` | [`packages/client/runtime/src/client/contract/store.ts:218`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/store.ts#L218) | `const actions = {} as Record<string, (...params: unknown[]) => void>` |
| `SessionManager` | `class` | [`packages/client/runtime/src/client/sessions/manager.ts:106`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts#L106) | `export class SessionManager {` |
| `SessionListState` | `interface` | [`packages/client/runtime/src/client/sessions/service.ts:80`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/service.ts#L80) | `export interface SessionListState {` |
| `jobs` | `const` | [`packages/client/ui-jobs/src/client/JobListAction.tsx:95`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-jobs/src/client/JobListAction.tsx#L95) | `const jobs = useSessions(state => state.jobsBySession[sessionId]) ?? NO_TASKS` |
| `SubagentCatalogAction` | `function` | [`packages/client/ui-subagent/src/client/SubagentCatalogAction.tsx:413`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-subagent/src/client/SubagentCatalogAction.tsx#L413) | `export function SubagentCatalogAction({` |
| `useSessions` | `const` | [`packages/client/web/src/app.tsx:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/app.tsx#L30) | `const useSessions = bindSnapshotSelector(sessions.list)` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `JobView` | `interface` | [`packages/host/apiproxy/src/api/jobs.ts:17`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/jobs.ts#L17) | `export interface JobView {` |
| `ownerSession` | `const` | [`packages/jobs/jobs-local/src/index.ts:364`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs-local/src/index.ts#L364) | `const ownerSession = job.owner?.id` |
| `detail` | `const` | [`packages/jobs/jobs-local/src/index.ts:526`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs-local/src/index.ts#L526) | `const detail = \`cancel threw during teardown; work may be orphaned: ${String(error)}\`` |
| `JobId` | `type` | [`packages/jobs/jobs/src/brand.ts:19`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/brand.ts#L19) | `export type JobId = Branded<'JobId'>` |
| `JobId` | `function` | [`packages/jobs/jobs/src/brand.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/brand.ts#L26) | `export function JobId(id: string): JobId {` |
| `JobRegistry` | `class` | [`packages/jobs/jobs/src/index.ts:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/index.ts#L62) | `export abstract class JobRegistry extends Service {` |
| `JobKind` | `type` | [`packages/jobs/jobs/src/types.ts:29`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/types.ts#L29) | `export type JobKind = JobKindMap[keyof JobKindMap]` |

### Tests and executable evidence

- [`apps/web/tests/background-job-list.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/background-job-list.e2e.ts) — The source note names this file directly. Contains the exact code literal `dsh-jobs` named by the note.
- [`packages/jobs/jobs-local/tests/jobs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs-local/tests/jobs.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `dsh-tool-jobs`.
- [`packages/host/apiproxy/tests/api-proxy-jobs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-jobs.spec.ts) — The source note names this file directly.
- [`packages/jobs/jobs/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/tests/service.spec.ts) — A test under the owning area exercises or imports `JobId`. A test under the owning area exercises or imports `dsh-jobs`.
- [`packages/jobs/jobs/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/tests/invariant.spec.ts) — A test under the owning area exercises or imports `JobId`. A test under the owning area exercises or imports `dsh-jobs`.
- [`packages/jobs/tool-jobs/tests/tool-jobs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/tests/tool-jobs.spec.ts) — A test under the owning area exercises or imports `pty-send`. A test under the owning area exercises or imports `dsh-tool-jobs`.
- [`packages/todo/tool-todo/tests/tool-todo.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/tool-todo.spec.ts) — A test under the owning area exercises or imports `dsh-tool-todo`.
- [`packages/todo/tool-todo/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/invariant.spec.ts) — A test under the owning area exercises or imports `dsh-tool-todo`.
- Source verification intent: The web e2e scenario is the end-to-end proof and runs keyless: a real run_in_background bash call registers with ctx.jobs, the header count and row appear with no user interaction, and killing the task through the registry flips the open list to its producer detail. It asserts the whole delivery path rather than any single layer. Below it, jobs-local pins the change feed at all four commit points, its containment of a throwing observer, and its removal on both explicit disposal and fiber teardown; api-proxy-jobs pins the baseline-only-when-non-empty rule, the three change pushes, the dropped internal fields.

## How to read the implementation

1. Start with [`packages/spill/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/README.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `sessionId`, `json`, `actions`, `SessionManager`, `SessionListState`, `jobs`, `SubagentCatalogAction`, `useSessions`, `list`, `JobView`, `ownerSession`, `detail`, `JobId`, `JobRegistry`
- Regex: `(?i)(sessionId|json|actions|SessionManager|SessionListState|jobs|SubagentCatalogAction|useSessions)`

```bash
rg -n --pcre2 "(?i)(sessionId|json|actions|SessionManager|SessionListState|jobs|SubagentCatalogAction|useSessions)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0165. Typed tool returns in Code Mode](0165-typed-tool-returns-in-code-mode.md): Shares source implementation: `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): Shares source implementation: `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0061. Web client Agent-scope parity model and the provisioning channel (agents/scope / blank reuse / provide)](0061-web-client-agent-scope-parity-model-and-the-provisioning-channel-agents.md): Shares source implementation: `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0045. Web client architecture --- the client cordis plugin tree, the slot system, and the React-free object layer](0045-web-client-architecture-the-client-cordis-plugin-tree-the-slot-system-an.md): Shares source implementation: `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0035. Agent-scope runtime design and correctness](0035-agent-scope-runtime-design-and-correctness.md): Shares source implementation: `packages/core/session/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0278-web-background-job-display.md`.
