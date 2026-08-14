---
id: "dsh-note-0324"
title: "Multi-select custom answer composition"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-30-multi-select-custom-answer-composition.md"
implementation_evidence: "medium"
target_anchor: "append-only event and session store"
tags:
  - "class/bug-fix"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/schema-types"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/projection"
aliases:
  - "selected"
  - "custom"
  - "multiSelect: true"
  - "multiSelect"
  - "Multi-select custom answer composition"
  - "bug fix"
  - "evidence"
  - "human control"
  - "schema types"
  - "session state"
  - "shell terminal"
  - "ui interaction"
  - "web retrieval"
  - "implemented"
search_regex: "(?i)(selected|custom|multiSelect:[- ]true|multiSelect|Multi\\-select[- ]custom[- ]answer[- ]composition|bug[- ]fix|evidence|human[- ]control)"
---

# 0324. Multi-select custom answer composition — implementation context

## Open this when

The user-questions result vocabulary carries selected option labels and optional custom text in separate fields, but its original semantics made them mutually exclusive for every question. On a multi-select question, opening or typing the custom answer discarded labels the user had already selected. The TUI returned only the custom text, and the Web host rejected a client response that preserved both fields.

## Source decision

For a question with multiSelect: true, one answer item may contain both a non-empty selected array and non-empty custom text. Web drafts preserve both values regardless of whether the user selects an option or types custom text first; the TUI retains pending custom text across option/custom mode switches and projects it with checked labels from either submit mode; and the Web host accepts the combined response after applying its existing id, label, uniqueness, batch, and non-empty-text validation. Single-select and optionless questions keep exclusive semantics: custom text overrides any selected option.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-30-multi-select-custom-answer-composition.md](../02-notes/implemented/bug-fix/2026-07-30-multi-select-custom-answer-composition.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-30-multi-select-custom-answer-composition.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-30-multi-select-custom-answer-composition.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/util/home-paths/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/index.ts) | package entry point | Defines `selected`, a construct named by the note. | `symbol-definition` |
| [`packages/bundle/headless/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts) | package entry point | Defines `selected`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `selected`, a construct named by the note. Defines `custom`, a construct named by the note. | `symbol-definition` |
| [`packages/api/gateway/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts) | package entry point | Defines `selected`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/analyzer.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts) | runtime implementation | Defines `selected`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-user-questions/src/client/QuestionComposer.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/QuestionComposer.tsx) | runtime implementation | Defines `custom`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `selected` | `const` | [`packages/api/gateway/src/client/index.ts:533`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts#L533) | `const selected = lookupParameters.length === 1 ? lookupParameters[0] : undefined` |
| `selected` | `const` | [`packages/bundle/headless/src/index.ts:116`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts#L116) | `const selected: ModelSelectionRef = { current: selection, assembled: undefined }` |
| `custom` | `const` | [`packages/client/ui-user-questions/src/client/QuestionComposer.tsx:130`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/QuestionComposer.tsx#L130) | `const custom = value.custom.trim()` |
| `custom` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:724`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L724) | `const custom = answer.custom?.trim()` |
| `selected` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2307`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2307) | `const selected: ModelSelection = {` |
| `selected` | `const` | [`packages/typert/generator/src/analyzer.ts:295`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L295) | `const selected = this.options.packages === undefined` |
| `selected` | `const` | [`packages/util/home-paths/src/index.ts:89`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/index.ts#L89) | `const selected = configured ?? (fromEnv !== undefined && fromEnv.trim().length > 0 ? fromEnv : defaultDshHome())` |

### Tests and executable evidence

- [`packages/host/apiproxy/tests/rpc-schemas.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/rpc-schemas.spec.ts) — A test under the owning area exercises or imports `multiSelect`.
- [`packages/client/runtime/tests/manager.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/manager.client.spec.ts) — A test under the owning area exercises or imports `multiSelect`.
- [`packages/host/apiproxy/tests/api-proxy-question.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-question.spec.ts) — A test under the owning area exercises or imports `multiSelect`.
- [`packages/client/ui-user-questions/tests/plan-review-panel.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/tests/plan-review-panel.client.spec.tsx) — A test under the owning area exercises or imports `multiSelect`.
- [`packages/client/ui-user-questions/tests/user-questions-composer.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/tests/user-questions-composer.client.spec.tsx) — A test under the owning area exercises or imports `multiSelect`.

## How to read the implementation

1. Start with [`packages/util/home-paths/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** append-only event and session store.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/evidence`, `concern/human-control`, `concern/schema-types`, `domain/session-state`, `domain/shell-terminal`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/projection`
- Aliases: `selected`, `custom`, `multiSelect: true`, `multiSelect`, `Multi-select custom answer composition`, `bug fix`, `evidence`, `human control`, `schema types`, `session state`, `shell terminal`, `ui interaction`, `web retrieval`, `implemented`
- Regex: `(?i)(selected|custom|multiSelect:[- ]true|multiSelect|Multi\-select[- ]custom[- ]answer[- ]composition|bug[- ]fix|evidence|human[- ]control)`

```bash
rg -n --pcre2 "(?i)(selected|custom|multiSelect:[- ]true|multiSelect|Multi\\-select[- ]custom[- ]answer[- ]composition|bug[- ]fix|evidence|human[- ]control)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0435. Concrete prose names actors and recorded facts](0435-concrete-prose-names-actors-and-recorded-facts.md): Shares source implementation: `packages/api/gateway/src/client/index.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0476. Buffer-free feedback telemetry](0476-buffer-free-feedback-telemetry.md): Shares source implementation: `packages/client/runtime/tests/manager.client.spec.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0366. Preset cards clamp their description instead of sizing the roster](0366-preset-cards-clamp-their-description-instead-of-sizing-the-roster.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/typert/generator/src/analyzer.ts`.
- **`shares-code-with`** — [0619. Tool-card single-row fields render inline](0619-tool-card-single-row-fields-render-inline.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/typert/generator/src/analyzer.ts`.
- **`shares-code-with`** — [0194. Trajectory inspection ledger](0194-trajectory-inspection-ledger.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0379. pnpm as the package manager instead of Yarn 4](0379-pnpm-as-the-package-manager-instead-of-yarn-4.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/typert/generator/src/analyzer.ts`.
- **`shares-code-with`** — [0224. Web result card --- a structured render intent for web_search and web_fetch](0224-web-result-card-a-structured-render-intent-for-web-search-and-web-fetch.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0324-multi-select-custom-answer-composition.md`.
