---
id: "dsh-note-0239"
title: "Goal command input projection"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-01-goal-command-input-projection.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/simplification"
  - "domain/agent-loop"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "composerPhase"
  - "blank"
  - "command/run"
  - "command/done"
  - "/goal"
  - "user/message"
  - "ui-goal"
  - "command-input"
  - "Session.composerPhase"
  - "summary.blank"
  - "/<name><args.trimEnd()>"
  - "turn/start"
  - "step/start"
  - "request/header"
search_regex: "(?i)(composerPhase|blank|command/run|command/done|/goal|user/message|ui\\-goal|command\\-input)"
---

# 0239. Goal command input projection — implementation context

## Open this when

Human commands execute outside the model turn and persist as command/run plus command/done. The Web transcript rendered only their result row. On a fresh session, /goal therefore cleared the composer and completed successfully while the page stayed on the empty hero; its result became visible only after later conversation content activated Chat. Appending an ordinary user/message from the handler would change model-visible history and command semantics.

## Source decision

The command registry and durable command lifecycle remain unchanged. command/run records the parser-owned name, optional verbatim arguments, source, and invocation id; command/done records settlement. Neither event carries browser presentation intent. The ui-goal client plugin registers a Goal-owned Conversation Definition beside the generic command Definition. Both match the same /goal command/run: the generic Definition retains the durable result row, while the Goal Definition builds a separate command-input Chat Node at an earlier fractional anchor.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-01-goal-command-input-projection.md](../02-notes/implemented/feature/2026-08-01-goal-command-input-projection.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-01-goal-command-input-projection.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-01-goal-command-input-projection.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-goal/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-goal`. | `named-package-member` |
| [`packages/client/ui-goal/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-goal/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-goal`. | `named-package-member` |
| [`packages/client/ui-goal`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-goal) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `blank`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx) | runtime implementation | Defines `composerPhase`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-goal/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-goal/README.md) | package contract and examples | Core file in the package named by the note: `packages/client/ui-goal`. | `named-package-member` |
| [`packages/client/ui-goal/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-goal/package.json) | composition and configuration | Core file in the package named by the note: `packages/client/ui-goal`. | `named-package-member` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `user/message` named by the note. Contains the exact code literal `turn/start` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) | package contract and examples | Contains the exact code literal `user/message` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.zh.md) | package contract and examples | Contains the exact code literal `user/message` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `user/message` named by the note. Contains the exact code literal `turn/start` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `user/message` named by the note. Contains the exact code literal `turn/start` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `composerPhase` | `const` | [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx:20`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx#L20) | `const composerPhase = useSession(s => s.composerPhase)` |
| `blank` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:513`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L513) | `const blank = state.blank && event.type !== 'turn/start'` |

### Tests and executable evidence

- [`packages/client/ui-goal/tests/browser-plugin.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-goal/tests/browser-plugin.client.spec.tsx) — A test under the owning area exercises or imports `ui-goal`. A test under the owning area exercises or imports `command-input`.
- [`packages/client/ui-conversation/tests/skeleton.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/skeleton.client.spec.tsx) — A test under the owning area exercises or imports `composerPhase`.
- [`packages/client/ui-conversation/tests/chat-view.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-view.client.spec.tsx) — A test under the owning area exercises or imports `composerPhase`.
- [`packages/client/ui-conversation/tests/input-bar.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-bar.client.spec.tsx) — A test under the owning area exercises or imports `composerPhase`.
- [`packages/client/ui-goal/tests/goal-command-input.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-goal/tests/goal-command-input.client.spec.tsx) — A test under the owning area exercises or imports `command-input`.
- [`packages/client/ui-conversation/tests/queue-dock.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/queue-dock.client.spec.tsx) — A test under the owning area exercises or imports `composerPhase`.
- [`packages/client/ui-conversation/tests/chat-stats.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-stats.client.spec.tsx) — A test under the owning area exercises or imports `composerPhase`.
- [`packages/client/ui-conversation/tests/input-matrix.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-matrix.client.spec.tsx) — A test under the owning area exercises or imports `composerPhase`.
- Source verification intent: Goal client tests pin the dual Definition output, ordering, other-command exclusion, bare and multiline text, done-only cuts, renderer semantics, disposal, and fresh-session phase selection. The keyless assembled Web scenario submits bare /goal in a fresh session with no model adapter, verifies both rows and the absence of model-surface events, then reloads and verifies the persisted transcript.

## How to read the implementation

1. Start with [`packages/client/ui-goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-goal/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/simplification`, `domain/agent-loop`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/projection`, `mechanism/registry`
- Aliases: `composerPhase`, `blank`, `command/run`, `command/done`, `/goal`, `user/message`, `ui-goal`, `command-input`, `Session.composerPhase`, `summary.blank`, `/<name><args.trimEnd()>`, `turn/start`, `step/start`, `request/header`
- Regex: `(?i)(composerPhase|blank|command/run|command/done|/goal|user/message|ui\-goal|command\-input)`

```bash
rg -n --pcre2 "(?i)(composerPhase|blank|command/run|command/done|/goal|user/message|ui\\-goal|command\\-input)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0318. Fixed header, sticky composer inside the transcript scrollport](0318-fixed-header-sticky-composer-inside-the-transcript-scrollport.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/client/ui-conversation/tests/chat-view.client.spec.tsx`.
- **`shares-code-with`** — [0275. The no-Workspace composer opens the existing picker](0275-the-no-workspace-composer-opens-the-existing-picker.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/client/ui-conversation/tests/skeleton.client.spec.tsx`.
- **`shares-code-with`** — [0322. Composer context stack order](0322-composer-context-stack-order.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/client/ui-conversation/tests/skeleton.client.spec.tsx`.
- **`shares-code-with`** — [0194. Trajectory inspection ledger](0194-trajectory-inspection-ledger.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0070. A capability-discriminated directory-picker seam for the web-GUI host](0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0427. Ordered Build for API Remotes Generated Contracts](0427-ordered-build-for-api-remotes-generated-contracts.md): Shares source implementation: `packages/client/ui-goal/src/index.ts`, `packages/client/ui-goal/src/invariant.ts`.
- **`shares-code-with`** — [0185. Workspace UI Complete Product Flow](0185-workspace-ui-complete-product-flow.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0239-goal-command-input-projection.md`.
