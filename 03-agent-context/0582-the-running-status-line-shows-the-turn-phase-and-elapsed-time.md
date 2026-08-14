---
id: "dsh-note-0582"
title: "The running status line shows the turn phase and elapsed time"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-21-tui-verbose-status-line.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "setStatus"
  - "waiting"
  - "executing"
  - "Loader"
  - "--- Enter sends steering, Esc cancels"
  - "step/start"
  - "assistant/chunk"
  - "tool/call"
  - "<phase> <phase-elapsed> · total <step-elapsed>"
  - "step/end"
  - "RunningStatus"
  - "setInterval"
  - "clearStatus"
  - "Thinking 4s · total 8s --- Enter sends steering, Esc cancels"
search_regex: "(?i)(setStatus|waiting|executing|Loader|\\-\\-\\-[- ]Enter[- ]sends[- ]steering,[- ]Esc[- ]cancels|step/start|assistant/chunk|tool/call)"
---

# 0582. The running status line shows the turn phase and elapsed time — implementation context

## Open this when

While a turn ran, the full-screen TUI showed a single static "Working" spinner. It conveyed neither how long the current step had taken nor what the agent was doing --- waiting on the model, thinking, streaming a response, or running tools --- so a slow or stalled turn was indistinguishable from a fast one.

## Source decision

While a turn runs, the status line above the editor shows a derived phase label with elapsed time, keeping the trailing --- Enter sends steering, Esc cancels hint. The four phases and their labels are waiting → "Waiting for the first token", thinking → "Thinking", responding → "Responding", and executing → "Executing tools". The phase is presentation state the TUI derives from live session events, not a session event or agent status of its own.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-21-tui-verbose-status-line.md](../02-notes/archived/feature/2026-07-21-tui-verbose-status-line.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-21-tui-verbose-status-line.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-21-tui-verbose-status-line.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/loader`. Defines `Loader`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`vendor/loader`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/vendor/loader) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/cordis/src/fiber.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts) | runtime implementation | Defines `executing`, a construct named by the note. | `symbol-definition` |
| [`.github/issue-management/policy.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/issue-management/policy.mjs) | repository automation | Defines `setStatus`, a construct named by the note. | `symbol-definition` |
| [`packages/lsp/lsp-stdio/src/connection.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/connection.ts) | runtime implementation | Defines `waiting`, a construct named by the note. | `symbol-definition` |
| [`vendor/loader/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/README.md) | package contract and examples | Core file in the package named by the note: `vendor/loader`. | `named-package-member` |
| [`vendor/loader/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/package.json) | composition and configuration | Core file in the package named by the note: `vendor/loader`. | `named-package-member` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `step/start` named by the note. Contains the exact code literal `assistant/chunk` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) | package contract and examples | Contains the exact code literal `tool/call` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) | package contract and examples | Contains the exact code literal `assistant/chunk` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `step/start` named by the note. Contains the exact code literal `assistant/chunk` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `step/start` named by the note. Contains the exact code literal `assistant/chunk` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `setStatus` | `function` | [`.github/issue-management/policy.mjs:554`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/issue-management/policy.mjs#L554) | `async function setStatus(number, status) {` |
| `waiting` | `const` | [`packages/lsp/lsp-stdio/src/connection.ts:319`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/connection.ts#L319) | `const waiting = [...this.pending.values()]` |
| `executing` | `let` | [`vendor/cordis/src/fiber.ts:459`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts#L459) | `let executing = true` |
| `Loader` | `namespace` | [`vendor/loader/src/index.ts:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts#L46) | `export namespace Loader {` |
| `Loader` | `class` | [`vendor/loader/src/index.ts:65`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts#L65) | `export class Loader extends EntryTree {` |

### Tests and executable evidence

- [`scripts/session-fixture-layout.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/session-fixture-layout.spec.ts) — Contains the exact code literal `assistant/chunk` named by the note.
- Source verification intent: packages/ui/tui/tests/tui.spec.ts pins each phase label against its triggering event (step/start, reasoning and text deltas and block-starts, tool/call), that a new step reopens the wait window, that the elapsed time advances on the controller's own timer past one second, that a step beyond a minute renders 1m…, that a mid-turn color-scheme change preserves the phase and elapsed time, and that a live event arriving before the turn runs moves no status. Verified live in tmux.

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

- Tags: `class/feature`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `domain/agent-loop`, `domain/build-release`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `setStatus`, `waiting`, `executing`, `Loader`, `--- Enter sends steering, Esc cancels`, `step/start`, `assistant/chunk`, `tool/call`, `<phase> <phase-elapsed> · total <step-elapsed>`, `step/end`, `RunningStatus`, `setInterval`, `clearStatus`, `Thinking 4s · total 8s --- Enter sends steering, Esc cancels`
- Regex: `(?i)(setStatus|waiting|executing|Loader|\-\-\-[- ]Enter[- ]sends[- ]steering,[- ]Esc[- ]cancels|step/start|assistant/chunk|tool/call)`

```bash
rg -n --pcre2 "(?i)(setStatus|waiting|executing|Loader|\\-\\-\\-[- ]Enter[- ]sends[- ]steering,[- ]Esc[- ]cancels|step/start|assistant/chunk|tool/call)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0567. Dedicated full-screen TUI front door](0567-dedicated-full-screen-tui-front-door.md): The source note links to this decision directly.
- **`source-link`** — [0575. The banner returns, borderless](0575-the-banner-returns-borderless.md): The source note links to this decision directly.
- **`shares-code-with`** — [0581. TUI status line badges queued steering messages](0581-tui-status-line-badges-queued-steering-messages.md): Shares source implementation: `.github/issue-management/policy.mjs`, `vendor/loader`.
- **`shares-code-with`** — [0537. Truncate interrupted final turns on load](0537-truncate-interrupted-final-turns-on-load.md): Shares source implementation: `docs/architecture.md`, `docs/config-catalog.md`.
- **`shares-code-with`** — [0466. Remove synthetic turns for log-only events](0466-remove-synthetic-turns-for-log-only-events.md): Shares source implementation: `docs/architecture.md`, `docs/config-catalog.md`.
- **`shares-code-with`** — [0149. The self-referential cordis toolset](0149-the-self-referential-cordis-toolset.md): Shares source implementation: `docs/config-catalog.md`, `docs/tool-catalog.md`.
- **`shares-code-with`** — [0621. TUI step timing trails the step's last message](0621-tui-step-timing-trails-the-step-s-last-message.md): Shares source implementation: `docs/architecture.md`, `docs/tool-catalog.md`.
- **`shares-code-with`** — [0120. Product subagent providers live in the shared profile host](0120-product-subagent-providers-live-in-the-shared-profile-host.md): Shares source implementation: `docs/config-catalog.md`, `docs/tool-catalog.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0582-the-running-status-line-shows-the-turn-phase-and-elapsed-time.md`.
