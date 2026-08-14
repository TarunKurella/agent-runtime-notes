---
id: "dsh-note-0214"
title: "Plan review as a decision, not a question"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-30-plan-review-presentation-intent.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "detail"
  - "PendingKind"
  - "ApprovalPanel"
  - "PlanReviewPanel"
  - "QuestionComposer"
  - "question"
  - "planReviewOf"
  - "approve"
  - "PendingQuestion"
  - "header"
  - "AskUserQuestionItem"
  - "exit_plan_mode"
  - "ctx.userQuestions.ask"
  - "ask_user_question"
search_regex: "(?i)(detail|PendingKind|ApprovalPanel|PlanReviewPanel|QuestionComposer|question|planReviewOf|approve)"
---

# 0214. Plan review as a decision, not a question — implementation context

## Open this when

exit_plan_mode presents a finished plan for review through ctx.userQuestions.ask(), the same seam ask_user_question uses. On the Web GUI that made a plan review render as the generic question flow of the ask-question Web presentation: a 1 / 1 pager, the plan as a question's supporting detail, the two verdicts as numbered radio rows with descriptions, an "Other --- enter a custom answer" row, and Skip this question / Submit in the footer. Every one of those affordances is wrong for the surface.

## Source decision

A question may declare a presentation intent, and the Web composer renders a declared intent as its own surface. AskUserQuestionItem gains intent?: AskUserQuestionIntent, a tagged union whose one member is { kind: 'plan-review', approve: string }; plan-mode sets it on the review question, naming Approve as the label that approves. An intent changes presentation only.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-30-plan-review-presentation-intent.md](../02-notes/implemented/feature/2026-07-30-plan-review-presentation-intent.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-30-plan-review-presentation-intent.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-30-plan-review-presentation-intent.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-plan/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-plan/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-plan`. | `named-package-member` |
| [`packages/plan/plan-mode/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/index.ts) | package entry point | Core file in the package named by the note: `packages/plan/plan-mode`. | `named-package-member` |
| [`packages/plan/plan-mode/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/plan/plan-mode`. | `named-package-member` |
| [`packages/client/ui-plan/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-plan/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-plan`. | `named-package-member` |
| [`packages/plan/plan-mode/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/plan/plan-mode`. | `named-package-member` |
| [`packages/client/ui-conversation/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-user-questions/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-user-questions`. | `named-package-member` |
| [`packages/client/ui-conversation/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/interaction/user-questions/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-questions/src/index.ts) | package entry point | Core file in the package named by the note: `packages/interaction/user-questions`. | `named-package-member` |
| [`packages/interaction/user-questions/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-questions/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/interaction/user-questions`. Defines `AskUserQuestionItem`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-user-questions/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-user-questions`. | `named-package-member` |
| [`packages/interaction/user-questions/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-questions/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/interaction/user-questions`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `detail` | `const` | [`packages/acp/acp/src/index.ts:314`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L314) | `const detail = error instanceof Error ? error.message : String(error)` |
| `PendingKind` | `type` | [`packages/client/runtime/src/client/sessions/pending.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/pending.ts#L16) | `export type PendingKind = keyof PendingPayloads` |
| `ApprovalPanel` | `function` | [`packages/client/ui-conversation/src/client/skeleton/ApprovalPanel.tsx:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ApprovalPanel.tsx#L41) | `export function ApprovalPanel(props: ApprovalComposerProps) {` |
| `PlanReviewPanel` | `function` | [`packages/client/ui-user-questions/src/client/PlanReviewPanel.tsx:42`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/PlanReviewPanel.tsx#L42) | `export function PlanReviewPanel({ pending, review, t }: PlanReviewPanelProps) {` |
| `QuestionComposer` | `function` | [`packages/client/ui-user-questions/src/client/QuestionComposer.tsx:60`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/QuestionComposer.tsx#L60) | `export function QuestionComposer(props: QuestionComposerProps) {` |
| `question` | `const` | [`packages/client/ui-user-questions/src/client/QuestionComposer.tsx:63`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/QuestionComposer.tsx#L63) | `const question = useMemo(() => new PendingQuestion(props.matched), [props.matched])` |
| `question` | `const` | [`packages/client/ui-user-questions/src/client/QuestionComposer.tsx:80`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/QuestionComposer.tsx#L80) | `const question = questions[index]!` |
| `planReviewOf` | `function` | [`packages/client/ui-user-questions/src/client/contract/slots.ts:66`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/contract/slots.ts#L66) | `export function planReviewOf(questions: readonly QuestionItem[]): PlanReview \| undefined {` |
| `question` | `const` | [`packages/client/ui-user-questions/src/client/contract/slots.ts:69`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/contract/slots.ts#L69) | `const question = questions[0] as QuestionItem` |
| `approve` | `const` | [`packages/client/ui-user-questions/src/client/contract/slots.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/contract/slots.ts#L75) | `const approve = options.find(option => option.label === intent.approve)` |
| `PendingQuestion` | `class` | [`packages/client/ui-user-questions/src/client/contract/slots.ts:94`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/contract/slots.ts#L94) | `export class PendingQuestion {` |
| `header` | `const` | [`packages/core/session/src/index.ts:259`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L259) | `const header = record?.['header']` |
| `AskUserQuestionItem` | `interface` | [`packages/interaction/user-questions/src/types.ts:35`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-questions/src/types.ts#L35) | `export interface AskUserQuestionItem {` |

### Tests and executable evidence

- [`packages/plan/plan-mode/tests/plan-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/tests/plan-mode.spec.ts) — A test under the owning area exercises or imports `exit_plan_mode`. A test under the owning area exercises or imports `ask_user_question`.
- [`packages/plan/plan-mode/tests/integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/tests/integration.spec.ts) — A test under the owning area exercises or imports `exit_plan_mode`.
- [`packages/client/ui-plan/tests/browser-plugin.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-plan/tests/browser-plugin.client.spec.ts) — A test under the owning area exercises or imports `plan-mode`. A test under the owning area exercises or imports `ui-plan`.
- [`packages/interaction/user-questions/tests/user-questions.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-questions/tests/user-questions.spec.ts) — A test under the owning area exercises or imports `Approve`. A test under the owning area exercises or imports `approve`.
- [`packages/client/ui-user-questions/tests/node-plugin.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/tests/node-plugin.client.spec.ts) — A test under the owning area exercises or imports `ask_user_question`. A test under the owning area exercises or imports `ui-user-questions`.
- [`packages/client/ui-user-questions/tests/browser-plugin.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/tests/browser-plugin.client.spec.ts) — A test under the owning area exercises or imports `QuestionComposer`.
- [`packages/client/ui-user-questions/tests/plan-review-panel.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/tests/plan-review-panel.client.spec.tsx) — A test under the owning area exercises or imports `plan-mode`. A test under the owning area exercises or imports `Approve`.
- [`packages/client/ui-user-questions/tests/user-questions-composer.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/tests/user-questions-composer.client.spec.tsx) — A test under the owning area exercises or imports `QuestionComposer`. A test under the owning area exercises or imports `PendingQuestion`.
- Source verification intent: ui-user-questions tests pin the narrowing (single-question batch, intent present, plan as detail, named approve label offered, binary single choice, decline absent when only approve is offered) and the panel (strip, markdown plan, accessible name, absence of pager/radio/skip/custom, approve and decline answering with the asker's labels, dismissal cancelling, one-shot latch with re-arm and message on a rejected receipt, tooltips present and absent, both locales).

## How to read the implementation

1. Start with [`packages/client/ui-plan/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-plan/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`
- Aliases: `detail`, `PendingKind`, `ApprovalPanel`, `PlanReviewPanel`, `QuestionComposer`, `question`, `planReviewOf`, `approve`, `PendingQuestion`, `header`, `AskUserQuestionItem`, `exit_plan_mode`, `ctx.userQuestions.ask`, `ask_user_question`
- Regex: `(?i)(detail|PendingKind|ApprovalPanel|PlanReviewPanel|QuestionComposer|question|planReviewOf|approve)`

```bash
rg -n --pcre2 "(?i)(detail|PendingKind|ApprovalPanel|PlanReviewPanel|QuestionComposer|question|planReviewOf|approve)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0208. Ask-question Web presentation](0208-ask-question-web-presentation.md): The source note links to this decision directly.
- **`shares-code-with`** — [0458. Plan-specific collaboration state](0458-plan-specific-collaboration-state.md): Shares source implementation: `packages/plan/plan-mode/src/index.ts`, `packages/plan/plan-mode/src/invariant.ts`.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0256. Producer-declared context forms](0256-producer-declared-context-forms.md): Shares source implementation: `packages/plan/plan-mode/src/index.ts`, `packages/plan/plan-mode/src/invariant.ts`.
- **`shares-code-with`** — [0074. dsh source launch through the tsx ESM hook](0074-dsh-source-launch-through-the-tsx-esm-hook.md): Shares source implementation: `packages/plan/plan-mode/src/index.ts`, `packages/plan/plan-mode/src/invariant.ts`.
- **`shares-code-with`** — [0076. Projected token usage and context occupancy](0076-projected-token-usage-and-context-occupancy.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0182. Session model selection in the Web composer](0182-session-model-selection-in-the-web-composer.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0177. Safe assistant Markdown in the Web conversation](0177-safe-assistant-markdown-in-the-web-conversation.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0214-plan-review-as-a-decision-not-a-question.md`.
