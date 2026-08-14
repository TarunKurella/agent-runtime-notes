---
id: "dsh-note-0282"
title: "Creator guidance lands as an introduce cue on the preset chip"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-10-creator-guidance-introduce-cue.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/cancellation-timeout"
  - "concern/evidence"
  - "concern/lifecycle"
  - "domain/configuration"
  - "domain/context"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
aliases:
  - "stage"
  - "INTRO_TEXT_DELAY_MS"
  - "min(40, 200/(n-1))"
  - "prefers-reduced-motion"
  - ".introIcon"
  - "apply.spec.ts"
  - "agent-preset-authoring"
  - "Creator guidance lands as an introduce cue on the preset chip"
  - "feature"
  - "cancellation timeout"
  - "evidence"
  - "lifecycle"
  - "configuration"
  - "context"
search_regex: "(?i)(stage|INTRO_TEXT_DELAY_MS|min\\(40,[- ]200/\\(n\\-1\\)\\)|prefers\\-reduced\\-motion|\\.introIcon|apply\\.spec\\.ts|agent\\-preset\\-authoring|feature)"
---

# 0282. Creator guidance lands as an introduce cue on the preset chip — implementation context

## Open this when

Authoring a preset happens inside a Creator-mode session, but the settings section gave no path into that fact. The creator entry sat outside the roster groups, the custom group vanished entirely while it had no member, and clicking the entry dropped the user onto the new-session screen with nothing marking what had changed: the staged preset chip rendered exactly as if the user had picked it by hand. Users reported not understanding that the flow had moved, or that the session they were about to start was the place where the preset gets built (#2184).

## Source decision

The custom group stays on screen while empty --- heading plus the creator entry, which lives inside the group as the standing "your preset will appear here" affordance rather than floating below the roster. A pick staged from another screen carries a one-shot introduce flag through the seat store (stage(id, introduce)), and the chip announces it: the preset icon eases in over 150ms, then the name's characters fade up on a stagger the moment the icon lands.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-10-creator-guidance-introduce-cue.md](../02-notes/implemented/feature/2026-08-10-creator-guidance-introduce-cue.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-10-creator-guidance-introduce-cue.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-10-creator-guidance-introduce-cue.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Defines `stage`, a construct named by the note. | `symbol-definition` |
| [`packages/compaction/compaction-basic/src/region.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/region.ts) | runtime implementation | Defines `stage`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-agent-preset/src/client/AgentPresetSeat.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-agent-preset/src/client/AgentPresetSeat.tsx) | runtime implementation | Defines `INTRO_TEXT_DELAY_MS`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `stage` | `let` | [`packages/boot/app-boot/src/index.ts:767`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L767) | `let stage = 'host preparation failed'` |
| `INTRO_TEXT_DELAY_MS` | `const` | [`packages/client/ui-agent-preset/src/client/AgentPresetSeat.tsx:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-agent-preset/src/client/AgentPresetSeat.tsx#L45) | `const INTRO_TEXT_DELAY_MS = 150` |
| `stage` | `let` | [`packages/compaction/compaction-basic/src/region.ts:198`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/region.ts#L198) | `let stage: TransactionFailure['stage'] = 'summary'` |

### Tests and executable evidence

- [`apps/web/tests/approval-composer.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/approval-composer.e2e.ts) — A test under the owning area exercises or imports `min`.
- [`apps/web/tests/chat-scroll-contract.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/chat-scroll-contract.e2e.ts) — A test under the owning area exercises or imports `min`.
- [`apps/web/tests/chat-long-interactions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/chat-long-interactions.e2e.ts) — A test under the owning area exercises or imports `min`.
- [`apps/web/tests/agent-preset-authoring.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/agent-preset-authoring.e2e.ts) — A test under the owning area exercises or imports `agent-preset-authoring`.
- [`packages/core/session/tests/chunk-rows.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/chunk-rows.spec.ts) — A test under the owning area exercises or imports `min`.
- [`apps/web/tests/snapshots/message-actions/fork.expected.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/message-actions/fork.expected.md) — A test under the owning area exercises or imports `min`.
- [`packages/client/ui-agent-preset/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-agent-preset/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `introduce`.
- [`packages/client/ui-agent-preset/tests/components.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-agent-preset/tests/components.client.spec.tsx) — A test under the owning area exercises or imports `introduce`.
- Source verification intent: Component tests pin the capped stagger (11-character Latin name at 20ms steps, 4-character CJK name at the 40ms tick, single character with no stagger), the acknowledgement timing, and the reduced-motion and empty-name skips. apply.spec.ts drives the cross-screen stage end to end: the creator draft stages with the cue set, one acknowledgement clears it, and a repeat acknowledgement leaves the snapshot untouched. The agent-preset-authoring web e2e holds the empty custom group (heading plus creator entry) in its goldens.

## How to read the implementation

1. Start with [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/cancellation-timeout`, `concern/evidence`, `concern/lifecycle`, `domain/configuration`, `domain/context`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`
- Aliases: `stage`, `INTRO_TEXT_DELAY_MS`, `min(40, 200/(n-1))`, `prefers-reduced-motion`, `.introIcon`, `apply.spec.ts`, `agent-preset-authoring`, `Creator guidance lands as an introduce cue on the preset chip`, `feature`, `cancellation timeout`, `evidence`, `lifecycle`, `configuration`, `context`
- Regex: `(?i)(stage|INTRO_TEXT_DELAY_MS|min\(40,[- ]200/\(n\-1\)\)|prefers\-reduced\-motion|\.introIcon|apply\.spec\.ts|agent\-preset\-authoring|feature)`

```bash
rg -n --pcre2 "(?i)(stage|INTRO_TEXT_DELAY_MS|min\\(40,[- ]200/\\(n\\-1\\)\\)|prefers\\-reduced\\-motion|\\.introIcon|apply\\.spec\\.ts|agent\\-preset\\-authoring|feature)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0618. Question-composer option rows are scroll content, not the slack absorber](0618-question-composer-option-rows-are-scroll-content-not-the-slack-absorber.md): Shares source implementation: `apps/web/tests/approval-composer.e2e.ts`, `apps/web/tests/chat-scroll-contract.e2e.ts`.
- **`shares-code-with`** — [0351. Reader scroll attribution through the observed-top ledger](0351-reader-scroll-attribution-through-the-observed-top-ledger.md): Shares source implementation: `apps/web/tests/approval-composer.e2e.ts`, `apps/web/tests/chat-scroll-contract.e2e.ts`.
- **`shares-code-with`** — [0145. Explicit model-facing tool order](0145-explicit-model-facing-tool-order.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0075. Regroup packages/ by measured clustering](0075-regroup-packages-by-measured-clustering.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0546. Extract example apps into packages](0546-extract-example-apps-into-packages.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0263. The continuable child return channel is an obligation](0263-the-continuable-child-return-channel-is-an-obligation.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0325. Source checkout paths do not define working directories](0325-source-checkout-paths-do-not-define-working-directories.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0375. Bound cold blank-session verification](0375-bound-cold-blank-session-verification.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0282-creator-guidance-lands-as-an-introduce-cue-on-the-preset-chip.md`.
