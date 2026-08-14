---
id: "dsh-note-0058"
title: "Separate context injection from turn execution"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-24-separate-context-injection-from-turn-execution.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "inject"
  - "source"
  - "additionalContexts"
  - "contexts"
  - "next"
  - "messages"
  - "UserMessage"
  - "HookContext[]"
  - "SendOptions.contexts"
  - "agent.inject"
  - "prompt-prefix"
  - "HookContext"
  - "PreStepDecision.messages"
  - "user/message"
search_regex: "(?i)(inject|source|additionalContexts|contexts|next|messages|UserMessage|HookContext\\[\\])"
---

# 0058. Separate context injection from turn execution — implementation context

## Open this when

The agent API represented supplementary model-facing input in three overlapping ways: callers attached HookContext[] through SendOptions.contexts, interception and tool hooks returned additionalContexts, and plugins called agent.inject(). These paths eventually wrote context into the same model history, but carried different placement, metadata, admission, queue, and turn-lifecycle rules. Atomic attachment to an inbox message forced the loop to preserve context through prompt admission, steering conversion, cancellation, and terminal discard.

## Source decision

inject() is the only caller-facing operation for supplementary model-facing input, and a turn means one execution of the model loop. A caller that owns context delivers an identified, frozen UserMessage through inject() and submits the direct message independently with followup() or steer(). An entering pre-step returns the complete PreStepDecision.messages batch for the request being finalized. Tool extension points still return additionalContexts, which enter the next-step inbox only after the corresponding tool results.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-24-separate-context-injection-from-turn-execution.md](../02-notes/implemented/architecture/2026-07-24-separate-context-injection-from-turn-execution.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-24-separate-context-injection-from-turn-execution.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-24-separate-context-injection-from-turn-execution.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/hmr/src/error.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/error.ts) | runtime implementation | Defines `source`, a construct named by the note. | `symbol-definition` |
| [`vendor/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts) | package entry point | Defines `next`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts) | runtime implementation | Defines `next`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/registry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts) | runtime implementation | Defines `inject`, a construct named by the note. | `symbol-definition` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `inject`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `messages`, a construct named by the note. Defines `source`, a construct named by the note. | `symbol-definition` |
| [`vendor/cosmokit/src/string.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cosmokit/src/string.ts) | runtime implementation | Defines `next`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `source`, a construct named by the note. Defines `next`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/invariant.ts) | runtime contract checks | Defines `inject`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/message.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/message.ts) | runtime implementation | Defines `UserMessage`, a construct named by the note. | `symbol-definition` |
| [`vendor/schemastery/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/schemastery/src/index.ts) | package entry point | Defines `messages`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `additionalContexts`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `inject` | `const` | [`packages/acp/acp/src/index.ts:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L44) | `export const inject = ['agents']` |
| `source` | `const` | [`packages/api/gateway/src/index.ts:562`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts#L562) | `const source = Function.prototype.toString.call(implementation)` |
| `inject` | `const` | [`packages/client/hmr/src/index.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts#L28) | `export const inject = ['clientModules', 'webServer']` |
| `additionalContexts` | `const` | [`packages/core/tools/src/index.ts:1760`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1760) | `const additionalContexts = [` |
| `additionalContexts` | `const` | [`packages/core/tools/src/index.ts:1920`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1920) | `const additionalContexts = prior?.additionalContexts ?? []` |
| `additionalContexts` | `const` | [`packages/core/tools/src/index.ts:1934`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1934) | `const additionalContexts = prior?.additionalContexts ?? []` |
| `contexts` | `const` | [`packages/extensions/tool-cordis/src/index.ts:387`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/src/index.ts#L387) | `const contexts = ids.map((id) => {` |
| `inject` | `const` | [`packages/fs/fs/src/invariant.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/invariant.ts#L12) | `export const inject = ['invariants']` |
| `next` | `const` | [`packages/goal/goal/src/fold.ts:205`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L205) | `const next = change.goal` |
| `source` | `const` | [`packages/goal/goal/src/fold.ts:322`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L322) | `const source = goalSource(event.data.source)` |
| `messages` | `const` | [`packages/llm/llm-pi-ai/src/context.ts:89`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/context.ts#L89) | `const messages: PiMessage[] = []` |
| `messages` | `const` | [`packages/llm/llm-pi-ai/src/context.ts:145`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/context.ts#L145) | `const messages: PiMessage[] = []` |
| `messages` | `const` | [`packages/llm/llm/src/index.ts:824`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L824) | `const messages: Message[] = options.messages.map((message) => {` |
| `source` | `const` | [`packages/llm/llm/src/index.ts:825`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L825) | `const source = message.source` |
| `next` | `const` | [`packages/llm/llm/src/index.ts:877`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L877) | `const next = await iterator.next()` |
| `UserMessage` | `interface` | [`packages/llm/llm/src/message.ts:141`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/message.ts#L141) | `export interface UserMessage extends Message {` |

### Tests and executable evidence

- [`scripts/test-invariants.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-invariants.ts) — A test under the owning area exercises or imports `injection`.
- [`apps/web/tests/steering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/steering.e2e.ts) — A test under the owning area exercises or imports `steer`. A test under the owning area exercises or imports `next-step`.
- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — A test under the owning area exercises or imports `injection`.
- [`apps/web/tests/schedule-after.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/schedule-after.e2e.ts) — A test under the owning area exercises or imports `followup`.
- [`apps/web/tests/built-boot.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/built-boot.snapshot.ts) — A test under the owning area exercises or imports `injection`.
- [`apps/web/tests/message-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/message-actions.e2e.ts) — A test under the owning area exercises or imports `injection`.
- [`apps/web/tests/settings-chrome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/settings-chrome.e2e.ts) — A test under the owning area exercises or imports `steer`.
- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `additionalContexts`.
- Source verification intent: Delivery inputs and steering inbox records contain no attached contexts; agent/inbox/inserted reports only the inserted message, while the durable splice retains its target list. UserMessage is the shared identified, frozen shape across prompt interception, tool execution, hook bridges, guards, and context producers. Prompt-prefix placement, prompt envelopes, and context/message are absent from public types, durable events, projection, and UI replay. Idle inject() immediately appends one durable inbox insertion but no model-visible user/message; a later waking delivery may start pre-step processing.

## How to read the implementation

1. Start with [`vendor/hmr/src/error.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/error.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `concern/trust`, `domain/agent-loop`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `inject`, `source`, `additionalContexts`, `contexts`, `next`, `messages`, `UserMessage`, `HookContext[]`, `SendOptions.contexts`, `agent.inject`, `prompt-prefix`, `HookContext`, `PreStepDecision.messages`, `user/message`
- Regex: `(?i)(inject|source|additionalContexts|contexts|next|messages|UserMessage|HookContext\[\])`

```bash
rg -n --pcre2 "(?i)(inject|source|additionalContexts|contexts|next|messages|UserMessage|HookContext\\[\\])" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0168. Cross-session references](0168-cross-session-references.md): The source note links to this decision directly.
- **`source-link`** — [0455. Remove implicit batching from ordinary sends](0455-remove-implicit-batching-from-ordinary-sends.md): The source note links to this decision directly.
- **`source-link`** — [0457. Project injected content verbatim, dropping the XML envelopes](0457-project-injected-content-verbatim-dropping-the-xml-envelopes.md): The source note links to this decision directly.
- **`source-link`** — [0466. Remove synthetic turns for log-only events](0466-remove-synthetic-turns-for-log-only-events.md): The source note links to this decision directly.
- **`shares-code-with`** — [0088. Claim inbox input before one pre-step decision](0088-claim-inbox-input-before-one-pre-step-decision.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0200. Continuable subagents](0200-continuable-subagents.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/goal/goal/src/fold.ts`.
- **`shares-code-with`** — [0078. Terminal LLM stream failures](0078-terminal-llm-stream-failures.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0454. Simplify session-log representation](0454-simplify-session-log-representation.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/llm/llm/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0058-separate-context-injection-from-turn-execution.md`.
