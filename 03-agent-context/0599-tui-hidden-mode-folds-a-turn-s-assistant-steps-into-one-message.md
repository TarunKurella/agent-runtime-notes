---
id: "dsh-note-0599"
title: "TUI hidden mode folds a turn's assistant steps into one message"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-29-tui-hidden-mode-assistant-fold.md"
implementation_evidence: "lead-only"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/context"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/streaming"
aliases:
  - "StreamingAssistantComponent"
  - "StepPosition"
  - "setFoldedContinuation"
  - "createTuiChat"
  - "deriveMessages"
  - "tool-cards-hidden-folded"
  - "TUI hidden mode folds a turn's assistant steps into one message"
  - "feature"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "recovery"
  - "context"
  - "llm"
search_regex: "(?i)(StreamingAssistantComponent|StepPosition|setFoldedContinuation|createTuiChat|deriveMessages|tool\\-cards\\-hidden\\-folded|feature|evidence)"
---

# 0599. TUI hidden mode folds a turn's assistant steps into one message — implementation context

## Open this when

Ctrl+O's hidden phase (consolidated TUI presentation) drops tool cards so the transcript reads as a conversation, but each model step still rendered its own Assistant header. A multi-step turn (text → tools → text) therefore showed several consecutive Assistant blocks with nothing between them --- the removed tool cards were the only thing that had justified the repeated headers. Codex-style conversation-only reading wants one assistant message per turn.

## Source decision

Hidden mode is also a fold rule, applied purely as TUI presentation: per turn, the first step whose rendered content is visible (text, or reasoning while reasoning display is on) owns the turn's single Assistant header; every other step renders as a headerless continuation, and a step with no visible body renders nothing at all --- a tool-only step neither consumes the header nor leaves a blank segment. Collapsed and expanded phases keep per-step headers; leaving hidden restores them.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-29-tui-hidden-mode-assistant-fold.md](../02-notes/archived/feature/2026-07-29-tui-hidden-mode-assistant-fold.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-29-tui-hidden-mode-assistant-fold.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-29-tui-hidden-mode-assistant-fold.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

No implementation path could be confirmed from the pinned tree. Use the search handles below; do not invent a source location.

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`packages/core/session/tests/fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/fork.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/session/tests/surface.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/surface.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/agent-loop/tests/resume.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/resume.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/session/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/properties.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/agent-loop/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/invariant.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.

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
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/context`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/streaming`
- Aliases: `StreamingAssistantComponent`, `StepPosition`, `setFoldedContinuation`, `createTuiChat`, `deriveMessages`, `tool-cards-hidden-folded`, `TUI hidden mode folds a turn's assistant steps into one message`, `feature`, `evidence`, `lifecycle`, `ownership`, `recovery`, `context`, `llm`
- Regex: `(?i)(StreamingAssistantComponent|StepPosition|setFoldedContinuation|createTuiChat|deriveMessages|tool\-cards\-hidden\-folded|feature|evidence)`

```bash
rg -n --pcre2 "(?i)(StreamingAssistantComponent|StepPosition|setFoldedContinuation|createTuiChat|deriveMessages|tool\\-cards\\-hidden\\-folded|feature|evidence)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0555. Consolidated TUI presentation and navigation](0555-consolidated-tui-presentation-and-navigation.md): The source note links to this decision directly.
- **`shares-code-with`** — [0530. Deterministic tests, the replay invariant fixture, and race stress](0530-deterministic-tests-the-replay-invariant-fixture-and-race-stress.md): Shares source implementation: `packages/core/session/tests/fork.spec.ts`, `packages/core/session/tests/properties.spec.ts`.
- **`shares-code-with`** — [0370. The chat flow surfaces a max-tokens turn end](0370-the-chat-flow-surfaces-a-max-tokens-turn-end.md): Shares source implementation: `packages/core/agent-loop/tests/loop.spec.ts`, `packages/core/session/tests/fork.spec.ts`.
- **`shares-code-with`** — [0447. Fold trace-only session facts into load-bearing events](0447-fold-trace-only-session-facts-into-load-bearing-events.md): Shares source implementation: `packages/core/session/tests/fork.spec.ts`, `packages/core/session/tests/session.spec.ts`.
- **`shares-code-with`** — [0519. Interactive side sessions and merge-back](0519-interactive-side-sessions-and-merge-back.md): Shares source implementation: `packages/core/agent-loop/tests/resume.spec.ts`, `packages/core/session/tests/fork.spec.ts`.
- **`shares-code-with`** — [0621. TUI step timing trails the step's last message](0621-tui-step-timing-trails-the-step-s-last-message.md): Shares source implementation: `packages/core/agent-loop/tests/tool-calls.spec.ts`, `packages/core/tools/tests/code-mode.spec.ts`.
- **`shares-code-with`** — [0043. Cooperative tool cancellation at the registry boundary](0043-cooperative-tool-cancellation-at-the-registry-boundary.md): Shares source implementation: `packages/core/agent-loop/tests/tool-calls.spec.ts`, `packages/core/tools/tests/code-mode.spec.ts`.
- **`shares-code-with`** — [0635. Generated persistence log event catalog](0635-generated-persistence-log-event-catalog.md): Shares source implementation: `packages/core/session/tests/fork.spec.ts`, `packages/core/session/tests/session.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0599-tui-hidden-mode-folds-a-turn-s-assistant-steps-into-one-message.md`.
