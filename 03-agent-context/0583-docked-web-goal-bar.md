---
id: "dsh-note-0583"
title: "Docked web goal bar"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-22-docked-web-goal-bar.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "ConversationRoot"
  - "GoalBar"
  - "GoalBarActions"
  - "title"
  - "goal"
  - "/goal"
  - "packages/client/ui-goal/src/client/GoalBar.tsx"
  - "blockedReason.message"
  - "GoalBarActions.onEdit"
  - "onClear"
  - "GOAL_NOT_FOUND"
  - "packages/client/ui-goal/src/client/slots.ts"
  - "onEdit"
  - "onPause"
search_regex: "(?i)(ConversationRoot|GoalBar|GoalBarActions|title|goal|/goal|packages/client/ui\\-goal/src/client/GoalBar\\.tsx|blockedReason\\.message)"
---

# 0583. Docked web goal bar — implementation context

## Open this when

The web UI had no goal surface at all: the goal stack shipped with model tools, the TUI/ACP adapters, and the /goal command, but the browser client exposed none of it --- no runtime verbs, no indicator. This change introduces the client goal verbs (runtime session methods over RPC) and the first goal UI together. Placement follows the redesign's premise that goal presence belongs to the composer's context: the goal is a property of the work the user is about to prompt, so its indicator belongs in the composer-context stack; the composer context stack decision owns its position among Goal, Todo, Queue, and.

## Source decision

GoalBar (packages/client/ui-goal/src/client/GoalBar.tsx) is a props-driven, self-contained component registered second in the composer's input-dock list, after Todo and before Queue. Its standalone 752px card follows the composer's horizontal geometry, and every visible state shares one fixed 36px height so switching phases never resizes it. Loading (goal === undefined), absent (goal === null), and phase === 'complete' render nothing --- a completed goal is history, not chrome.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-22-docked-web-goal-bar.md](../02-notes/archived/feature/2026-07-22-docked-web-goal-bar.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-22-docked-web-goal-bar.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-22-docked-web-goal-bar.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-goal/src/client/slots.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-goal/src/client/slots.ts) | runtime implementation | The source note names this file directly. Defines `GoalBarActions`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/client/ui-goal/src/client/GoalBar.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-goal/src/client/GoalBar.tsx) | runtime implementation | The source note names this file directly. Defines `GoalBar`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/goal/goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts) | package entry point | Core file in the package named by the note: `packages/goal/goal`. Defines `goal`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/goal/goal/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/goal/goal`. | `named-package-member` |
| [`packages/goal/goal/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/goal/goal`. | `named-package-member` |
| [`packages/goal/goal`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/web/src/app.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/app.tsx) | runtime implementation | Defines `title`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx) | runtime implementation | Defines `ConversationRoot`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/README.md) | package contract and examples | Core file in the package named by the note: `packages/goal/goal`. | `named-package-member` |
| [`packages/goal/goal/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/package.json) | composition and configuration | Core file in the package named by the note: `packages/goal/goal`. | `named-package-member` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `agent/inbox/spliced` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Contains the exact code literal `agent/inbox/spliced` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ConversationRoot` | `function` | [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx#L15) | `export function ConversationRoot({` |
| `GoalBar` | `function` | [`packages/client/ui-goal/src/client/GoalBar.tsx:34`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-goal/src/client/GoalBar.tsx#L34) | `export function GoalBar({ goal, onEdit, onPause, onResume, onClear, t }: GoalBarProps & PropsLocale<'goal'>) {` |
| `GoalBarActions` | `interface` | [`packages/client/ui-goal/src/client/slots.ts:20`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-goal/src/client/slots.ts#L20) | `export interface GoalBarActions {` |
| `title` | `const` | [`packages/client/web/src/app.tsx:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/app.tsx#L32) | `const title = useSessions((state) => {` |
| `goal` | `const` | [`packages/goal/goal/src/index.ts:259`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L259) | `const goal: GoalSnapshot = {` |
| `goal` | `const` | [`packages/goal/goal/src/index.ts:283`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L283) | `const goal: GoalSnapshot = {` |
| `goal` | `const` | [`packages/goal/goal/src/index.ts:551`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L551) | `const goal = this.view(cache)` |
| `goal` | `const` | [`packages/goal/goal/src/index.ts:562`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L562) | `const goal = cache.state.goal` |

### Tests and executable evidence

- [`packages/goal/goal/tests/goal.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/tests/goal.spec.ts) — A test under the owning area exercises or imports `GOAL_NOT_FOUND`.
- [`packages/client/ui-goal/tests/goalbar.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-goal/tests/goalbar.client.spec.tsx) — A test under the owning area exercises or imports `GoalBar`. A test under the owning area exercises or imports `GoalBarActions`.
- [`packages/client/ui-goal/tests/browser-plugin.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-goal/tests/browser-plugin.client.spec.tsx) — A test under the owning area exercises or imports `GoalBar`. A test under the owning area exercises or imports `GoalBarActions`.
- [`packages/client/ui-conversation/tests/skeleton.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/skeleton.client.spec.tsx) — A test under the owning area exercises or imports `ConversationRoot`.
- [`packages/client/ui-conversation/tests/views-type-chain.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/views-type-chain.client.spec.tsx) — A test under the owning area exercises or imports `ConversationRoot`.
- [`scripts/client-bundle-purity.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-purity.spec.ts) — Contains the exact code literal `packages/client/ui-goal/src/client/GoalBar.tsx` named by the note.
- [`packages/client/runtime/tests/manager.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/manager.client.spec.ts) — Contains the exact code literal `session/projection` named by the note.
- [`packages/client/runtime/tests/sessions-service.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/sessions-service.client.spec.ts) — Contains the exact code literal `session/projection` named by the note.
- Source verification intent: packages/client/ui-goal/tests/goalbar.spec.tsx pins the behavior through props alone: loading/absent/complete render nothing, the active strip renders label/objective and fires clear, rapid same-frame clear clicks dispatch once and a successful clear hides before projection convergence, the edit form prefills, rejects empty, saves on Enter, cancels on Esc, and resets when the goal's identity changes, the active strip fires pause, the paused strip fires resume, and the blocked strip exposes the reason tooltip.

## How to read the implementation

1. Start with [`packages/client/ui-goal/src/client/slots.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-goal/src/client/slots.ts) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/archived`, `mechanism/policy`, `mechanism/projection`
- Aliases: `ConversationRoot`, `GoalBar`, `GoalBarActions`, `title`, `goal`, `/goal`, `packages/client/ui-goal/src/client/GoalBar.tsx`, `blockedReason.message`, `GoalBarActions.onEdit`, `onClear`, `GOAL_NOT_FOUND`, `packages/client/ui-goal/src/client/slots.ts`, `onEdit`, `onPause`
- Regex: `(?i)(ConversationRoot|GoalBar|GoalBarActions|title|goal|/goal|packages/client/ui\-goal/src/client/GoalBar\.tsx|blockedReason\.message)`

```bash
rg -n --pcre2 "(?i)(ConversationRoot|GoalBar|GoalBarActions|title|goal|/goal|packages/client/ui\\-goal/src/client/GoalBar\\.tsx|blockedReason\\.message)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0090. Goal-owned durable events](0090-goal-owned-durable-events.md): Shares source implementation: `packages/goal/goal`, `packages/goal/goal/README.md`.
- **`shares-code-with`** — [0160. Human `/goal` command](0160-human-goal-command.md): Shares source implementation: `packages/goal/goal/src/index.ts`, `packages/goal/goal/src/invariant.ts`.
- **`shares-code-with`** — [0162. Persisted same-session goal domain](0162-persisted-same-session-goal-domain.md): Shares source implementation: `packages/goal/goal/src/index.ts`, `packages/goal/goal/src/invariant.ts`.
- **`shares-code-with`** — [0164. Same-session goal-round driver](0164-same-session-goal-round-driver.md): Shares source implementation: `packages/goal/goal/src/index.ts`, `packages/goal/goal/src/invariant.ts`.
- **`shares-code-with`** — [0157. Harness-level goal-based execution](0157-harness-level-goal-based-execution.md): Shares source implementation: `packages/goal/goal/src/index.ts`, `packages/goal/goal/src/invariant.ts`.
- **`shares-code-with`** — [0092. Typert Gateway Targeted Method Calls](0092-typert-gateway-targeted-method-calls.md): Shares source implementation: `packages/goal/goal/src/index.ts`, `packages/goal/goal/src/types.ts`.
- **`shares-code-with`** — [0046. Meaningful package invariant contracts](0046-meaningful-package-invariant-contracts.md): Shares source implementation: `packages/goal/goal/src/index.ts`, `packages/goal/goal/src/types.ts`.
- **`shares-code-with`** — [0617. Intent draft echoes in the same tick](0617-intent-draft-echoes-in-the-same-tick.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/client/web/src/app.tsx`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0583-docked-web-goal-bar.md`.
