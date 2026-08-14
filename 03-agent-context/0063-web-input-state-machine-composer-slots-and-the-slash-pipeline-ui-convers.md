---
id: "dsh-note-0063"
title: "Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-25-web-input-machine-and-slash-pipeline.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "conversation"
  - "scanTextRefs"
  - "SessionInputShell"
  - "InputMachine"
  - "ConversationRoot"
  - "lexicon"
  - "locked"
  - "redo"
  - "submit"
  - "InputTriggerController"
  - "InputTriggerService"
  - "useAnchoredMaxHeight"
  - "disabled"
  - "undo"
search_regex: "(?i)(conversation|scanTextRefs|SessionInputShell|InputMachine|ConversationRoot|lexicon|locked|redo)"
---

# 0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger) — implementation context

## Open this when

Two composers, each a law unto itself: hero (EmptyState, the controlled chain writing straight into the Session) and the in-conversation InputBar (a plain controlled textarea) --- behavior, draft ownership, and send path all inconsistent. To bring the three trigger families --- / commands, skill references, @ references --- onto the input surface, these had to be answered: How the three trigger families layer, and who holds knowledge of "commands" versus who stays zero-knowledge; How the input box expresses "command mode" --- derived from the draft text or explicit state?

## Source decision

A pure state machine, events in / effects out, clock injected. Four phases (plain / adjudicating / claimed / submitting). Command mode is never derived from the draft; the pick paths establish it explicitly at discrete moments; the claim is watched by draft.startsWith(token), with a backspace break releasing automatically; the claim shape is {token, hint?} (hint feeds ghost text).

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-25-web-input-machine-and-slash-pipeline.md](../02-notes/implemented/architecture/2026-07-25-web-input-machine-and-slash-pipeline.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-25-web-input-machine-and-slash-pipeline.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-25-web-input-machine-and-slash-pipeline.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/jsdoc.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/jsdoc.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. Defines `input`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/surface.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `plan`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/session) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/scope/src/store.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/store.ts) | runtime implementation | Defines `undo`, a construct named by the note. | `symbol-definition` |
| [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) | package entry point | Defines `order`, a construct named by the note. | `symbol-definition` |
| [`packages/lsp/tool-lsp/src/render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/tool-lsp/src/render.ts) | runtime implementation | Defines `notice`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts) | package entry point | Defines `dirty`, a construct named by the note. Defines `model`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/code-mode.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts) | runtime implementation | Defines `release`, a construct named by the note. | `symbol-definition` |
| [`packages/settings/settings/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts) | package entry point | Defines `left`, a construct named by the note. Defines `right`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `conversation` | `const` | [`packages/client/runtime/src/client/index.ts:190`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/index.ts#L190) | `const conversation = {` |
| `scanTextRefs` | `function` | [`packages/client/ui-conversation/src/client/input/decorations.ts:63`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/decorations.ts#L63) | `export function scanTextRefs(` |
| `SessionInputShell` | `class` | [`packages/client/ui-conversation/src/client/input/facade.ts:69`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/facade.ts#L69) | `export class SessionInputShell implements SessionInput {` |
| `InputMachine` | `class` | [`packages/client/ui-conversation/src/client/input/machine.ts:105`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/machine.ts#L105) | `export class InputMachine {` |
| `ConversationRoot` | `function` | [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx#L15) | `export function ConversationRoot({` |
| `lexicon` | `const` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:56`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L56) | `const lexicon = useLexicon(s => s)` |
| `locked` | `const` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:134`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L134) | `const locked = disabled` |
| `redo` | `const` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:303`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L303) | `const redo = e.key === 'y' \|\| e.shiftKey` |
| `submit` | `const` | [`packages/client/ui-conversation/src/client/skeleton/PermissionSelect.tsx:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/PermissionSelect.tsx#L98) | `const submit = (id: string): void => {` |
| `InputTriggerController` | `class` | [`packages/client/ui-input-trigger/src/client/controller.ts:40`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-input-trigger/src/client/controller.ts#L40) | `export class InputTriggerController {` |
| `InputTriggerService` | `class` | [`packages/client/ui-input-trigger/src/client/service.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-input-trigger/src/client/service.ts#L28) | `export class InputTriggerService extends Service implements InputTriggerServiceContract {` |
| `useAnchoredMaxHeight` | `function` | [`packages/client/ui-primitives/src/useAnchoredMaxHeight.ts:21`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/useAnchoredMaxHeight.ts#L21) | `export function useAnchoredMaxHeight(ref: RefObject<HTMLElement>, cap: number, signal: unknown): number {` |
| `disabled` | `const` | [`packages/client/ui-settings-plugins/src/client/BashCard.tsx:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/client/BashCard.tsx#L23) | `const disabled = !state.writable` |
| `undo` | `let` | [`packages/core/scope/src/store.ts:249`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/store.ts#L249) | `let undo: () => void` |
| `input` | `const` | [`packages/core/session/src/index.ts:151`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L151) | `const input: unknown = source === undefined` |
| `header` | `const` | [`packages/core/session/src/index.ts:259`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L259) | `const header = record?.['header']` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model`.
- [`packages/core/scope/tests/store.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/tests/store.spec.ts) — A test under the owning area exercises or imports `undo`.
- [`packages/client/ui-input-trigger/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-input-trigger/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `InputTriggerService`.
- [`packages/client/ui-input-trigger/tests/service.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-input-trigger/tests/service.client.spec.ts) — A test under the owning area exercises or imports `InputTriggerService`. A test under the owning area exercises or imports `InputTriggerController`.
- [`packages/client/ui-conversation/tests/skeleton.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/skeleton.client.spec.tsx) — A test under the owning area exercises or imports `SessionInputShell`. A test under the owning area exercises or imports `ConversationRoot`.
- [`packages/client/ui-conversation/tests/input-bar.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-bar.client.spec.tsx) — A test under the owning area exercises or imports `redo`. A test under the owning area exercises or imports `SessionInputShell`.
- [`packages/client/ui-conversation/tests/chat-view.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-view.client.spec.tsx) — A test under the owning area exercises or imports `submit`.

## How to read the implementation

1. Start with [`scripts/jsdoc.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/jsdoc.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `conversation`, `scanTextRefs`, `SessionInputShell`, `InputMachine`, `ConversationRoot`, `lexicon`, `locked`, `redo`, `submit`, `InputTriggerController`, `InputTriggerService`, `useAnchoredMaxHeight`, `disabled`, `undo`
- Regex: `(?i)(conversation|scanTextRefs|SessionInputShell|InputMachine|ConversationRoot|lexicon|locked|redo)`

```bash
rg -n --pcre2 "(?i)(conversation|scanTextRefs|SessionInputShell|InputMachine|ConversationRoot|lexicon|locked|redo)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0061. Web client Agent-scope parity model and the provisioning channel (agents/scope / blank reuse / provide)](0061-web-client-agent-scope-parity-model-and-the-provisioning-channel-agents.md): The source note links to this decision directly.
- **`source-link`** — [0062. Web command business surfaces and assembly (ui-commands / ui-skill / ui-subagent)](0062-web-command-business-surfaces-and-assembly-ui-commands-ui-skill-ui-subag.md): The source note links to this decision directly.
- **`shares-code-with`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0185. Workspace UI Complete Product Flow](0185-workspace-ui-complete-product-flow.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0460. Collapse live persistence into one flush controller](0460-collapse-live-persistence-into-one-flush-controller.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0038. Initiating Agent scope over AsyncLocalStorage](0038-initiating-agent-scope-over-asynclocalstorage.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0102. Reusable Session preparation before publication](0102-reusable-session-preparation-before-publication.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0141. SessionStore fork API](0141-sessionstore-fork-api.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md`.
