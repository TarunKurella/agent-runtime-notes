---
id: "dsh-note-0683"
title: "Snapshot semantic terminal state for the TUI"
status: "archived"
class: "testing"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/testing/2026-07-18-tui-terminal-state-snapshots.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/testing"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "jsonl"
  - "Terminal.write"
  - "packages/ui/tui/tests/tui.spec.ts"
  - "packages/ui/tui/tests/tui.snapshot.ts"
  - "session.jsonl"
  - "session.<n>.jsonl"
  - "terminal.expected.txt"
  - "user/message"
  - "assistant/chunk"
  - "dsh-llm-replay"
  - "DSH_SNAPSHOT=record"
  - "DSH_SNAPSHOT=refresh"
  - "HeadlessTerminal"
  - "@xterm/headless"
search_regex: "(?i)(jsonl|Terminal\\.write|packages/ui/tui/tests/tui\\.spec\\.ts|packages/ui/tui/tests/tui\\.snapshot\\.ts|session\\.jsonl|session\\.<n>\\.jsonl|terminal\\.expected\\.txt|user/message)"
---

# 0683. Snapshot semantic terminal state for the TUI — implementation context

## Open this when

The TUI is a stateful renderer. Its user-visible result depends on ANSI parsing, differential frames, wrapping, scrollback, viewport position, terminal width, focus, cursor state, and each tool's presentation intent. Unit tests that collect Terminal.write() fragments can prove event handling, but they cannot prove the final screen a terminal displays. The same screen may also be emitted through different write fragments, so pinning those fragments creates false regressions. Component-line snapshots stop before ANSI reaches a terminal and miss cursor movement, clearing, styling, overlay composition, and reflow.

## Source decision

Reusable TUI coverage has two complementary package layers: packages/ui/tui/tests/tui.spec.ts tests event mapping, input routing, disposal, and error behavior directly. packages/ui/tui/tests/tui.snapshot.ts mounts the production TUI against a headless terminal emulator for transient states that a completed session log cannot retain: in-flight streaming, pending tool calls, overlays, expansion, compaction reflow, errors, and shutdown. The explicit-config entrypoint decision removed the product TUI composition, recorded application journeys, and PTY suite.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/testing/2026-07-18-tui-terminal-state-snapshots.md](../02-notes/archived/testing/2026-07-18-tui-terminal-state-snapshots.md)
- Pinned source: [.agents/notes/archived/testing/2026-07-18-tui-terminal-state-snapshots.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/testing/2026-07-18-tui-terminal-state-snapshots.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/terminal/terminal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/index.ts) | package entry point | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/terminal/terminal/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/terminal/terminal/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/test-support/llm-replay/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`packages/test-support/llm-replay/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`packages/terminal/terminal`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/test-support/llm-replay`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay) | package or module directory | The note names this package or capability. | `named-package` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Defines `jsonl`, a construct named by the note. Contains the exact code literal `user/message` named by the note. | `exact-code-occurrence, symbol-definition` |
| [`packages/terminal/terminal/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/README.md) | package contract and examples | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/terminal/terminal/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/package.json) | composition and configuration | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/test-support/llm-replay/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/README.md) | package contract and examples | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`packages/test-support/llm-replay/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/package.json) | composition and configuration | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `jsonl` | `const` | [`scripts/gen-doc-graphs.ts:731`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts#L731) | `const jsonl = nodeId('bundle', 'jsonl')` |

### Tests and executable evidence

- [`apps/web/tests/queue-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/queue-actions.e2e.ts) — Contains the exact code literal `dsh-llm-replay` named by the note.
- [`scripts/session-fixture-layout.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/session-fixture-layout.spec.ts) — Contains the exact code literal `assistant/chunk` named by the note.
- [`apps/web/tests/replay-round-trip.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/replay-round-trip.e2e.ts) — Contains the exact code literal `dsh-llm-replay` named by the note.
- [`apps/web/tests/live-interactions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/live-interactions.e2e.ts) — Contains the exact code literal `dsh-llm-replay` named by the note.
- [`apps/web/tests/turn-tail-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/turn-tail-actions.e2e.ts) — Contains the exact code literal `dsh-llm-replay` named by the note.

## How to read the implementation

1. Start with [`packages/terminal/terminal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
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
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/testing`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/jobs-tasks`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `jsonl`, `Terminal.write`, `packages/ui/tui/tests/tui.spec.ts`, `packages/ui/tui/tests/tui.snapshot.ts`, `session.jsonl`, `session.<n>.jsonl`, `terminal.expected.txt`, `user/message`, `assistant/chunk`, `dsh-llm-replay`, `DSH_SNAPSHOT=record`, `DSH_SNAPSHOT=refresh`, `HeadlessTerminal`, `@xterm/headless`
- Regex: `(?i)(jsonl|Terminal\.write|packages/ui/tui/tests/tui\.spec\.ts|packages/ui/tui/tests/tui\.snapshot\.ts|session\.jsonl|session\.<n>\.jsonl|terminal\.expected\.txt|user/message)`

```bash
rg -n --pcre2 "(?i)(jsonl|Terminal\\.write|packages/ui/tui/tests/tui\\.spec\\.ts|packages/ui/tui/tests/tui\\.snapshot\\.ts|session\\.jsonl|session\\.<n>\\.jsonl|terminal\\.expected\\.txt|user/message)" source-deepseek-harness
rg -l --fixed-strings "class/testing" 03-agent-context
```

## Connected notes

- **`source-link`** — [0676. Explicit-config dsh entrypoint](0676-explicit-config-dsh-entrypoint.md): The source note links to this decision directly.
- **`shares-code-with`** — [0470. Capability-neutral sandbox policy context](0470-capability-neutral-sandbox-policy-context.md): Shares source implementation: `packages/terminal/terminal`, `packages/terminal/terminal/README.md`.
- **`shares-code-with`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares source implementation: `packages/terminal/terminal/src/index.ts`, `packages/terminal/terminal/src/invariant.ts`.
- **`shares-code-with`** — [0571. Auto-titled terminal from the first message](0571-auto-titled-terminal-from-the-first-message.md): Shares source implementation: `packages/test-support/llm-replay`, `packages/test-support/llm-replay/src/index.ts`.
- **`shares-code-with`** — [0677. Use `session.jsonl` as the only snapshot session-log artifact](0677-use-session-jsonl-as-the-only-snapshot-session-log-artifact.md): Shares source implementation: `packages/test-support/llm-replay`, `packages/test-support/llm-replay/src/index.ts`.
- **`shares-code-with`** — [0335. Goal-round wrap-up message](0335-goal-round-wrap-up-message.md): Shares source implementation: `packages/test-support/llm-replay`, `packages/test-support/llm-replay/src/index.ts`.
- **`shares-code-with`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): Shares source implementation: `packages/test-support/llm-replay/src/index.ts`, `packages/test-support/llm-replay/src/invariant.ts`.
- **`shares-code-with`** — [0533. Persist assembled assistant messages, not stream chunks](0533-persist-assembled-assistant-messages-not-stream-chunks.md): Shares source implementation: `packages/test-support/llm-replay`, `packages/test-support/llm-replay/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0683-snapshot-semantic-terminal-state-for-the-tui.md`.
