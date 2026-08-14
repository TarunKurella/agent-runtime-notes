---
id: "dsh-note-0605"
title: "Web composer stats detail and input-zone polish"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-30-web-composer-stats-and-input-polish.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/context"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/tools"
  - "lifecycle/archived"
  - "mechanism/projection"
aliases:
  - "formatTokens"
  - "formatDuration"
  - "StatsLine"
  - "timing"
  - "--dsh-composer-height"
  - "conversation.composer.dock"
  - "ComposerBarOwnerProps.footer"
  - ".root"
  - "completedTime - stepStartTime"
  - "time - callTime"
  - ".composerStack"
  - "linear-gradient"
  - "color-mix"
  - "bg-base"
search_regex: "(?i)(formatTokens|formatDuration|StatsLine|timing|\\-\\-dsh\\-composer\\-height|conversation\\.composer\\.dock|ComposerBarOwnerProps\\.footer|\\.root)"
---

# 0605. Web composer stats detail and input-zone polish — implementation context

## Open this when

The web composer footer showed a single joined stats string (cache/tokens/turns/steps) in its own stack row, visually detached from the input card and missing the design's duration and token-split details. The input zone itself had accumulated per-entry spacing hacks: dock strips carried their own margins, the sticky seat sat on a solid fill that clipped the transcript hard, the back-to-bottom control cleared the composer by a hardcoded offset that broke as the draft grew, and the goal and todo strips disagreed on surface color and column width.

## Source decision

The stats line renders inside the InputBar's width column through a new footer owner prop and expands to the design's grouped detail row; the composer stack owns one 6px rhythm; the seat fades the transcript through a fixed 36px token-bound gradient; the back-to-bottom control follows a live --dsh-composer-height; goal, todo, and queue share one 752px tip-fill column. 'conversation.composer.dock' entries reach the page as the ComposerBarOwnerProps.footer slot, rendered under the card inside the bar's .root, so the stats line and the card share one width constraint.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-30-web-composer-stats-and-input-polish.md](../02-notes/archived/feature/2026-07-30-web-composer-stats-and-input-polish.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-30-web-composer-stats-and-input-polish.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-30-web-composer-stats-and-input-polish.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/context/time-context/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/index.ts) | package entry point | Defines `formatDuration`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-jobs/src/client/JobListAction.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-jobs/src/client/JobListAction.tsx) | runtime implementation | Defines `formatDuration`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/StatsLine.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/StatsLine.tsx) | runtime implementation | Defines `StatsLine`, a construct named by the note. Defines `formatTokens`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/turn-metrics.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/turn-metrics.ts) | runtime implementation | Defines `timing`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx) | runtime implementation | Defines `timing`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-subagent/src/client/SubagentCatalogAction.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-subagent/src/client/SubagentCatalogAction.tsx) | runtime implementation | Defines `formatTokens`, a construct named by the note. Defines `formatDuration`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `formatTokens` | `function` | [`packages/client/ui-conversation/src/client/chat/StatsLine.tsx:84`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/StatsLine.tsx#L84) | `export function formatTokens(n: number): string {` |
| `formatDuration` | `function` | [`packages/client/ui-conversation/src/client/chat/StatsLine.tsx:97`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/StatsLine.tsx#L97) | `export function formatDuration(ms: number): string {` |
| `StatsLine` | `const` | [`packages/client/ui-conversation/src/client/chat/StatsLine.tsx:163`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/StatsLine.tsx#L163) | `export const StatsLine = memo(function StatsLine({ useSession, useProjection, t }: StatsLineProps) {` |
| `timing` | `const` | [`packages/client/ui-conversation/src/client/chat/turn-metrics.ts:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/turn-metrics.ts#L41) | `const timing = node.timing` |
| `formatDuration` | `function` | [`packages/client/ui-jobs/src/client/JobListAction.tsx:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-jobs/src/client/JobListAction.tsx#L62) | `function formatDuration(elapsedMs: number, t: TranslateNS<typeof NS>): string {` |
| `formatTokens` | `function` | [`packages/client/ui-subagent/src/client/SubagentCatalogAction.tsx:64`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-subagent/src/client/SubagentCatalogAction.tsx#L64) | `function formatTokens(value: number): string {` |
| `formatDuration` | `function` | [`packages/client/ui-subagent/src/client/SubagentCatalogAction.tsx:124`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-subagent/src/client/SubagentCatalogAction.tsx#L124) | `function formatDuration(ms: number, t: TranslateNS<typeof NS>): string {` |
| `timing` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx:126`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx#L126) | `const timing = [duration, segments].filter(value => value !== null).join(' · ')` |
| `formatDuration` | `function` | [`packages/context/time-context/src/index.ts:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/index.ts#L41) | `function formatDuration(elapsedMs: number): string {` |

### Tests and executable evidence

- [`apps/web/tests/steering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/steering.e2e.ts) — A test under the owning area exercises or imports `dock`.
- [`scripts/project-doc-site.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.spec.ts) — A test under the owning area exercises or imports `footer`.
- [`apps/web/tests/todo-row.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/todo-row.snapshot.ts) — A test under the owning area exercises or imports `dock`.
- [`apps/web/tests/built-boot.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/built-boot.snapshot.ts) — A test under the owning area exercises or imports `footer`.
- [`apps/web/tests/message-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/message-actions.e2e.ts) — A test under the owning area exercises or imports `footer`.
- [`apps/web/tests/search-card.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/search-card.snapshot.ts) — A test under the owning area exercises or imports `footer`.
- [`apps/web/tests/complex-history.perf.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/complex-history.perf.ts) — A test under the owning area exercises or imports `footer`.
- [`apps/web/tests/navigation-panes.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/navigation-panes.e2e.ts) — A test under the owning area exercises or imports `linear-gradient`.

## How to read the implementation

1. Start with [`packages/context/time-context/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `domain/build-release`, `domain/context`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/tools`, `lifecycle/archived`, `mechanism/projection`
- Aliases: `formatTokens`, `formatDuration`, `StatsLine`, `timing`, `--dsh-composer-height`, `conversation.composer.dock`, `ComposerBarOwnerProps.footer`, `.root`, `completedTime - stepStartTime`, `time - callTime`, `.composerStack`, `linear-gradient`, `color-mix`, `bg-base`
- Regex: `(?i)(formatTokens|formatDuration|StatsLine|timing|\-\-dsh\-composer\-height|conversation\.composer\.dock|ComposerBarOwnerProps\.footer|\.root)`

```bash
rg -n --pcre2 "(?i)(formatTokens|formatDuration|StatsLine|timing|\\-\\-dsh\\-composer\\-height|conversation\\.composer\\.dock|ComposerBarOwnerProps\\.footer|\\.root)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0253. Web turn and window latency/throughput metrics](0253-web-turn-and-window-latency-throughput-metrics.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/StatsLine.tsx`, `packages/client/ui-conversation/src/client/chat/turn-metrics.ts`.
- **`shares-code-with`** — [0566. Optional time-context plugin](0566-optional-time-context-plugin.md): Shares source implementation: `packages/context/time-context/src/index.ts`.
- **`shares-code-with`** — [0257. Durable Session-local reminders](0257-durable-session-local-reminders.md): Shares source implementation: `packages/context/time-context/src/index.ts`.
- **`shares-code-with`** — [0194. Trajectory inspection ledger](0194-trajectory-inspection-ledger.md): Shares source implementation: `packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx`.
- **`shares-code-with`** — [0156. Durable per-step time context](0156-durable-per-step-time-context.md): Shares source implementation: `packages/context/time-context/src/index.ts`.
- **`shares-code-with`** — [0076. Projected token usage and context occupancy](0076-projected-token-usage-and-context-occupancy.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/StatsLine.tsx`.
- **`shares-code-with`** — [0621. TUI step timing trails the step's last message](0621-tui-step-timing-trails-the-step-s-last-message.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/turn-metrics.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx`.
- **`shares-code-with`** — [0404. Portable pull-request CI recovery boundary](0404-portable-pull-request-ci-recovery-boundary.md): Shares source implementation: `apps/web/tests/steering.e2e.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0605-web-composer-stats-detail-and-input-zone-polish.md`.
