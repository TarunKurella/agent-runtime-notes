---
id: "dsh-note-0320"
title: "The approval takeover shares the composer's text cap"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-30-approval-panel-command-cap.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/trust"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
aliases:
  - "ConversationRoot"
  - "InputBar"
  - "DONE"
  - "data-approval-scroll"
  - "--dsh-composer-text-max-height: 336px"
  - ".composerSeat"
  - "box-sizing: border-box"
  - "tabIndex={0}"
  - "--dsh-scrollbar-thumb{,-hover}"
  - "scrollHeight"
  - "clientHeight"
  - "printf 'alpha %.0s' {1..400}"
  - "apps/web/tests/approval-composer.e2e.ts"
  - "bash: notes.txt: Operation not permitted"
search_regex: "(?i)(ConversationRoot|InputBar|DONE|data\\-approval\\-scroll|\\-\\-dsh\\-composer\\-text\\-max\\-height:[- ]336px|\\.composerSeat|box\\-sizing:[- ]border\\-box|tabIndex=\\{0\\})"
---

# 0320. The approval takeover shares the composer's text cap — implementation context

## Open this when

The approval panel is a composer takeover: while a sandbox escalation waits, it replaces the InputBar in the composer seat with the model's justification, the paired command, and a refuse/allow row. Both texts are unbounded model output, and the card had no height cap. A long command --- the realistic shape, since escalation happens on the command the sandbox just denied, and a denied command is often a long inline write --- grew the card until the action row left the viewport.

## Source decision

The panel's justification and command move into one scroll region (data-approval-scroll) capped at the same height as the composer's draft area; the amber strip and the action row sit outside it, so both buttons are in the card at every content length. The cap is one value with two consumers, declared as --dsh-composer-text-max-height: 336px on ConversationRoot's .composerSeat --- the composer chain's only shared ancestor, since the fallback InputBar and an elected takeover render as siblings.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-30-approval-panel-command-cap.md](../02-notes/implemented/bug-fix/2026-07-30-approval-panel-command-cap.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-30-approval-panel-command-cap.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-30-approval-panel-command-cap.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-theme/src/styles/scrollbar.css`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/styles/scrollbar.css) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/llm/llm-deepseek/src/sse.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/sse.ts) | runtime implementation | Defines `DONE`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx) | runtime implementation | Defines `InputBar`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx) | runtime implementation | Defines `ConversationRoot`, a construct named by the note. | `symbol-definition` |
| [`tsconfig.host.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.host.json) | composition and configuration | Contains the exact code literal `apps/web/tests/approval-composer.e2e.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ConversationRoot` | `function` | [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx#L15) | `export function ConversationRoot({` |
| `InputBar` | `function` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L46) | `export function InputBar({` |
| `DONE` | `const` | [`packages/llm/llm-deepseek/src/sse.ts:18`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/sse.ts#L18) | `export const DONE = '[DONE]'` |

### Tests and executable evidence

- [`apps/web/tests/approval-composer.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/approval-composer.e2e.ts) — The source note names this file directly. A test under the owning area exercises or imports `data-approval-scroll`.
- [`scripts/publint-all.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.spec.ts) — A test under the owning area exercises or imports `tsx`.
- [`scripts/gen-doc-graphs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.spec.ts) — A test under the owning area exercises or imports `var`.
- [`scripts/coverage-exempt.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/coverage-exempt.spec.ts) — A test under the owning area exercises or imports `tsx`.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `data-`.
- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — A test under the owning area exercises or imports `tsx`.
- [`scripts/client-bundle-css.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-css.spec.ts) — A test under the owning area exercises or imports `css`.
- [`scripts/publication-payload.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publication-payload.spec.ts) — A test under the owning area exercises or imports `css`.
- Source verification intent: apps/web/tests/approval-composer.e2e.ts drives the real composition: a read-only session, a denied write, the model's escalation retry, and the answer clicked through the panel. The geometry assertion runs on the live panel at two viewport heights and is guarded against holding vacuously --- the region must actually be scrolling, and the measured cap must equal the composer's own, which the test reads off the live draft scrollport before sending rather than hardcoding the px value. Confirmed both directions against the built client.

## How to read the implementation

1. Start with [`packages/client/ui-theme/src/styles/scrollbar.css`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/styles/scrollbar.css) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/trust`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`
- Aliases: `ConversationRoot`, `InputBar`, `DONE`, `data-approval-scroll`, `--dsh-composer-text-max-height: 336px`, `.composerSeat`, `box-sizing: border-box`, `tabIndex={0}`, `--dsh-scrollbar-thumb{,-hover}`, `scrollHeight`, `clientHeight`, `printf 'alpha %.0s' {1..400}`, `apps/web/tests/approval-composer.e2e.ts`, `bash: notes.txt: Operation not permitted`
- Regex: `(?i)(ConversationRoot|InputBar|DONE|data\-approval\-scroll|\-\-dsh\-composer\-text\-max\-height:[- ]336px|\.composerSeat|box\-sizing:[- ]border\-box|tabIndex=\{0\})`

```bash
rg -n --pcre2 "(?i)(ConversationRoot|InputBar|DONE|data\\-approval\\-scroll|\\-\\-dsh\\-composer\\-text\\-max\\-height:[- ]336px|\\.composerSeat|box\\-sizing:[- ]border\\-box|tabIndex=\\{0\\})" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0327. The composer's two text layers share one scrollport](0327-the-composer-s-two-text-layers-share-one-scrollport.md): The source note links to this decision directly.
- **`shares-code-with`** — [0499. Keep supported-platform tests semantic](0499-keep-supported-platform-tests-semantic.md): Shares source implementation: `scripts/install-lefthook.spec.ts`, `scripts/publint-all.spec.ts`.
- **`shares-code-with`** — [0622. Web conversation UI polish sweep](0622-web-conversation-ui-polish-sweep.md): Shares source implementation: `scripts/coverage-exempt.spec.ts`, `scripts/publint-all.spec.ts`.
- **`shares-code-with`** — [0177. Safe assistant Markdown in the Web conversation](0177-safe-assistant-markdown-in-the-web-conversation.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/InputBar.tsx`.
- **`shares-code-with`** — [0373. Unlink fixture junctions before recursive deletion](0373-unlink-fixture-junctions-before-recursive-deletion.md): Shares source implementation: `scripts/gen-doc-graphs.spec.ts`, `scripts/publint-all.spec.ts`.
- **`shares-code-with`** — [0617. Intent draft echoes in the same tick](0617-intent-draft-echoes-in-the-same-tick.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/client/ui-conversation/src/client/skeleton/InputBar.tsx`.
- **`shares-code-with`** — [0318. Fixed header, sticky composer inside the transcript scrollport](0318-fixed-header-sticky-composer-inside-the-transcript-scrollport.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`.
- **`shares-code-with`** — [0337. Todo-first composer context order](0337-todo-first-composer-context-order.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0320-the-approval-takeover-shares-the-composer-s-text-cap.md`.
