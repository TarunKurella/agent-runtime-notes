---
id: "dsh-note-0208"
title: "Ask-question Web presentation"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-29-ask-question-web-presentation.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/extensions"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/registry"
aliases:
  - "PendingWait"
  - "ChatView"
  - "selected"
  - "ToolRow"
  - "QuestionComposer"
  - "question"
  - "answered"
  - "custom"
  - "interrupted"
  - "cancelled"
  - "waiting"
  - "PendingCard"
  - "ask_user_question"
  - "ASK_CANCELLED"
search_regex: "(?i)(PendingWait|ChatView|selected|ToolRow|QuestionComposer|question|answered|custom)"
---

# 0208. Ask-question Web presentation — implementation context

## Open this when

The Web GUI could already collect answers through the QuestionComposer composer takeover, but the transcript around it was wrong on three counts. A pending question rendered twice: once as the composer takeover and once as the read-only PendingCard placeholder that predates the takeover. A settled ask_user_question call rendered as the generic "Tool call" row dumping raw args JSON, so the two composer verdicts --- the user dismissing the whole set (ASK_CANCELLED) and a turn interrupt landing while the question was pending (ASK_ABORTED) --- both read as anonymous red-dot failures.

## Source decision

A pending question owns exactly two surfaces: the composer takeover collects the answers, and a dedicated ask_user_question toolview row in the transcript names the interaction outcome. The row registers into the keyed tool.call.toolview hole exactly like todo_write and composes the shared ToolRow (chrome, running sweep, leading expansion).

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-29-ask-question-web-presentation.md](../02-notes/implemented/feature/2026-07-29-ask-question-web-presentation.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-29-ask-question-web-presentation.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-29-ask-question-web-presentation.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/locale/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/locale`. | `named-package-member` |
| [`packages/client/locale/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/locale`. | `named-package-member` |
| [`packages/client/ui-primitives/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/client/ui-primitives/src/Menu.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Menu.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. Defines `selected`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-primitives/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/client/ui-user-questions/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-user-questions`. | `named-package-member` |
| [`packages/client/ui-conversation/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-user-questions/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-user-questions`. | `named-package-member` |
| [`packages/client/ui-conversation/src/client/chat/ChatView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. Defines `ChatView`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-user-questions/src/client/contract/slots.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/contract/slots.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-user-questions`. Defines `question`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-user-questions/src/client/QuestionComposer.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/QuestionComposer.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-user-questions`. Defines `QuestionComposer`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `PendingWait` | `class` | [`packages/client/runtime/src/client/sessions/pending.ts:34`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/pending.ts#L34) | `export class PendingWait<K extends PendingKind = PendingKind> {` |
| `ChatView` | `function` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:146`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L146) | `export function ChatView({` |
| `selected` | `const` | [`packages/client/ui-primitives/src/Menu.tsx:211`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Menu.tsx#L211) | `const selected = entry.id === selectedId \|\| selectedIds?.includes(entry.id) === true` |
| `ToolRow` | `function` | [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx:128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx#L128) | `export function ToolRow({` |
| `QuestionComposer` | `function` | [`packages/client/ui-user-questions/src/client/QuestionComposer.tsx:60`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/QuestionComposer.tsx#L60) | `export function QuestionComposer(props: QuestionComposerProps) {` |
| `question` | `const` | [`packages/client/ui-user-questions/src/client/QuestionComposer.tsx:63`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/QuestionComposer.tsx#L63) | `const question = useMemo(() => new PendingQuestion(props.matched), [props.matched])` |
| `question` | `const` | [`packages/client/ui-user-questions/src/client/QuestionComposer.tsx:80`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/QuestionComposer.tsx#L80) | `const question = questions[index]!` |
| `selected` | `const` | [`packages/client/ui-user-questions/src/client/QuestionComposer.tsx:102`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/QuestionComposer.tsx#L102) | `const selected = current.selected.includes(label)` |
| `answered` | `const` | [`packages/client/ui-user-questions/src/client/QuestionComposer.tsx:114`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/QuestionComposer.tsx#L114) | `const answered = (item: DraftAnswer): boolean =>` |
| `custom` | `const` | [`packages/client/ui-user-questions/src/client/QuestionComposer.tsx:130`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/QuestionComposer.tsx#L130) | `const custom = value.custom.trim()` |
| `selected` | `const` | [`packages/client/ui-user-questions/src/client/QuestionComposer.tsx:217`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/QuestionComposer.tsx#L217) | `const selected = draft.selected.includes(option.label)` |
| `question` | `const` | [`packages/client/ui-user-questions/src/client/contract/slots.ts:69`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/contract/slots.ts#L69) | `const question = questions[0] as QuestionItem` |
| `interrupted` | `const` | [`packages/client/ui-workflow-run/src/client/workflow-definition.ts:95`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workflow-run/src/client/workflow-definition.ts#L95) | `const interrupted = state.stopReason === undefined` |
| `cancelled` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2037`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2037) | `const cancelled = () => err<{ items: SessionSearchItem[]; hasMore: boolean }>(request, {` |
| `waiting` | `const` | [`packages/lsp/lsp-stdio/src/connection.ts:319`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/connection.ts#L319) | `const waiting = [...this.pending.values()]` |

### Tests and executable evidence

- [`packages/client/ui-tool/tests/web-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/web-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/tool-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/tool-row.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/diff-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/diff-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/read-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/read-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/search-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/search-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/terminal-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/terminal-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/tool-row-styles.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/tool-row-styles.client.spec.ts) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/coverage-tails.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/coverage-tails.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- Source verification intent: ui-conversation tests pin the row's waiting/answered/skipped/cancelled/interrupted/fallback matrix, the approval-only pending filter, and the slot registration; ui-user-questions tests pin the redesigned composer (checkbox multi-select, always-visible custom row, footer pager, dictionary-key feedback re-translation, IME-safe Enter) and the plugin's dictionary registration plus inject face; ui-primitives tests pin the icon set. The assembled Web GUI was exercised against a live session covering answer, cancel, and turn-interrupt paths.

## How to read the implementation

1. Start with [`packages/client/locale/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/extensions`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/registry`
- Aliases: `PendingWait`, `ChatView`, `selected`, `ToolRow`, `QuestionComposer`, `question`, `answered`, `custom`, `interrupted`, `cancelled`, `waiting`, `PendingCard`, `ask_user_question`, `ASK_CANCELLED`
- Regex: `(?i)(PendingWait|ChatView|selected|ToolRow|QuestionComposer|question|answered|custom)`

```bash
rg -n --pcre2 "(?i)(PendingWait|ChatView|selected|ToolRow|QuestionComposer|question|answered|custom)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0320. The approval takeover shares the composer's text cap](0320-the-approval-takeover-shares-the-composer-s-text-cap.md): The source note links to this decision directly.
- **`source-link`** — [0178. Web UI permission presets and approval answering](0178-web-ui-permission-presets-and-approval-answering.md): The source note links to this decision directly.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0177. Safe assistant Markdown in the Web conversation](0177-safe-assistant-markdown-in-the-web-conversation.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0294. Web attachment display aligns with DeepSeek Chat via attachment atoms](0294-web-attachment-display-aligns-with-deepseek-chat-via-attachment-atoms.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0111. Client Tool presentation ownership](0111-client-tool-presentation-ownership.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/ChatView.tsx`, `packages/client/ui-conversation/src/index.ts`.
- **`shares-code-with`** — [0214. Plan review as a decision, not a question](0214-plan-review-as-a-decision-not-a-question.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0225. Web search card --- the grep and glob render intent reaches the browser](0225-web-search-card-the-grep-and-glob-render-intent-reaches-the-browser.md): Shares source implementation: `packages/client/ui-primitives/src/index.ts`, `packages/client/ui-primitives/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0208-ask-question-web-presentation.md`.
