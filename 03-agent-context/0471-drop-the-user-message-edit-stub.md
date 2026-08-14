---
id: "dsh-note-0471"
title: "Drop the user-message edit stub"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-07-31-drop-user-message-edit-stub.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "domain/llm"
  - "domain/testing"
  - "lifecycle/implemented"
aliases:
  - "MessageIconActions"
  - "MessageItem"
  - "Drop the user-message edit stub"
  - "simplification"
  - "concurrency"
  - "evidence"
  - "lifecycle"
  - "llm"
  - "testing"
  - "implemented"
search_regex: "(?i)(MessageIconActions|MessageItem|Drop[- ]the[- ]user\\-message[- ]edit[- ]stub|simplification|concurrency|evidence|lifecycle|testing)"
---

# 0471. Drop the user-message edit stub — implementation context

## Open this when

The user bubble's IconActions row carried an edit button beside copy and branch. Nothing backed it: the control had no click handler, no client mutation, and no host operation for resending an edited message. A user who found it saw an affordance the product cannot honor.

## Source decision

MessageIconActions renders clock / copy / branch only, and its edit prop is gone with the button; MessageItem no longer passes it. The user bubble and the assistant chrome now differ only by clock side. The package README records the missing capability under Known Limitations, and the web message-actions golden pins the row without the control. The common locale keeps its generic edit term, which is shared vocabulary rather than this component's copy.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-07-31-drop-user-message-edit-stub.md](../02-notes/implemented/simplification/2026-07-31-drop-user-message-edit-stub.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-07-31-drop-user-message-edit-stub.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-07-31-drop-user-message-edit-stub.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-conversation/src/client/chat/MessageIconActions.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/MessageIconActions.tsx) | runtime implementation | Defines `MessageIconActions`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `MessageIconActions` | `function` | [`packages/client/ui-conversation/src/client/chat/MessageIconActions.tsx:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/MessageIconActions.tsx#L46) | `export function MessageIconActions({` |

### Tests and executable evidence

- [`packages/client/ui-conversation/tests/chat-view.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-view.client.spec.tsx) — A test under the owning area exercises or imports `MessageItem`.
- [`packages/client/ui-conversation/tests/chat-branch-tails.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-branch-tails.client.spec.tsx) — A test under the owning area exercises or imports `MessageItem`.

## How to read the implementation

1. Start with [`packages/client/ui-conversation/src/client/chat/MessageIconActions.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/MessageIconActions.tsx) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `domain/llm`, `domain/testing`, `lifecycle/implemented`
- Aliases: `MessageIconActions`, `MessageItem`, `Drop the user-message edit stub`, `simplification`, `concurrency`, `evidence`, `lifecycle`, `llm`, `testing`, `implemented`
- Regex: `(?i)(MessageIconActions|MessageItem|Drop[- ]the[- ]user\-message[- ]edit[- ]stub|simplification|concurrency|evidence|lifecycle|testing)`

```bash
rg -n --pcre2 "(?i)(MessageIconActions|MessageItem|Drop[- ]the[- ]user\\-message[- ]edit[- ]stub|simplification|concurrency|evidence|lifecycle|testing)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0477. User and steering bubbles drop the branch action](0477-user-and-steering-bubbles-drop-the-branch-action.md): Shares source implementation: `packages/client/ui-conversation/tests/chat-branch-tails.client.spec.tsx`, `packages/client/ui-conversation/tests/chat-view.client.spec.tsx`.
- **`shares-code-with`** — [0351. Reader scroll attribution through the observed-top ledger](0351-reader-scroll-attribution-through-the-observed-top-ledger.md): Shares source implementation: `packages/client/ui-conversation/tests/chat-view.client.spec.tsx`.
- **`shares-code-with`** — [0318. Fixed header, sticky composer inside the transcript scrollport](0318-fixed-header-sticky-composer-inside-the-transcript-scrollport.md): Shares source implementation: `packages/client/ui-conversation/tests/chat-view.client.spec.tsx`.
- **`shares-code-with`** — [0239. Goal command input projection](0239-goal-command-input-projection.md): Shares source implementation: `packages/client/ui-conversation/tests/chat-view.client.spec.tsx`.
- **`shares-code-with`** — [0624. Web details default closed](0624-web-details-default-closed.md): Shares source implementation: `packages/client/ui-conversation/tests/chat-view.client.spec.tsx`.
- **`same-design-pressure`** — [0654. Drop `GenerateOptions.prefill` and `ToolSchema.strict` --- request knobs with no working end-to-end path](0654-drop-generateoptions-prefill-and-toolschema-strict-request-knobs-with-no.md): Shares design concerns: `concern/concurrency`, `concern/evidence`, `concern/lifecycle`.
- **`same-design-pressure`** — [0474. Drop the Windows PowerShell picker fallback](0474-drop-the-windows-powershell-picker-fallback.md): Shares design concerns: `concern/evidence`, `concern/lifecycle`, `domain/llm`.
- **`same-design-pressure`** — [0534. Drop bash full-output spill files](0534-drop-bash-full-output-spill-files.md): Shares design concerns: `concern/evidence`, `concern/lifecycle`, `domain/llm`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0471-drop-the-user-message-edit-stub.md`.
