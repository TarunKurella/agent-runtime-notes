---
id: "dsh-note-0182"
title: "Session model selection in the Web composer"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-24-web-session-model-selector.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/generation"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "current"
  - "locked"
  - "off"
  - "ModelDirectory"
  - "models"
  - "ModelDirectoryResolver"
  - "ModelSelection"
  - "client"
  - "idle"
  - "model"
  - "loading"
  - "ready"
  - "request/header"
  - "ctx.agentDefaultModel"
search_regex: "(?i)(current|locked|ModelDirectory|models|ModelDirectoryResolver|ModelSelection|client|idle)"
---

# 0182. Session model selection in the Web composer — implementation context

## Open this when

The Web conversation needs a visible, mutable session model selection sourced from the Host. Copying TUI presentation or hardcoding DeepSeek models in the browser would split model discovery and step-boundary semantics across front ends. A switch made while a response is running also needs one atomic boundary: prompt variables and request routing cannot observe different selections.

## Source decision

The Web Host installs ModelSelection for every created or resumed Agent. The provider/model/reasoning selection comes from the latest request/header when the session has used a model, otherwise from ctx.agentDefaultModel. session.selectModel assigns the session-local selection, and prompt assembly captures it with request routing; a switch during a running step therefore applies to the next assembled step. The next consumed selection persists through the full request/header snapshot, while a choice that has not reached a request remains process-local.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-24-web-session-model-selector.md](../02-notes/implemented/feature/2026-07-24-web-session-model-selector.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-24-web-session-model-selector.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-24-web-session-model-selector.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-conversation/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-model-selection/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-model-selection`. | `named-package-member` |
| [`packages/client/ui-conversation/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-model-selection/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-model-selection`. | `named-package-member` |
| [`packages/client/ui-model-selection/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/client/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-model-selection`. Defines `models`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-model-selection/src/client/service.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/client/service.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-model-selection`. Defines `ModelDirectoryResolver`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/input/facade.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/facade.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. Defines `current`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/input/blocks.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/blocks.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. Defines `current`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-model-selection/src/client/directory.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/client/directory.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-model-selection`. Defines `ModelDirectory`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/PermissionSelect.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/PermissionSelect.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. Defines `current`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/conversation-nodes/assistant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/assistant.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. Defines `current`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx) | runtime implementation | Defines `locked`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `current` | `const` | [`packages/client/ui-conversation/src/client/conversation-nodes/assistant.ts:302`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/assistant.ts#L302) | `const current = context.current.get('chat')` |
| `current` | `const` | [`packages/client/ui-conversation/src/client/conversation-nodes/retry.ts:84`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/conversation-nodes/retry.ts#L84) | `const current = attempts.at(-1)` |
| `current` | `const` | [`packages/client/ui-conversation/src/client/input/blocks.ts:59`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/blocks.ts#L59) | `const current = store.getSnapshot()` |
| `current` | `const` | [`packages/client/ui-conversation/src/client/input/facade.ts:144`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/facade.ts#L144) | `const current = new Set(this.imageIds)` |
| `locked` | `const` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:134`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L134) | `const locked = disabled` |
| `current` | `const` | [`packages/client/ui-conversation/src/client/skeleton/PermissionSelect.tsx:88`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/PermissionSelect.tsx#L88) | `const current = value.options.find(option => option.value === currentValue)` |
| `off` | `const` | [`packages/client/ui-layout/src/client/index.ts:150`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/index.ts#L150) | `const off = ctx.on('theme/change', (snapshot) => { presenter.apply(snapshot) })` |
| `ModelDirectory` | `class` | [`packages/client/ui-model-selection/src/client/directory.ts:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/client/directory.ts#L37) | `export class ModelDirectory {` |
| `models` | `const` | [`packages/client/ui-model-selection/src/client/index.ts:124`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/client/index.ts#L124) | `const models = scope.modelDirectories` |
| `models` | `const` | [`packages/client/ui-model-selection/src/client/index.ts:155`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/client/index.ts#L155) | `const models = scope.modelDirectories` |
| `ModelDirectoryResolver` | `class` | [`packages/client/ui-model-selection/src/client/service.ts:34`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/client/service.ts#L34) | `export class ModelDirectoryResolver extends Service {` |
| `ModelSelection` | `interface` | [`packages/core/agent/src/model-selection.ts:10`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/model-selection.ts#L10) | `export interface ModelSelection {` |
| `client` | `const` | [`packages/sdk/client/src/api.ts:148`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/api.ts#L148) | `const client = this.harness.client` |
| `idle` | `const` | [`packages/subagent/subagent/src/continuation.ts:1303`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/continuation.ts#L1303) | `const idle = activation.handle.agent.whenIdle()` |
| `model` | `const` | [`packages/typert/loader/src/index.ts:109`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts#L109) | `const model = requireObject(pkgName, manifest.model, 'TYPERT.model')` |
| `loading` | `let` | [`packages/typert/loader/src/index.ts:344`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts#L344) | `let loading = manifests.get(pkgName)` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/client/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/client`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/client`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/client/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/client`.
- [`packages/client/ui-conversation/tests/skeleton.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/skeleton.client.spec.tsx) — A test under the owning area exercises or imports `locked`.
- [`packages/client/ui-conversation/tests/input-bar.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-bar.client.spec.tsx) — A test under the owning area exercises or imports `locked`.
- [`packages/client/ui-conversation/tests/input-machine.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-machine.client.spec.ts) — A test under the owning area exercises or imports `locked`.
- [`packages/client/ui-conversation/tests/input-matrix.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-matrix.client.spec.tsx) — A test under the owning area exercises or imports `locked`.
- [`packages/client/ui-model-selection/tests/model-select.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/tests/model-select.client.spec.tsx) — A test under the owning area exercises or imports `ModelSelection`. A test under the owning area exercises or imports `locked`.
- Source verification intent: Host tests pin grouped discovery, catalog and exact-metadata failure isolation, logged effort restoration without stale-row injection, advisory unlisted selection, unsupported effort rejection, default materialization, and next-assembly switching. Client tests pin the shared directory, reconnect restoration, and complete-selection submission. Component tests pin dynamic effort labels, descriptions, provider-default exposure, effort submission, and the Select model fallback for an absent row.

## How to read the implementation

1. Start with [`packages/client/ui-conversation/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/generation`, `mechanism/projection`, `mechanism/registry`
- Aliases: `current`, `locked`, `off`, `ModelDirectory`, `models`, `ModelDirectoryResolver`, `ModelSelection`, `client`, `idle`, `model`, `loading`, `ready`, `request/header`, `ctx.agentDefaultModel`
- Regex: `(?i)(current|locked|ModelDirectory|models|ModelDirectoryResolver|ModelSelection|client|idle)`

```bash
rg -n --pcre2 "(?i)(current|locked|ModelDirectory|models|ModelDirectoryResolver|ModelSelection|client|idle)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0076. Projected token usage and context occupancy](0076-projected-token-usage-and-context-occupancy.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0208. Ask-question Web presentation](0208-ask-question-web-presentation.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0292. Web surface for message feedback](0292-web-surface-for-message-feedback.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0214. Plan review as a decision, not a question](0214-plan-review-as-a-decision-not-a-question.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`.
- **`shares-code-with`** — [0177. Safe assistant Markdown in the Web conversation](0177-safe-assistant-markdown-in-the-web-conversation.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0675. Web UI drops steer entry and interjection chrome](0675-web-ui-drops-steer-entry-and-interjection-chrome.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0182-session-model-selection-in-the-web-composer.md`.
