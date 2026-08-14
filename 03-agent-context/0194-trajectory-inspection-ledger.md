---
id: "dsh-note-0194"
title: "Trajectory inspection ledger"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-27-trajectory-inspection-ledger.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
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
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "startedAt"
  - "ConversationNodeAssembler"
  - "views"
  - "ConversationRoot"
  - "Session.views"
  - "focus-within"
  - "data-conversation-composer-overlay"
  - "Trajectory inspection ledger"
  - "feature"
  - "boundary"
  - "compatibility"
  - "discovery routing"
  - "evidence"
  - "human control"
search_regex: "(?i)(startedAt|ConversationNodeAssembler|views|ConversationRoot|Session\\.views|focus\\-within|data\\-conversation\\-composer\\-overlay|Trajectory[- ]inspection[- ]ledger)"
---

# 0194. Trajectory inspection ledger — implementation context

## Open this when

Trajectory has to make prose, machine payloads, token usage, timing, and nested tool activity readable in the same viewport. The earlier stacked Turn and Step cards preserved hierarchy but spent too much vertical space on repeated chrome, while a completely flat table would erase the causal structure that makes a trajectory useful. Role colors also risked borrowing success and warning semantics, which made visual decoration indistinguishable from runtime state.

## Source decision

Render a compact, turn-aware event ledger with a local record inspector, using the existing DeepSeek design system. The ledger keeps materialized business records in Session Event order within the loaded window. Turn boundaries use a slightly heavier rule, the raw Turn id, and a continuous left rail; Request boundaries appear as small points integrated into that structure and use one chronological numbering space across ordinary and compaction requests. Event kind and content form the two stable columns.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-27-trajectory-inspection-ledger.md](../02-notes/implemented/feature/2026-07-27-trajectory-inspection-ledger.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-27-trajectory-inspection-ledger.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-27-trajectory-inspection-ledger.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/run-gates.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts) | repository automation | Defines `startedAt`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `views`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `startedAt`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/layout.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts) | runtime implementation | Defines `startedAt`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/apply.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts) | runtime implementation | Defines `views`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx) | runtime implementation | Defines `startedAt`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/sessions/conversation-assembler.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation-assembler.ts) | runtime implementation | Defines `ConversationNodeAssembler`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx) | runtime implementation | Defines `ConversationRoot`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `startedAt` | `const` | [`packages/client/connection/src/client/fixture.ts:2022`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L2022) | `const startedAt = Date.now()` |
| `ConversationNodeAssembler` | `class` | [`packages/client/runtime/src/client/sessions/conversation-assembler.ts:137`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation-assembler.ts#L137) | `export class ConversationNodeAssembler implements ConversationViewSnapshotStore {` |
| `views` | `const` | [`packages/client/ui-conversation/src/client/apply.ts:162`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts#L162) | `const views = {` |
| `ConversationRoot` | `function` | [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx#L15) | `export function ConversationRoot({` |
| `startedAt` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx:74`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx#L74) | `const startedAt = cell.startedAt === null \|\| !Number.isFinite(cell.startedAt)` |
| `startedAt` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:149`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L149) | `const startedAt = finiteTime(result.callTime)` |
| `startedAt` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:153`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L153) | `const startedAt = finiteTime(call.time)` |
| `views` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:3376`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L3376) | `const views: ConfigurableProviderView[] = directory.map(entry => ({` |
| `views` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:3464`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L3464) | `const views = jobViews(jobs.list(ctx.agents.get(session.id)))` |
| `views` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:3501`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L3501) | `const views = jobs === undefined ? [] : jobViews(jobs.list(ctx.agents.get(session.id)))` |
| `startedAt` | `const` | [`scripts/run-gates.ts:89`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts#L89) | `const startedAt = performance.now()` |

### Tests and executable evidence

- [`apps/web/tests/message-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/message-actions.e2e.ts) — A test under the owning area exercises or imports `focus-within`.
- [`apps/web/tests/composer-tab-geometry.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/composer-tab-geometry.e2e.ts) — A test under the owning area exercises or imports `data-conversation-composer-overlay`.
- [`packages/client/ui-trajectory/tests/views.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/tests/views.client.spec.tsx) — A test under the owning area exercises or imports `startedAt`. A test under the owning area exercises or imports `data-conversation-composer-overlay`.
- [`packages/client/ui-trajectory/tests/layout.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/tests/layout.client.spec.tsx) — A test under the owning area exercises or imports `startedAt`.
- [`packages/client/ui-conversation/tests/skeleton.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/skeleton.client.spec.tsx) — A test under the owning area exercises or imports `ConversationRoot`.
- [`packages/client/runtime/tests/conversation-assembler.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/conversation-assembler.client.spec.ts) — A test under the owning area exercises or imports `ConversationNodeAssembler`.
- [`packages/client/ui-trajectory/tests/snapshot-builder.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/tests/snapshot-builder.client.spec.ts) — A test under the owning area exercises or imports `startedAt`.
- [`packages/client/ui-conversation/tests/views-type-chain.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/views-type-chain.client.spec.tsx) — A test under the owning area exercises or imports `ConversationRoot`.

## How to read the implementation

1. Start with [`scripts/run-gates.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/context`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `startedAt`, `ConversationNodeAssembler`, `views`, `ConversationRoot`, `Session.views`, `focus-within`, `data-conversation-composer-overlay`, `Trajectory inspection ledger`, `feature`, `boundary`, `compatibility`, `discovery routing`, `evidence`, `human control`
- Regex: `(?i)(startedAt|ConversationNodeAssembler|views|ConversationRoot|Session\.views|focus\-within|data\-conversation\-composer\-overlay|Trajectory[- ]inspection[- ]ledger)`

```bash
rg -n --pcre2 "(?i)(startedAt|ConversationNodeAssembler|views|ConversationRoot|Session\\.views|focus\\-within|data\\-conversation\\-composer\\-overlay|Trajectory[- ]inspection[- ]ledger)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0127. Trajectory assembly from registered Conversation Contexts](0127-trajectory-assembly-from-registered-conversation-contexts.md): The source note links to this decision directly.
- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx`.
- **`shares-code-with`** — [0404. Portable pull-request CI recovery boundary](0404-portable-pull-request-ci-recovery-boundary.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx`.
- **`shares-code-with`** — [0297. Workspace Sidebar Order and Folding](0297-workspace-sidebar-order-and-folding.md): Shares source implementation: `apps/web/tests/composer-tab-geometry.e2e.ts`, `packages/client/connection/src/client/fixture.ts`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0239. Goal command input projection](0239-goal-command-input-projection.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0288. Web session-log export as a host-streamed ZIP download](0288-web-session-log-export-as-a-host-streamed-zip-download.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0600. Web message IconActions and clocks](0600-web-message-iconactions-and-clocks.md): Shares source implementation: `apps/web/tests/message-actions.e2e.ts`, `packages/client/connection/src/client/fixture.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0194-trajectory-inspection-ledger.md`.
