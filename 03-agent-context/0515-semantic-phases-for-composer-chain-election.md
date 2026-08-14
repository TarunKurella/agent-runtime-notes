---
id: "dsh-note-0515"
title: "Semantic phases for composer-chain election"
status: "proposed"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/architecture/2026-08-08-semantic-composer-chain-phases.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/trust"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/proposed"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "interactions"
  - "SlotMap"
  - "composer"
  - "SlotSpec"
  - "SlotCore"
  - "priority"
  - "SubagentReadOnlyComposer"
  - "interaction"
  - "conversation.composer"
  - "Semantic phases for composer-chain election"
  - "architecture"
  - "boundary"
  - "compatibility"
  - "concurrency"
search_regex: "(?i)(interactions|SlotMap|composer|SlotSpec|SlotCore|priority|SubagentReadOnlyComposer|interaction)"
---

# 0515. Semantic phases for composer-chain election — implementation context

## Open this when

The browser's conversation.composer chain orders every candidate by one global numeric priority, then elects the first selector returning a match. Question uses the default priority 0, approval uses 1, and the one-shot or unavailable-parent read-only subagent composer uses -10. A selected one-shot history can therefore show the read-only explanation while an answerable question or approval is pending underneath it. The defect is not one incorrect number.

## Source decision

A chain declaration may define an ordered tuple of domain-owned phases. conversation.composer declares ['interaction', 'restriction']; every registration on that phased chain must name one phase, and its numeric priority orders entries only within that phase. SlotCore sorts by declared phase index, then local priority, then stable registration order. Registration fails immediately when a phased chain entry omits its phase or names one outside the declaration. Unphased chains retain their current numeric behavior.

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/architecture/2026-08-08-semantic-composer-chain-phases.md](../02-notes/proposed/architecture/2026-08-08-semantic-composer-chain-phases.md)
- Pinned source: [.agents/notes/proposed/architecture/2026-08-08-semantic-composer-chain-phases.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/architecture/2026-08-08-semantic-composer-chain-phases.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/plan/plan-mode/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/index.ts) | package entry point | Defines `interaction`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-slots/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts) | package entry point | Defines `priority`, a construct named by the note. Defines `SlotCore`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/slots.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/slots.ts) | runtime implementation | Defines `SlotMap`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-layout/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/index.ts) | package entry point | Defines `SlotMap`, a construct named by the note. | `symbol-definition` |
| [`packages/extensions/ui-cordis/src/client/slots.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/slots.ts) | runtime implementation | Defines `SlotMap`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-tool/src/client/contract/slots.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/contract/slots.ts) | runtime implementation | Defines `SlotMap`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/sessions/manager.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts) | runtime implementation | Defines `interactions`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/ChatView.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx) | runtime implementation | Defines `composer`, a construct named by the note. | `symbol-definition` |
| [`packages/extensions/cordis-client-runner/src/client/guard.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-client-runner/src/client/guard.ts) | runtime implementation | Defines `priority`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-subagent/src/client/SubagentReadOnlyComposer.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-subagent/src/client/SubagentReadOnlyComposer.tsx) | runtime implementation | Defines `SubagentReadOnlyComposer`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx) | runtime implementation | Defines `composer`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `interactions` | `let` | [`packages/client/runtime/src/client/sessions/manager.ts:657`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts#L657) | `let interactions = this.pendingInteractions.get(sessionId)` |
| `interactions` | `const` | [`packages/client/runtime/src/client/sessions/manager.ts:669`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts#L669) | `const interactions = this.pendingInteractions.get(sessionId)` |
| `SlotMap` | `interface` | [`packages/client/runtime/src/client/slots.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/slots.ts#L26) | `interface SlotMap {` |
| `composer` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:55`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L55) | `const composer = scrollport.querySelector<HTMLElement>('[data-composer-seat]')` |
| `composer` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:336`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L336) | `const composer = scrollport.querySelector<HTMLElement>('[data-composer-seat]')` |
| `composer` | `const` | [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx:170`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx#L170) | `const composer = renderSlotChain(` |
| `SlotMap` | `interface` | [`packages/client/ui-layout/src/client/index.ts:34`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/index.ts#L34) | `interface SlotMap {` |
| `SlotMap` | `interface` | [`packages/client/ui-slots/src/index.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L24) | `export interface SlotMap {}` |
| `SlotSpec` | `type` | [`packages/client/ui-slots/src/index.ts:130`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L130) | `export type SlotSpec<E extends SlotEntryDef> = {` |
| `SlotCore` | `class` | [`packages/client/ui-slots/src/index.ts:678`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L678) | `export class SlotCore {` |
| `priority` | `const` | [`packages/client/ui-slots/src/index.ts:796`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L796) | `const priority = options.priority ?? 0` |
| `SubagentReadOnlyComposer` | `function` | [`packages/client/ui-subagent/src/client/SubagentReadOnlyComposer.tsx:19`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-subagent/src/client/SubagentReadOnlyComposer.tsx#L19) | `export function SubagentReadOnlyComposer({` |
| `SlotMap` | `interface` | [`packages/client/ui-tool/src/client/contract/slots.ts:8`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/contract/slots.ts#L8) | `interface SlotMap {` |
| `priority` | `let` | [`packages/extensions/cordis-client-runner/src/client/guard.ts:122`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-client-runner/src/client/guard.ts#L122) | `let priority = options.priority` |
| `SlotMap` | `interface` | [`packages/extensions/ui-cordis/src/client/slots.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/slots.ts#L24) | `interface SlotMap {` |
| `interaction` | `const` | [`packages/plan/plan-mode/src/index.ts:330`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/index.ts#L330) | `const interaction = ctx.get('userQuestions')` |

### Tests and executable evidence

- [`packages/core/tools/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/scoped.spec.ts) — A test under the owning area exercises or imports `restriction`.
- [`native/landlock-run/test/launcher.test.js`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/test/launcher.test.js) — A test under the owning area exercises or imports `restriction`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `restriction`.
- [`packages/core/system-prompt/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/tests/scoped.spec.ts) — A test under the owning area exercises or imports `restriction`.
- [`packages/client/ui-slots/tests/core.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/tests/core.client.spec.ts) — A test under the owning area exercises or imports `priority`. A test under the owning area exercises or imports `SlotCore`.
- [`packages/client/runtime/tests/session.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/session.client.spec.ts) — A test under the owning area exercises or imports `interactions`.
- [`packages/client/runtime/tests/manager.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/manager.client.spec.ts) — A test under the owning area exercises or imports `interactions`.
- [`packages/sandbox/sandbox-windows-acl/tests/probe.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/tests/probe.spec.ts) — A test under the owning area exercises or imports `restriction`.
- Source verification intent: SlotCore tests prove phase order dominates arbitrary local priorities, local priority and stable registration order still work within a phase, unknown or omitted phases fail loud, and unphased chains are unchanged. Composer tests cover question plus read-only, approval plus read-only, question plus approval plus read-only, resolution back to read-only, and the all-declined InputBar fallback. Question remains ahead of approval within interaction.

## How to read the implementation

1. Start with [`packages/plan/plan-mode/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/trust`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/proposed`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `interactions`, `SlotMap`, `composer`, `SlotSpec`, `SlotCore`, `priority`, `SubagentReadOnlyComposer`, `interaction`, `conversation.composer`, `Semantic phases for composer-chain election`, `architecture`, `boundary`, `compatibility`, `concurrency`
- Regex: `(?i)(interactions|SlotMap|composer|SlotSpec|SlotCore|priority|SubagentReadOnlyComposer|interaction)`

```bash
rg -n --pcre2 "(?i)(interactions|SlotMap|composer|SlotSpec|SlotCore|priority|SubagentReadOnlyComposer|interaction)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0334. Reject human interaction from runtime-owned subagents](0334-reject-human-interaction-from-runtime-owned-subagents.md): The source note links to this decision directly.
- **`source-link`** — [0178. Web UI permission presets and approval answering](0178-web-ui-permission-presets-and-approval-answering.md): The source note links to this decision directly.
- **`source-link`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): The source note links to this decision directly.
- **`source-link`** — [0214. Plan review as a decision, not a question](0214-plan-review-as-a-decision-not-a-question.md): The source note links to this decision directly.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-slots/src/index.ts`.
- **`shares-code-with`** — [0113. Client Conversation business-node assembly and keyed Chat snapshots](0113-client-conversation-business-node-assembly-and-keyed-chat-snapshots.md): Shares source implementation: `packages/client/ui-conversation/src/client/chat/ChatView.tsx`.
- **`shares-code-with`** — [0278. Web background-job display](0278-web-background-job-display.md): Shares source implementation: `packages/client/runtime/src/client/sessions/manager.ts`.
- **`shares-code-with`** — [0458. Plan-specific collaboration state](0458-plan-specific-collaboration-state.md): Shares source implementation: `packages/plan/plan-mode/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0515-semantic-phases-for-composer-chain-election.md`.
