---
id: "dsh-note-0584"
title: "Trajectory step cell and turn list chrome"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-23-trajectory-step-cell.md"
implementation_evidence: "high"
target_anchor: "append-only event and session store"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "lifecycle/archived"
  - "mechanism/streaming"
aliases:
  - "callTime"
  - "ConversationNode"
  - "TrajectoryCell"
  - "time"
  - "selected"
  - "TrajectoryTurn"
  - "usage"
  - "partial"
  - "runningCalls"
  - "deriveTrajectoryLayout"
  - "callId"
  - "@deepseek-ai/dsh-client-ui-trajectory"
  - "tool-call"
  - "tool-result"
search_regex: "(?i)(callTime|ConversationNode|TrajectoryCell|time|selected|TrajectoryTurn|usage|partial)"
---

# 0584. Trajectory step cell and turn list chrome — implementation context

## Open this when

The trajectory tab needs a reusable step row and turn-list chrome that can show expanded assistant blocks, own-duration times, Message token columns, and in-flight work. Without folding session event times into conversation nodes and expanding blocks into cells, the UI cannot match the product chrome.

## Source decision

@deepseek-ai/dsh-client-ui-trajectory owns the presentational trajectory list chrome: TrajectoryCell --- 38px step row with kinds User / Message / Tool (no Think, Call, or Result rows). Reasoning blocks are skipped (no block-level clock). Each tool-call + paired tool-result folds into one Tool row (name · truncated args) whose Time is result.time - callTime when both are known. Message rows carry Input/Output/Think token columns from assistant.usage. Own-duration Time uses +Ns / +N.1s, or --- when absent.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-23-trajectory-step-cell.md](../02-notes/archived/feature/2026-07-23-trajectory-step-cell.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-23-trajectory-step-cell.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-23-trajectory-step-cell.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-trajectory/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/client/ui-trajectory`. | `named-file, named-package-member` |
| [`packages/client/ui-trajectory/src/client/layout.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts) | runtime implementation | The source note names this file directly. Core file in the package named by the note: `packages/client/ui-trajectory`. | `named-file, named-package-member, symbol-definition` |
| [`packages/client/runtime/src/client/sessions/conversation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts) | runtime implementation | The source note names this file directly. Defines `ConversationNode`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/client/ui-trajectory/src/client/TrajectoryCell.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryCell.tsx) | runtime implementation | The source note names this file directly. Defines `TrajectoryCell`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/client/ui-trajectory/src/client/TrajectoryTurn.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTurn.tsx) | runtime implementation | The source note names this file directly. Defines `TrajectoryTurn`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/client/ui-trajectory/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-trajectory`. | `named-package-member` |
| [`packages/client/ui-trajectory/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-trajectory`. | `named-package-member` |
| [`packages/client/ui-trajectory/src/client/TrajectoryView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryView.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-trajectory`. Defines `usage`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-trajectory/src/client/trajectory-snapshot-builder.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/trajectory-snapshot-builder.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-trajectory`. Defines `partial`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-trajectory/src/client/trajectory-assistant-definition.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/trajectory-assistant-definition.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-trajectory`. Defines `partial`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-trajectory/tests`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/tests) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx) | runtime implementation | Defines `selected`, a construct named by the note. Defines `time`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `callTime` | `const` | [`packages/client/connection/src/client/fixture.ts:596`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L596) | `const callTime = events[callIndex]?.time as number` |
| `ConversationNode` | `type` | [`packages/client/runtime/src/client/sessions/conversation.ts:281`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts#L281) | `export type ConversationNode =` |
| `TrajectoryCell` | `function` | [`packages/client/ui-trajectory/src/client/TrajectoryCell.tsx:43`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryCell.tsx#L43) | `export function TrajectoryCell({` |
| `time` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx:270`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx#L270) | `const time = \`${two(date.getHours())}:${two(date.getMinutes())}:${two(date.getSeconds())}.${three(date.getMilliseconds())}\`` |
| `selected` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx:1748`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx#L1748) | `const selected = selectedTemplate === undefined` |
| `selected` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx:526`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx#L526) | `const selected = orderedRange(drag.anchorTime, pointTime)` |
| `TrajectoryTurn` | `function` | [`packages/client/ui-trajectory/src/client/TrajectoryTurn.tsx:19`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTurn.tsx#L19) | `export function TrajectoryTurn({ turn, children }: TrajectoryTurnProps) {` |
| `usage` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryView.tsx:85`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryView.tsx#L85) | `const usage = value as UsageLike \| undefined` |
| `partial` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryView.tsx:150`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryView.tsx#L150) | `const partial = inspection.partial` |
| `runningCalls` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryView.tsx:151`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryView.tsx#L151) | `const runningCalls = inspection.runningCalls` |
| `usage` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryView.tsx:189`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryView.tsx#L189) | `const usage = requestUsage(entry.request?.usage ?? entry.node?.usage)` |
| `deriveTrajectoryLayout` | `function` | [`packages/client/ui-trajectory/src/client/layout.ts:138`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L138) | `export function deriveTrajectoryLayout(input: TrajectoryLayoutInput): readonly TrajectoryTurnModel[] {` |
| `usage` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:678`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L678) | `const usage = node.usage as UsageLike \| undefined` |
| `partial` | `const` | [`packages/client/ui-trajectory/src/client/trajectory-assistant-definition.ts:347`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/trajectory-assistant-definition.ts#L347) | `const partial: PartialAssistant \| null = node === undefined && boundary === undefined && state.sawChunk` |
| `partial` | `let` | [`packages/client/ui-trajectory/src/client/trajectory-snapshot-builder.ts:191`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/trajectory-snapshot-builder.ts#L191) | `let partial: TrajectorySnapshot['partial'] = null` |
| `runningCalls` | `const` | [`packages/client/ui-trajectory/src/client/trajectory-snapshot-builder.ts:192`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/trajectory-snapshot-builder.ts#L192) | `const runningCalls: TrajectorySnapshot['runningCalls'][number][] = []` |

### Tests and executable evidence

- [`packages/client/runtime/tests/session.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/session.client.spec.ts) — A test under the owning area exercises or imports `ConversationNode`.
- [`packages/client/ui-trajectory/tests/cell.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/tests/cell.client.spec.tsx) — A test under the owning area exercises or imports `TrajectoryCell`.
- [`packages/client/ui-trajectory/tests/views.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/tests/views.client.spec.tsx) — A test under the owning area exercises or imports `runningCalls`. A test under the owning area exercises or imports `callTime`.
- [`packages/client/ui-trajectory/tests/layout.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/tests/layout.client.spec.tsx) — A test under the owning area exercises or imports `TrajectoryTurn`. A test under the owning area exercises or imports `deriveTrajectoryLayout`.

## How to read the implementation

1. Start with [`packages/client/ui-trajectory/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/README.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** append-only event and session store.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
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

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/session-state`, `domain/shell-terminal`, `lifecycle/archived`, `mechanism/streaming`
- Aliases: `callTime`, `ConversationNode`, `TrajectoryCell`, `time`, `selected`, `TrajectoryTurn`, `usage`, `partial`, `runningCalls`, `deriveTrajectoryLayout`, `callId`, `@deepseek-ai/dsh-client-ui-trajectory`, `tool-call`, `tool-result`
- Regex: `(?i)(callTime|ConversationNode|TrajectoryCell|time|selected|TrajectoryTurn|usage|partial)`

```bash
rg -n --pcre2 "(?i)(callTime|ConversationNode|TrajectoryCell|time|selected|TrajectoryTurn|usage|partial)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0292. Web surface for message feedback](0292-web-surface-for-message-feedback.md): Shares source implementation: `packages/client/ui-trajectory/src/client/layout.ts`, `packages/client/ui-trajectory/src/index.ts`.
- **`shares-code-with`** — [0219. Steer a queued Web message into the active turn](0219-steer-a-queued-web-message-into-the-active-turn.md): Shares source implementation: `packages/client/ui-trajectory/src/client/layout.ts`.
- **`shares-code-with`** — [0194. Trajectory inspection ledger](0194-trajectory-inspection-ledger.md): Shares source implementation: `packages/client/ui-trajectory/src/client/layout.ts`.
- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `packages/client/ui-trajectory/src/client/layout.ts`.
- **`shares-code-with`** — [0504. Frame-coalesced reasoning-chunk publication and browser stress validation](0504-frame-coalesced-reasoning-chunk-publication-and-browser-stress-validatio.md): Shares source implementation: `packages/client/runtime/src/client/sessions/conversation.ts`.
- **`shares-code-with`** — [0600. Web message IconActions and clocks](0600-web-message-iconactions-and-clocks.md): Shares source implementation: `packages/client/ui-trajectory/src/client/layout.ts`.
- **`shares-code-with`** — [0405. Calibrated translation prompt v4 contract](0405-calibrated-translation-prompt-v4-contract.md): Shares source implementation: `packages/client/ui-trajectory/src/client/trajectory-assistant-definition.ts`.
- **`shares-code-with`** — [0441. Python public publication workflow](0441-python-public-publication-workflow.md): Shares source implementation: `packages/client/ui-trajectory/src/client/layout.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0584-trajectory-step-cell-and-turn-list-chrome.md`.
