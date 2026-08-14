---
id: "dsh-note-0053"
title: "Unify agent delivery on send(target × wakeup) and coalesce injected context into user/message"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-22-unified-send-and-coalesced-user-messages.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/agent-loop"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "plugin"
  - "cancel"
  - "target"
  - "inject"
  - "SurfaceEventType"
  - "wakeup"
  - "claim"
  - "MessageId"
  - "context"
  - "MessageSource"
  - "UserMessage"
  - "send"
  - "running"
  - "source"
search_regex: "(?i)(plugin|cancel|target|inject|SurfaceEventType|wakeup|claim|MessageId)"
---

# 0053. Unify agent delivery on send(target × wakeup) and coalesce injected context into user/message — implementation context

## Open this when

The agent's public driving surface had grown three near-parallel verbs --- send, steer, inject --- each with its own options type, its own live event story, and its own durable event. send and steer both queued a frozen inbox record and emitted agent/queued; inject bypassed the inbox and wrote a separate context/message durable event. The three verbs actually vary along only two independent axes: which queue an item joins (a whole new turn versus the active turn) and whether the item makes the model run.

## Source decision

One primitive, three preset aliases. The Agent interface's send(message, target, wakeup) covers the (target × wakeup) matrix. Its complete UserMessage owns identity, role, model-facing content, and producer source; the remaining arguments own only routing policy. followup (next-turn/wakeup), steer (next-step/wakeup), and inject (next-step/no-wakeup) each accept that one message and fix the policy. wakeup reserves a driver when the agent is idle; an already active driver receives no second reservation and can claim the input only if it reaches a later pre-step boundary.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-22-unified-send-and-coalesced-user-messages.md](../02-notes/implemented/architecture/2026-07-22-unified-send-and-coalesced-user-messages.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-22-unified-send-and-coalesced-user-messages.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-22-unified-send-and-coalesced-user-messages.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. Defines `target`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/runtime-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/runtime-types.ts) | runtime implementation | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent) | package or module directory | The note names this package or capability. | `named-package` |
| [`apps/cli/src/args.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts) | runtime implementation | Defines `plugin`, a construct named by the note. | `symbol-definition` |
| [`vendor/hmr/src/error.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/error.ts) | runtime implementation | Defines `source`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/brand.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts) | runtime implementation | Defines `MessageId`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `context`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/message.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/message.ts) | runtime implementation | Defines `UserMessage`, a construct named by the note. Defines `MessageSource`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Defines `SurfaceEventType`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/code-mode.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts) | runtime implementation | Defines `wakeup`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `plugin` | `const` | [`apps/cli/src/args.ts:171`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts#L171) | `const plugin = program.command('plugin').description('manage a profile\'s plugins by forwarding the remaining arguments to pnpm in the profile directory')` |
| `cancel` | `const` | [`packages/client/ui-primitives/src/pointer-grace.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/pointer-grace.ts#L36) | `const cancel = useCallback(() => {` |
| `target` | `const` | [`packages/core/agent/src/index.ts:378`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L378) | `const target = (factory as AgentFactory & { [symbols.original]?: AgentFactory })[symbols.original] ?? factory` |
| `inject` | `const` | [`packages/core/agent/src/invariant.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts#L12) | `export const inject = ['invariants']` |
| `SurfaceEventType` | `type` | [`packages/core/session/src/types.ts:343`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L343) | `export type SurfaceEventType =` |
| `wakeup` | `const` | [`packages/core/tools/src/code-mode.ts:381`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L381) | `const wakeup = (): void => {` |
| `claim` | `const` | [`packages/extensions/cordis-client-runner/src/client/runtime.ts:415`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-client-runner/src/client/runtime.ts#L415) | `const claim = (component: unknown): void => {` |
| `MessageId` | `type` | [`packages/llm/llm/src/brand.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts#L16) | `export type MessageId = Branded<'MessageId'>` |
| `context` | `const` | [`packages/llm/llm/src/index.ts:648`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L648) | `const context = resolved.context` |
| `MessageSource` | `type` | [`packages/llm/llm/src/message.ts:126`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/message.ts#L126) | `export type MessageSource = MessageSourceMap[keyof MessageSourceMap]` |
| `UserMessage` | `interface` | [`packages/llm/llm/src/message.ts:141`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/message.ts#L141) | `export interface UserMessage extends Message {` |
| `send` | `const` | [`packages/lsp/lsp-stdio/src/instance.ts:206`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/instance.ts#L206) | `const send = this.connection.request(requestMethod(operation), params)` |
| `running` | `const` | [`packages/shell/bash-local/src/index.ts:257`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts#L257) | `const running = this.ctx.subprocess.spawn(this.spawnSpec(spec, argv, this.config.maxOutputBytes, spec.signal))` |
| `source` | `const` | [`vendor/hmr/src/error.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/error.ts#L24) | `const source = readFileSync(file, 'utf8')` |

### Tests and executable evidence

- [`packages/core/agent/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/agent.spec.ts) — A test under the owning area exercises or imports `steer`. A test under the owning area exercises or imports `UserMessage`.
- [`packages/core/session/tests/surface.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/surface.spec.ts) — A test under the owning area exercises or imports `SurfaceEventType`.
- [`packages/lsp/lsp-stdio/tests/fixture-server.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/fixture-server.ts) — A test under the owning area exercises or imports `send`.
- [`packages/core/agent/tests/consumed-work.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/consumed-work.spec.ts) — A test under the owning area exercises or imports `next-turn`.
- [`packages/core/session/tests/gen-persistence-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/gen-persistence-catalog.spec.ts) — A test under the owning area exercises or imports `SurfaceEventType`.
- [`apps/web/tests/goal-multi-turn-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/goal-multi-turn-actions.e2e.ts) — Contains the exact code literal `goal/change` named by the note.
- [`apps/cli/tests/fixtures/dsh-badge/snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/dsh-badge/snapshot.ts) — Contains the exact code literal `agent/pre-step` named by the note.
- [`apps/web/tests/snapshots/goal-multi-turn-actions/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/goal-multi-turn-actions/session.jsonl) — Contains the exact code literal `goal/change` named by the note.

## How to read the implementation

1. Start with [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `concern/simplification`, `domain/agent-loop`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`
- Aliases: `plugin`, `cancel`, `target`, `inject`, `SurfaceEventType`, `wakeup`, `claim`, `MessageId`, `context`, `MessageSource`, `UserMessage`, `send`, `running`, `source`
- Regex: `(?i)(plugin|cancel|target|inject|SurfaceEventType|wakeup|claim|MessageId)`

```bash
rg -n --pcre2 "(?i)(plugin|cancel|target|inject|SurfaceEventType|wakeup|claim|MessageId)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0659. Remove the `agent/steering` mirror emit](0659-remove-the-agent-steering-mirror-emit.md): The source note links to this decision directly.
- **`source-link`** — [0042. Explicit turn cancellation capability](0042-explicit-turn-cancellation-capability.md): The source note links to this decision directly.
- **`source-link`** — [0071. Create every message as an identified immutable value](0071-create-every-message-as-an-identified-immutable-value.md): The source note links to this decision directly.
- **`source-link`** — [0088. Claim inbox input before one pre-step decision](0088-claim-inbox-input-before-one-pre-step-decision.md): The source note links to this decision directly.
- **`shares-code-with`** — [0363. Bounded background job admission](0363-bounded-background-job-admission.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0463. Intent-named subagent continuation operations](0463-intent-named-subagent-continuation-operations.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0038. Initiating Agent scope over AsyncLocalStorage](0038-initiating-agent-scope-over-asynclocalstorage.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0011. Agent lifecycle and ownership contracts](0011-agent-lifecycle-and-ownership-contracts.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0053-unify-agent-delivery-on-send-target-wakeup-and-coalesce-injected-context.md`.
