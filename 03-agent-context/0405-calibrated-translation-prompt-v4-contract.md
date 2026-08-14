---
id: "dsh-note-0405"
title: "Calibrated translation prompt v4 contract"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-23-translation-prompt-v4-contract.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/llm"
  - "domain/protocols"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/generation"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "final"
  - "review"
  - "translation-rules.md"
  - "translation-prompt-v4"
  - "Calibrated translation prompt v4 contract"
  - "process"
  - "boundary"
  - "compatibility"
  - "evidence"
  - "human control"
  - "lifecycle"
  - "ownership"
  - "schema types"
  - "build release"
search_regex: "(?i)(final|review|translation\\-rules\\.md|translation\\-prompt\\-v4|Calibrated[- ]translation[- ]prompt[- ]v4[- ]contract|boundary|compatibility|evidence)"
---

# 0405. Calibrated translation prompt v4 contract — implementation context

## Open this when

Automated counterpart generation needs a stable prompt that reproduces the register and corrections established by human-reviewed translations. Injecting a general-purpose instruction document changes that calibrated model input whenever human or agent guidance changes, while an unframed response cannot carry a draft, its self-review, and the corrected document separately. Plain XML-like section tags also collide with valid Markdown that documents those same tags.

## Source decision

The committed translation prompt is the calibrated pipeline asset. Its renderer injects only the source language, target language, and current terminology table, and rejects unknown, missing, or malformed placeholder syntax before assembling a request. The request assembler retains the source basename outside the model-visible prompt and places each reviewed whole-document pair into one bare-text user/assistant example turn before the real source document.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-23-translation-prompt-v4-contract.md](../02-notes/implemented/process/2026-07-23-translation-prompt-v4-contract.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-23-translation-prompt-v4-contract.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-23-translation-prompt-v4-contract.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/i18n/terminology.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/i18n/terminology.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`scripts/translation-prompt.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-prompt.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`docs/i18n/translation-prompt.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/i18n/translation-prompt.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`vendor/loader/src/config/tree.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/tree.ts) | runtime implementation | Defines `final`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/ansi.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ansi.ts) | runtime implementation | Defines `final`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-user-questions/src/client/QuestionComposer.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/QuestionComposer.tsx) | runtime implementation | Defines `review`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/conversation-nodes/assistant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/assistant.ts) | runtime implementation | Defines `final`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/trajectory-assistant-definition.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/trajectory-assistant-definition.ts) | runtime implementation | Defines `final`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `final` | `const` | [`packages/client/ui-conversation/src/client/conversation-nodes/assistant.ts:149`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/assistant.ts#L149) | `const final = state.final` |
| `final` | `const` | [`packages/client/ui-primitives/src/ansi.ts:304`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ansi.ts#L304) | `const final = String(match[2])` |
| `final` | `const` | [`packages/client/ui-trajectory/src/client/trajectory-assistant-definition.ts:205`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/trajectory-assistant-definition.ts#L205) | `const final = state.final` |
| `review` | `const` | [`packages/client/ui-user-questions/src/client/QuestionComposer.tsx:64`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/QuestionComposer.tsx#L64) | `const review = useMemo(() => planReviewOf(question.questions), [question])` |
| `final` | `const` | [`vendor/loader/src/config/tree.ts:79`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/tree.ts#L79) | `const final = parts.pop()!` |

### Tests and executable evidence

- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `translation`.
- [`scripts/translation-brief.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-brief.spec.ts) — A test under the owning area exercises or imports `translation`.
- [`scripts/translation-prompt.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-prompt.spec.ts) — A test under the owning area exercises or imports `translation`. A test under the owning area exercises or imports `xml`.
- [`apps/web/tests/pwa-manifest.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/pwa-manifest.e2e.ts) — A test under the owning area exercises or imports `xml`.
- [`scripts/cordis-config-files.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-config-files.spec.ts) — A test under the owning area exercises or imports `translation`.
- [`scripts/translation-pairing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing.spec.ts) — A test under the owning area exercises or imports `translation`.
- [`scripts/translation-pairing-merge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing-merge.spec.ts) — A test under the owning area exercises or imports `translation`.
- [`apps/web/tests/access-confirmation.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/access-confirmation.e2e.ts) — A test under the owning area exercises or imports `translation`.

## How to read the implementation

1. Start with [`docs/i18n/terminology.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/i18n/terminology.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/process`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `domain/build-release`, `domain/llm`, `domain/protocols`, `domain/shell-terminal`, `domain/storage`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/generation`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `final`, `review`, `translation-rules.md`, `translation-prompt-v4`, `Calibrated translation prompt v4 contract`, `process`, `boundary`, `compatibility`, `evidence`, `human control`, `lifecycle`, `ownership`, `schema types`, `build release`
- Regex: `(?i)(final|review|translation\-rules\.md|translation\-prompt\-v4|Calibrated[- ]translation[- ]prompt[- ]v4[- ]contract|boundary|compatibility|evidence)`

```bash
rg -n --pcre2 "(?i)(final|review|translation\\-rules\\.md|translation\\-prompt\\-v4|Calibrated[- ]translation[- ]prompt[- ]v4[- ]contract|boundary|compatibility|evidence)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0406. Briefed minimal translation updates](0406-briefed-minimal-translation-updates.md): Shares source implementation: `scripts/translation-brief.spec.ts`, `scripts/translation-prompt.ts`.
- **`shares-code-with`** — [0045. Web client architecture --- the client cordis plugin tree, the slot system, and the React-free object layer](0045-web-client-architecture-the-client-cordis-plugin-tree-the-slot-system-an.md): Shares source implementation: `vendor/loader/src/config/tree.ts`.
- **`shares-code-with`** — [0200. Continuable subagents](0200-continuable-subagents.md): Shares source implementation: `vendor/loader/src/config/tree.ts`.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-primitives/src/ansi.ts`.
- **`shares-code-with`** — [0127. Trajectory assembly from registered Conversation Contexts](0127-trajectory-assembly-from-registered-conversation-contexts.md): Shares source implementation: `vendor/loader/src/config/tree.ts`.
- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0223. Web result card frontend --- rendering the web render intent in the browser](0223-web-result-card-frontend-rendering-the-web-render-intent-in-the-browser.md): Shares source implementation: `packages/client/ui-primitives/src/ansi.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0405-calibrated-translation-prompt-v4-contract.md`.
