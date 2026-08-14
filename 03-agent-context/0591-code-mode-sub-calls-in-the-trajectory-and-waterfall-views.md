---
id: "dsh-note-0591"
title: "Code Mode sub-calls in the trajectory and waterfall views"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-26-code-mode-trajectory-waterfall-spans.md"
implementation_evidence: "medium"
target_anchor: "append-only event and session store"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/lifecycle"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
aliases:
  - "callId"
  - "running"
  - "timing"
  - "durationSeconds"
  - "measured"
  - "unknown"
  - "run_code"
  - "codeDispatches"
  - "deriveSubSpans"
  - "callTime: null"
  - "#N"
  - "Code Mode sub-calls in the trajectory and waterfall views"
  - "feature"
  - "boundary"
search_regex: "(?i)(callId|running|timing|durationSeconds|measured|unknown|run_code|codeDispatches)"
---

# 0591. Code Mode sub-calls in the trajectory and waterfall views — implementation context

## Open this when

Trajectory and waterfall still rendered a run_code turn as one opaque Tool cell / one node-count bar. The chat view got nested sub-rows in the earlier PRs, but the two analytical views --- whose whole purpose is structure and timing --- showed none of the sub-call structure and none of the per-sub-call wall time the dispatch pair now records. Waterfall sub-spans were deliberately deferred until that pair existed: a span without real timing would have been a lie.

## Source decision

Trajectory: subtool cells interleaved after their parent Tool cell. Waterfall: real-time sub-lanes under the owning turn row. Trajectory: the layout fold takes the snapshot's codeDispatches index; after each Tool cell whose callId has dispatches (assistant-block calls, orphan results, and running calls alike), it interleaves one subtool cell per sub-dispatch in start order --- indexes stay sequential across the interleave. A settled sub-call's duration is its start/settle pair (durationSeconds(sub.time, sub.callTime)); a running one shows the em dash, exactly the native in-flight convention.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-26-code-mode-trajectory-waterfall-spans.md](../02-notes/archived/feature/2026-07-26-code-mode-trajectory-waterfall-spans.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-26-code-mode-trajectory-waterfall-spans.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-26-code-mode-trajectory-waterfall-spans.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/clean.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/clean.ts) | repository automation | Defines `unknown`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `callId`, a construct named by the note. Defines `unknown`, a construct named by the note. | `symbol-definition` |
| [`packages/plan/plan-mode/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/index.ts) | package entry point | Defines `unknown`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Defines `callId`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/bash-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts) | package entry point | Defines `running`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/pwsh-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/index.ts) | package entry point | Defines `running`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `callId`, a construct named by the note. | `symbol-definition` |
| [`packages/core/system-prompt/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/index.ts) | package entry point | Defines `unknown`, a construct named by the note. | `symbol-definition` |
| [`packages/subagent/subagent/src/descriptor.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/descriptor.ts) | runtime implementation | Defines `unknown`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/acp-snapshot/src/suite.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts) | runtime implementation | Defines `callId`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `callId`, a construct named by the note. Defines `running`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/layout.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts) | runtime implementation | Defines `durationSeconds`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `callId` | `const` | [`packages/client/connection/src/client/fixture.ts:404`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L404) | `const callId = \`fx-call-${turn}\`` |
| `running` | `const` | [`packages/client/connection/src/client/fixture.ts:1792`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1792) | `const running = summaryOf(id)?.running === true` |
| `running` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L156) | `const running = useSession(s => s.running)` |
| `timing` | `const` | [`packages/client/ui-conversation/src/client/chat/turn-metrics.ts:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/turn-metrics.ts#L41) | `const timing = node.timing` |
| `timing` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx:126`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx#L126) | `const timing = [duration, segments].filter(value => value !== null).join(' · ')` |
| `durationSeconds` | `function` | [`packages/client/ui-trajectory/src/client/layout.ts:656`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L656) | `function durationSeconds(later: number, earlier: number \| null): number \| null {` |
| `measured` | `const` | [`packages/compaction/compaction-basic/src/region.ts:420`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/region.ts#L420) | `const measured = dependencies.meter.measure(session).nodes.slice(current.startIdx, current.endIdx + 1)` |
| `callId` | `const` | [`packages/core/session/src/invariant.ts:137`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts#L137) | `const callId = event.data.message.source.callId` |
| `unknown` | `const` | [`packages/core/system-prompt/src/index.ts:170`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/index.ts#L170) | `const unknown = toolOrder.filter(name => name !== TOOL_ORDER_REST && !knownNames.has(name))` |
| `unknown` | `const` | [`packages/core/tools/src/index.ts:1089`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1089) | `const unknown = [...allow ?? [], ...deny ?? []].filter(name => !known.has(name))` |
| `callId` | `const` | [`packages/core/tools/src/index.ts:1367`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1367) | `const callId = exec.callId` |
| `running` | `const` | [`packages/extensions/ui-cordis/src/client/CordisPanel.tsx:154`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/CordisPanel.tsx#L154) | `const running = all.filter(view => visiblePanelStatus(` |
| `callId` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:764`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L764) | `const callId = message.source.callId` |
| `unknown` | `const` | [`packages/plan/plan-mode/src/index.ts:114`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/index.ts#L114) | `const unknown = Object.keys(config).filter(key => key !== 'section')` |
| `running` | `const` | [`packages/shell/bash-local/src/index.ts:257`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts#L257) | `const running = this.ctx.subprocess.spawn(this.spawnSpec(spec, argv, this.config.maxOutputBytes, spec.signal))` |
| `running` | `const` | [`packages/shell/pwsh-local/src/index.ts:286`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/index.ts#L286) | `const running = this.ctx.subprocess.spawn(this.spawnSpec(spec, this.config.maxOutputBytes, spec.signal, argv))` |

### Tests and executable evidence

- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `run_code`.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `run_code`.
- [`apps/web/tests/code-mode-round.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/code-mode-round.e2e.ts) — A test under the owning area exercises or imports `run_code`.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `run_code`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `run_code`.
- [`packages/core/tools/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/invariant.spec.ts) — A test under the owning area exercises or imports `run_code`.
- [`packages/core/tools/tests/gen-tool-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/gen-tool-catalog.spec.ts) — A test under the owning area exercises or imports `run_code`.
- [`packages/core/agent/tests/verify-export-jsdoc.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/verify-export-jsdoc.spec.ts) — A test under the owning area exercises or imports `Sub`.

## How to read the implementation

1. Start with [`scripts/clean.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/clean.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/lifecycle`, `domain/build-release`, `domain/extensions`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/projection`
- Aliases: `callId`, `running`, `timing`, `durationSeconds`, `measured`, `unknown`, `run_code`, `codeDispatches`, `deriveSubSpans`, `callTime: null`, `#N`, `Code Mode sub-calls in the trajectory and waterfall views`, `feature`, `boundary`
- Regex: `(?i)(callId|running|timing|durationSeconds|measured|unknown|run_code|codeDispatches)`

```bash
rg -n --pcre2 "(?i)(callId|running|timing|durationSeconds|measured|unknown|run_code|codeDispatches)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0616. TUI presents a reason for every turn-end kind](0616-tui-presents-a-reason-for-every-turn-end-kind.md): Shares source implementation: `packages/plan/plan-mode/src/index.ts`, `packages/shell/bash-local/src/index.ts`.
- **`shares-code-with`** — [0353. Latch wake-ups that land in the cancel-convergence window](0353-latch-wake-ups-that-land-in-the-cancel-convergence-window.md): Shares source implementation: `packages/plan/plan-mode/src/index.ts`, `packages/shell/bash-local/src/index.ts`.
- **`shares-code-with`** — [0461. Collapse agent-loop events around the observable state machine](0461-collapse-agent-loop-events-around-the-observable-state-machine.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/shell/bash-local/src/index.ts`.
- **`shares-code-with`** — [0270. Steer the whole Web queue with an empty-draft Cmd/Ctrl+Enter](0270-steer-the-whole-web-queue-with-an-empty-draft-cmd-ctrl-enter.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/shell/bash-local/src/index.ts`.
- **`shares-code-with`** — [0348. `list_agents` uses `ready` for resumable children](0348-list-agents-uses-ready-for-resumable-children.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/shell/bash-local/src/index.ts`.
- **`shares-code-with`** — [0127. Trajectory assembly from registered Conversation Contexts](0127-trajectory-assembly-from-registered-conversation-contexts.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/core/session/src/invariant.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0138. dsh-hooks-claude-code + dsh-hooks-codex --- the Claude Code / Codex hook bridges](0138-dsh-hooks-claude-code-dsh-hooks-codex-the-claude-code-codex-hook-bridges.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/shell/bash-local/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0591-code-mode-sub-calls-in-the-trajectory-and-waterfall-views.md`.
