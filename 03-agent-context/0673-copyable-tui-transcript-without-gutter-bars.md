---
id: "dsh-note-0673"
title: "Copyable TUI transcript without gutter bars"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-07-27-copyable-transcript-no-gutter-bar.md"
implementation_evidence: "lead-only"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/recovery"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
aliases:
  - "UserMessageComponent"
  - "messageHeader"
  - "paddingX = 1"
  - "GutterBox"
  - "/"
  - "*.expected.txt"
  - "Copyable TUI transcript without gutter bars"
  - "simplification"
  - "evidence"
  - "human control"
  - "recovery"
  - "llm"
  - "session state"
  - "shell terminal"
search_regex: "(?i)(UserMessageComponent|messageHeader|paddingX[- ]=[- ]1|GutterBox|\\*\\.expected\\.txt|Copyable[- ]TUI[- ]transcript[- ]without[- ]gutter[- ]bars|simplification|evidence)"
---

# 0673. Copyable TUI transcript without gutter bars — implementation context

## Open this when

The TUI grouped user prompts and tool cards behind a colored left-gutter bar (▌ ) prepended to every body line, and indented assistant and system blocks by one column. Both are per-line prefixes: a terminal mouse drag-select over the scrollback captures the leading ▌ or the leading space on each line, so copy-paste of a message, a tool's output, or a code block pulls in decoration the user must strip by hand. The bar was the transcript's only per-message separator, so it could not simply be dropped without another way to tell messages apart.

## Source decision

The scrollback carries no per-line prefix. Messages are separated only by a bold, underlined role header in the role color and blank-line spacing, both of which the terminal already inserts around each block. The underline gives each role a distinct visual band without a background fill, so it reads on any terminal theme and never enters the clipboard: User and steering prompts (UserMessageComponent) are a plain Container: a bold, underlined accent You / Steering header line (via the shared messageHeader helper), then the prompt body at column 0.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-07-27-copyable-transcript-no-gutter-bar.md](../02-notes/archived/simplification/2026-07-27-copyable-transcript-no-gutter-bar.md)
- Pinned source: [.agents/notes/archived/simplification/2026-07-27-copyable-transcript-no-gutter-bar.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-07-27-copyable-transcript-no-gutter-bar.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

No implementation path could be confirmed from the pinned tree. Use the search handles below; do not invent a source location.

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `You`.
- [`python/sdk/tests/test_bundled_runtime.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_bundled_runtime.py) — A test under the owning area exercises or imports `You`.
- [`apps/web/tests/minimal-preset.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/minimal-preset.snapshot.ts) — A test under the owning area exercises or imports `You`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `You`. A test under the owning area exercises or imports `Steering`.
- [`packages/preset/persona/tests/persona.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/persona/tests/persona.spec.ts) — A test under the owning area exercises or imports `You`.
- [`packages/core/agent-loop/tests/request-cache.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/request-cache.e2e.ts) — A test under the owning area exercises or imports `You`.
- [`packages/core/agent-loop/tests/scope-lifecycle.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/scope-lifecycle.spec.ts) — A test under the owning area exercises or imports `You`.
- [`packages/client/runtime/tests/conversation.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/conversation.client.spec.ts) — A test under the owning area exercises or imports `Assistant`.

## How to read the implementation

1. Start from the source note and run the regex below across the pinned repository.
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

- Tags: `class/simplification`, `concern/evidence`, `concern/human-control`, `concern/recovery`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/event-sourcing`
- Aliases: `UserMessageComponent`, `messageHeader`, `paddingX = 1`, `GutterBox`, `/`, `*.expected.txt`, `Copyable TUI transcript without gutter bars`, `simplification`, `evidence`, `human control`, `recovery`, `llm`, `session state`, `shell terminal`
- Regex: `(?i)(UserMessageComponent|messageHeader|paddingX[- ]=[- ]1|GutterBox|\*\.expected\.txt|Copyable[- ]TUI[- ]transcript[- ]without[- ]gutter[- ]bars|simplification|evidence)`

```bash
rg -n --pcre2 "(?i)(UserMessageComponent|messageHeader|paddingX[- ]=[- ]1|GutterBox|\\*\\.expected\\.txt|Copyable[- ]TUI[- ]transcript[- ]without[- ]gutter[- ]bars|simplification|evidence)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0592. Assistant timing line renders after the message body](0592-assistant-timing-line-renders-after-the-message-body.md): Shares source implementation: `apps/cli/tests/web-agent-presets.e2e.ts`, `apps/web/tests/minimal-preset.snapshot.ts`.
- **`shares-code-with`** — [0604. Versioned TUI first-run welcome](0604-versioned-tui-first-run-welcome.md): Shares source implementation: `apps/cli/tests/web-agent-presets.e2e.ts`.
- **`shares-code-with`** — [0219. Steer a queued Web message into the active turn](0219-steer-a-queued-web-message-into-the-active-turn.md): Shares source implementation: `apps/cli/tests/web-agent-presets.e2e.ts`.
- **`shares-code-with`** — [0370. The chat flow surfaces a max-tokens turn end](0370-the-chat-flow-surfaces-a-max-tokens-turn-end.md): Shares source implementation: `packages/core/agent-loop/tests/loop.spec.ts`.
- **`shares-code-with`** — [0599. TUI hidden mode folds a turn's assistant steps into one message](0599-tui-hidden-mode-folds-a-turn-s-assistant-steps-into-one-message.md): Shares source implementation: `packages/core/agent-loop/tests/loop.spec.ts`.
- **`shares-code-with`** — [0562. The session prefix --- request-only messages in front of the derived history](0562-the-session-prefix-request-only-messages-in-front-of-the-derived-history.md): Shares source implementation: `packages/core/agent-loop/tests/request-cache.e2e.ts`.
- **`shares-code-with`** — [0486. Remove the steering interjection caption](0486-remove-the-steering-interjection-caption.md): Shares source implementation: `packages/core/agent-loop/tests/loop.spec.ts`.
- **`shares-code-with`** — [0429. Browser GIFs preserve one evidence chain](0429-browser-gifs-preserve-one-evidence-chain.md): Shares source implementation: `apps/cli/tests/web-agent-presets.e2e.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0673-copyable-tui-transcript-without-gutter-bars.md`.
