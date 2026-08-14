---
id: "dsh-note-0621"
title: "TUI step timing trails the step's last message"
status: "archived"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/bug-fix/2026-07-27-tui-step-timing-trails-tool-cards.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/bug-fix"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/security"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
aliases:
  - "timing"
  - "streaming"
  - "Model wait … · Completed …"
  - "StepTimingComponent"
  - "AssistantMessageComponent"
  - "StreamingAssistantComponent"
  - "tool/call"
  - "tool/result"
  - "trailStreamingTiming"
  - "step/end"
  - "removeStreaming"
  - "untrusted-controls"
  - "cordis-tools-pending"
  - "advanced-cards-*"
search_regex: "(?i)(timing|streaming|Model[- ]wait[- ]…[- ]·[- ]Completed[- ]…|StepTimingComponent|AssistantMessageComponent|StreamingAssistantComponent|tool/call|tool/result)"
---

# 0621. TUI step timing trails the step's last message — implementation context

## Open this when

The per-step timing summary (Model wait … · Completed …) was a child of the assistant message component, so it rendered directly under the assistant text. When a step drove tool calls, the tool cards were appended to the chat after the assistant message, leaving the timing line stranded above them --- one message before the step's actual last output. The summary is meant to close a step, so on any tool-calling step it appeared in the wrong place.

## Source decision

The timing summary is its own StepTimingComponent, no longer a child of AssistantMessageComponent. StreamingAssistantComponent owns one and exposes it as timing, but the renderer attaches it to the chat as a sibling that follows the assistant message. Whenever a tool/call or tool/result of the open step appends a card, trailStreamingTiming() moves the footer back to the tail of the chat, so it always trails the step's last message. On step/end the footer is completed in place --- already at the tail --- and stays pinned while the next step's output follows.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/bug-fix/2026-07-27-tui-step-timing-trails-tool-cards.md](../02-notes/archived/bug-fix/2026-07-27-tui-step-timing-trails-tool-cards.md)
- Pinned source: [.agents/notes/archived/bug-fix/2026-07-27-tui-step-timing-trails-tool-cards.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/bug-fix/2026-07-27-tui-step-timing-trails-tool-cards.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-trajectory/src/client/layout.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts) | runtime implementation | Defines `streaming`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/turn-metrics.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/turn-metrics.ts) | runtime implementation | Defines `timing`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx) | runtime implementation | Defines `timing`, a construct named by the note. | `symbol-definition` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `tool/call` named by the note. Contains the exact code literal `tool/result` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) | package contract and examples | Contains the exact code literal `tool/call` named by the note. Contains the exact code literal `tool/result` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.zh.md) | package contract and examples | Contains the exact code literal `tool/call` named by the note. Contains the exact code literal `tool/result` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `tool/call` named by the note. Contains the exact code literal `tool/result` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `tool/call` named by the note. Contains the exact code literal `tool/result` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Contains the exact code literal `tool/call` named by the note. Contains the exact code literal `tool/result` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.zh.md) | package contract and examples | Contains the exact code literal `step/end` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-tool-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-tool-catalog.ts) | repository automation | Contains the exact code literal `tool/call` named by the note. Contains the exact code literal `tool/result` named by the note. | `exact-code-occurrence` |
| [`docs/persistence-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/persistence-catalog.md) | package contract and examples | Contains the exact code literal `tool/call` named by the note. Contains the exact code literal `tool/result` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `timing` | `const` | [`packages/client/ui-conversation/src/client/chat/turn-metrics.ts:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/turn-metrics.ts#L41) | `const timing = node.timing` |
| `timing` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx:126`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx#L126) | `const timing = [duration, segments].filter(value => value !== null).join(' · ')` |
| `streaming` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:679`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L679) | `const streaming = opts?.streaming === true` |

### Tests and executable evidence

- [`examples/acp-agent/tests/acp.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/acp.snapshot.ts) — A test under the owning area exercises or imports `code-mode`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `code-mode`.
- [`packages/plan/plan-mode/tests/plan-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/tests/plan-mode.spec.ts) — A test under the owning area exercises or imports `code-mode`.
- [`packages/core/agent-loop/tests/tool-calls.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/tool-calls.spec.ts) — A test under the owning area exercises or imports `code-mode`.

## How to read the implementation

1. Start with [`packages/client/ui-trajectory/src/client/layout.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
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

- Tags: `class/bug-fix`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `domain/agent-loop`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/security`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`
- Aliases: `timing`, `streaming`, `Model wait … · Completed …`, `StepTimingComponent`, `AssistantMessageComponent`, `StreamingAssistantComponent`, `tool/call`, `tool/result`, `trailStreamingTiming`, `step/end`, `removeStreaming`, `untrusted-controls`, `cordis-tools-pending`, `advanced-cards-*`
- Regex: `(?i)(timing|streaming|Model[- ]wait[- ]…[- ]·[- ]Completed[- ]…|StepTimingComponent|AssistantMessageComponent|StreamingAssistantComponent|tool/call|tool/result)`

```bash
rg -n --pcre2 "(?i)(timing|streaming|Model[- ]wait[- ]\u2026[- ]\u00b7[- ]Completed[- ]\u2026|StepTimingComponent|AssistantMessageComponent|StreamingAssistantComponent|tool/call|tool/result)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0537. Truncate interrupted final turns on load](0537-truncate-interrupted-final-turns-on-load.md): Shares source implementation: `docs/architecture.md`, `docs/tool-catalog.md`.
- **`shares-code-with`** — [0315. Atomic Web image admission](0315-atomic-web-image-admission.md): Shares source implementation: `docs/architecture.md`, `docs/tool-catalog.md`.
- **`shares-code-with`** — [0582. The running status line shows the turn phase and elapsed time](0582-the-running-status-line-shows-the-turn-phase-and-elapsed-time.md): Shares source implementation: `docs/architecture.md`, `docs/tool-catalog.md`.
- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx`, `packages/client/ui-trajectory/src/client/layout.ts`.
- **`shares-code-with`** — [0219. Steer a queued Web message into the active turn](0219-steer-a-queued-web-message-into-the-active-turn.md): Shares source implementation: `examples/acp-agent/tests/acp.snapshot.ts`, `packages/client/ui-trajectory/src/client/layout.ts`.
- **`shares-code-with`** — [0194. Trajectory inspection ledger](0194-trajectory-inspection-ledger.md): Shares source implementation: `packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx`, `packages/client/ui-trajectory/src/client/layout.ts`.
- **`shares-code-with`** — [0599. TUI hidden mode folds a turn's assistant steps into one message](0599-tui-hidden-mode-folds-a-turn-s-assistant-steps-into-one-message.md): Shares source implementation: `packages/core/agent-loop/tests/tool-calls.spec.ts`, `packages/core/tools/tests/code-mode.spec.ts`.
- **`shares-code-with`** — [0605. Web composer stats detail and input-zone polish](0605-web-composer-stats-detail-and-input-zone-polish.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/turn-metrics.ts`, `packages/client/ui-trajectory/src/client/TrajectoryTimeline.tsx`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0621-tui-step-timing-trails-the-step-s-last-message.md`.
