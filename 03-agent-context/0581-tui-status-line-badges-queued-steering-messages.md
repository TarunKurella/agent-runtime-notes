---
id: "dsh-note-0581"
title: "TUI status line badges queued steering messages"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-21-tui-steering-queue-badge.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
aliases:
  - "setStatus"
  - "pendingSteering"
  - "running"
  - "Loader"
  - "agent.steer"
  - "Enter sends steering, Esc cancels"
  - "agent/queued"
  - "steering/message"
  - "formatTurnStatus"
  - "createTuiChat"
  - "info.steering"
  - "-1"
  - "setMessage"
  - "agent/status"
search_regex: "(?i)(setStatus|pendingSteering|running|Loader|agent\\.steer|Enter[- ]sends[- ]steering,[- ]Esc[- ]cancels|agent/queued|steering/message)"
---

# 0581. TUI status line badges queued steering messages — implementation context

## Open this when

While a turn runs, an editor submission calls agent.steer() and joins the steering queue behind the running turn (front-door Agent Note). The running status line ended only with the Enter sends steering, Esc cancels hint, so pressing Enter gave no feedback that the message landed or how many were waiting to reach the model. A user steering several times could not tell the queue from a dropped keystroke.

## Source decision

The agent's inbox is the authoritative steering queue but is not observable from the TUI, so the badge is a live count reconstructed from the public agent/queued and steering/message events rather than a projection of the queue itself. The running status line composes through formatTurnStatus, which inserts a ${queued} queued · badge before the Enter sends steering, Esc cancels hint when queued > 0 and shows the plain hint at zero; the phase label and elapsed timing before it are the verbose status line's.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-21-tui-steering-queue-badge.md](../02-notes/archived/feature/2026-07-21-tui-steering-queue-badge.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-21-tui-steering-queue-badge.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-21-tui-steering-queue-badge.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/loader`. Defines `Loader`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`vendor/loader`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/vendor/loader) | package or module directory | The note names this package or capability. | `named-package` |
| [`.github/issue-management/policy.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/issue-management/policy.mjs) | repository automation | Defines `setStatus`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/bash-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts) | package entry point | Defines `running`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/ChatView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx) | runtime implementation | Defines `pendingSteering`, a construct named by the note. | `symbol-definition` |
| [`vendor/loader/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/README.md) | package contract and examples | Core file in the package named by the note: `vendor/loader`. | `named-package-member` |
| [`vendor/loader/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/package.json) | composition and configuration | Core file in the package named by the note: `vendor/loader`. | `named-package-member` |
| [`docs/subsystems/core.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/core.md) | package contract and examples | Contains the exact code literal `steering/message` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `agent/status` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Contains the exact code literal `agent/status` named by the note. | `exact-code-occurrence` |
| [`docs/subsystems/core.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/core.zh.md) | package contract and examples | Contains the exact code literal `steering/message` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.zh.md) | package contract and examples | Contains the exact code literal `agent/status` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `setStatus` | `function` | [`.github/issue-management/policy.mjs:554`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/issue-management/policy.mjs#L554) | `async function setStatus(number, status) {` |
| `pendingSteering` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:163`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L163) | `const pendingSteering = useMemo(` |
| `running` | `const` | [`packages/shell/bash-local/src/index.ts:257`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts#L257) | `const running = this.ctx.subprocess.spawn(this.spawnSpec(spec, argv, this.config.maxOutputBytes, spec.signal))` |
| `Loader` | `namespace` | [`vendor/loader/src/index.ts:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts#L46) | `export namespace Loader {` |
| `Loader` | `class` | [`vendor/loader/src/index.ts:65`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts#L65) | `export class Loader extends EntryTree {` |

### Tests and executable evidence

- [`packages/core/agent-loop/tests/resume.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/resume.spec.ts) — Contains the exact code literal `steering/message` named by the note.
- [`packages/session/session-persistence/tests/coordinator-contract.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/tests/coordinator-contract.ts) — Contains the exact code literal `steering/message` named by the note.
- Source verification intent: packages/ui/tui/tests/tui.spec.ts drives the running status frame through the real createTuiChat: the plain hint at zero, a foreign-agent queue ignored, the increment to 2 queued, a non-steering queue left untouched, the decrement as each message drains, the clamp on a drain past zero, and the reset when the turn ends. Verified live in tmux --- the badge showed 3 queued after three agent.steer() calls, then 1 queued as two drained.

## How to read the implementation

1. Start with [`vendor/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `domain/build-release`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/projection`
- Aliases: `setStatus`, `pendingSteering`, `running`, `Loader`, `agent.steer`, `Enter sends steering, Esc cancels`, `agent/queued`, `steering/message`, `formatTurnStatus`, `createTuiChat`, `info.steering`, `-1`, `setMessage`, `agent/status`
- Regex: `(?i)(setStatus|pendingSteering|running|Loader|agent\.steer|Enter[- ]sends[- ]steering,[- ]Esc[- ]cancels|agent/queued|steering/message)`

```bash
rg -n --pcre2 "(?i)(setStatus|pendingSteering|running|Loader|agent\\.steer|Enter[- ]sends[- ]steering,[- ]Esc[- ]cancels|agent/queued|steering/message)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0567. Dedicated full-screen TUI front door](0567-dedicated-full-screen-tui-front-door.md): The source note links to this decision directly.
- **`source-link`** — [0582. The running status line shows the turn phase and elapsed time](0582-the-running-status-line-shows-the-turn-phase-and-elapsed-time.md): The source note links to this decision directly.
- **`shares-code-with`** — [0601. Live standalone compaction progress in the terminal](0601-live-standalone-compaction-progress-in-the-terminal.md): Shares source implementation: `docs/agent-lifecycle.md`, `scripts/gen-doc-graphs.ts`.
- **`shares-code-with`** — [0353. Latch wake-ups that land in the cancel-convergence window](0353-latch-wake-ups-that-land-in-the-cancel-convergence-window.md): Shares source implementation: `packages/shell/bash-local/src/index.ts`, `scripts/gen-doc-graphs.ts`.
- **`shares-code-with`** — [0358. The minimal preset owns the complete RL agent composition](0358-the-minimal-preset-owns-the-complete-rl-agent-composition.md): Shares source implementation: `docs/agent-lifecycle.md`, `scripts/gen-doc-graphs.ts`.
- **`shares-code-with`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): Shares source implementation: `vendor/loader/src/index.ts`.
- **`shares-code-with`** — [0546. Extract example apps into packages](0546-extract-example-apps-into-packages.md): Shares source implementation: `packages/shell/bash-local/src/index.ts`, `vendor/loader/src/index.ts`.
- **`shares-code-with`** — [0633. Documentation graph index for maintainers and SDK users](0633-documentation-graph-index-for-maintainers-and-sdk-users.md): Shares source implementation: `docs/agent-lifecycle.md`, `scripts/gen-doc-graphs.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0581-tui-status-line-badges-queued-steering-messages.md`.
