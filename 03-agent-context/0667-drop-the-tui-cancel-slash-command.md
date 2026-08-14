---
id: "dsh-note-0667"
title: "Drop the TUI `/cancel` slash command"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-07-21-tui-remove-cancel-command.md"
implementation_evidence: "medium"
target_anchor: "ContextFrame compiler and evidence-preserving compaction"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/context"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
aliases:
  - "cancelled"
  - "/cancel"
  - "Ctrl+C"
  - "agent.cancel"
  - "Enter sends steering, Esc cancels"
  - "/help"
  - "baseCommands"
  - "case '/cancel"
  - "/clear"
  - "/reasoning"
  - "/tools"
  - "/redraw"
  - "/reload"
  - "/resume"
search_regex: "(?i)(cancelled|/cancel|Ctrl\\+C|agent\\.cancel|Enter[- ]sends[- ]steering,[- ]Esc[- ]cancels|/help|baseCommands|case[- ]'/cancel)"
---

# 0667. Drop the TUI `/cancel` slash command — implementation context

## Open this when

The TUI exposed two identical ways to cancel a running turn: the Esc (and Ctrl+C) keybinding and a /cancel slash command. Both called agent.cancel('cancelled from terminal') with the same reason; when idle, /cancel only printed a "The agent is already idle." notice while the keybindings stayed silent. The running status line already advertises the keybinding (Enter sends steering, Esc cancels), and cancelling by keystroke needs no editor submission, so the slash command was a second, less discoverable path to the same effect --- surface area with no behavior of its own.

## Source decision

/cancel is removed. Cancelling a running turn is a keybinding-only affordance (Esc, or Ctrl+C while running), which the status-line hint and the /help shortcut list already document. The baseCommands autocomplete entry, the /help command line, the case '/cancel' branch in the editor submit handler, and the "already idle" notice it owned are gone; every other slash command (/help, /clear, /reasoning, /tools, /redraw, /reload, /resume, /exit, /skill:) is unchanged. Typing /cancel now falls through to the generic Unknown command: warning like any other unrecognized slash input.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-07-21-tui-remove-cancel-command.md](../02-notes/archived/simplification/2026-07-21-tui-remove-cancel-command.md)
- Pinned source: [.agents/notes/archived/simplification/2026-07-21-tui-remove-cancel-command.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-07-21-tui-remove-cancel-command.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `cancelled`, a construct named by the note. | `symbol-definition` |
| [`packages/schedule/schedule/src/tools.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/tools.ts) | runtime implementation | Defines `cancelled`, a construct named by the note. | `symbol-definition` |
| [`packages/context/session-reference/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/index.ts) | package entry point | Defines `cancelled`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `cancelled` | `function` | [`packages/context/session-reference/src/index.ts:299`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/index.ts#L299) | `function cancelled(signal: AbortSignal): SessionReferenceError {` |
| `cancelled` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2037`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2037) | `const cancelled = () => err<{ items: SessionSearchItem[]; hasMore: boolean }>(request, {` |
| `cancelled` | `const` | [`packages/schedule/schedule/src/tools.ts:193`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/tools.ts#L193) | `const cancelled = cancellationPlaceholder(signal)` |

### Tests and executable evidence

- [`packages/client/ui-goal/tests/goalbar.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-goal/tests/goalbar.client.spec.tsx) — A test under the owning area exercises or imports `Esc`.
- Source verification intent: packages/ui/tui/tests/tui.spec.ts asserts agent.cancelled contains 'cancelled from terminal', driven by the Esc/Ctrl+C keystrokes in that turn --- the sole cancel affordance. The errors-and-help and disposed-terminal snapshots pin the /help line without /cancel; per-file coverage on packages/ui/tui/src stays at 100%.

## How to read the implementation

1. Start with [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** ContextFrame compiler and evidence-preserving compaction.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/context`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`
- Aliases: `cancelled`, `/cancel`, `Ctrl+C`, `agent.cancel`, `Enter sends steering, Esc cancels`, `/help`, `baseCommands`, `case '/cancel`, `/clear`, `/reasoning`, `/tools`, `/redraw`, `/reload`, `/resume`
- Regex: `(?i)(cancelled|/cancel|Ctrl\+C|agent\.cancel|Enter[- ]sends[- ]steering,[- ]Esc[- ]cancels|/help|baseCommands|case[- ]'/cancel)`

```bash
rg -n --pcre2 "(?i)(cancelled|/cancel|Ctrl\\+C|agent\\.cancel|Enter[- ]sends[- ]steering,[- ]Esc[- ]cancels|/help|baseCommands|case[- ]'/cancel)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `packages/context/session-reference/src/index.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0168. Cross-session references](0168-cross-session-references.md): Shares source implementation: `packages/context/session-reference/src/index.ts`.
- **`shares-code-with`** — [0070. A capability-discriminated directory-picker seam for the web-GUI host](0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0252. Web transcript marks context source, recall, and steering](0252-web-transcript-marks-context-source-recall-and-steering.md): Shares source implementation: `packages/context/session-reference/src/index.ts`.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0194. Trajectory inspection ledger](0194-trajectory-inspection-ledger.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0130. Multiplex concurrent ACP sessions over one connection](0130-multiplex-concurrent-acp-sessions-over-one-connection.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0127. Trajectory assembly from registered Conversation Contexts](0127-trajectory-assembly-from-registered-conversation-contexts.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0667-drop-the-tui-cancel-slash-command.md`.
