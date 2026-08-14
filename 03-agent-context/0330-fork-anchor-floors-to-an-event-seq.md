---
id: "dsh-note-0330"
title: "Fork anchor floors to an event seq"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-31-fork-anchor-floors-to-event-seq.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/agent-loop"
  - "domain/llm"
  - "domain/session-state"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "aborted"
  - "turnEnd.seq - 0.9"
  - "session.fork"
  - "turn/end"
  - "SessionRuntime.fork"
  - "atSeq"
  - "dsh-client-runtime"
  - "turn/start"
  - "turnEnd.seq - 1"
  - "forkAt"
  - "ui-conversation"
  - "assistant/message"
  - "Fork anchor floors to an event seq"
  - "bug fix"
search_regex: "(?i)(aborted|turnEnd\\.seq[- ]\\-[- ]0\\.9|session\\.fork|turn/end|SessionRuntime\\.fork|atSeq|dsh\\-client\\-runtime|turn/start)"
---

# 0330. Fork anchor floors to an event seq — implementation context

## Open this when

The fork button on a stopped assistant message did nothing at all --- no child session, no error, no visible reaction. The frozen node behind that message is not a log event. Both the live projection and the history replay mint it with a flow-ordering seq of turnEnd.seq - 0.9, placing it strictly after every event of the aborted turn and before the next one, and the chat view hands that node seq to the fork entry point unchanged.

## Source decision

SessionRuntime.fork floors atSeq before the RPC. The fractional-seq convention belongs to dsh-client-runtime, which mints it in both the live and replay projections, so the same package converts it back to a real event seq at the wire boundary instead of every UI caller remembering to. Integer anchors are unaffected. Flooring lands inside the anchor's own turn rather than clipping backward: every turn opens with turn/start, so turnEnd.seq - 1 cannot itself be an earlier turn's turn/end.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-31-fork-anchor-floors-to-event-seq.md](../02-notes/implemented/bug-fix/2026-07-31-fork-anchor-floors-to-event-seq.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-31-fork-anchor-floors-to-event-seq.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-31-fork-anchor-floors-to-event-seq.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/runtime/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/runtime`. | `named-package-member` |
| [`packages/client/runtime/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/runtime`. | `named-package-member` |
| [`packages/client/ui-conversation/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-conversation/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/runtime`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/ui-conversation`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Defines `aborted`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/README.md) | package contract and examples | Core file in the package named by the note: `packages/client/runtime`. | `named-package-member` |
| [`packages/client/runtime/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/package.json) | composition and configuration | Core file in the package named by the note: `packages/client/runtime`. | `named-package-member` |
| [`packages/client/ui-conversation/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/README.md) | package contract and examples | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-conversation/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/package.json) | composition and configuration | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-client-runtime` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `aborted` | `const` | [`packages/core/agent-loop/src/index.ts:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L98) | `const aborted = Promise.withResolvers<never>()` |

### Tests and executable evidence

- [`packages/client/runtime/tests/sessions-service.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/sessions-service.client.spec.ts) — A test under the owning area exercises or imports `atSeq`.
- [`packages/client/ui-conversation/tests/chat-view.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-view.client.spec.tsx) — A test under the owning area exercises or imports `forkAt`.
- [`packages/client/ui-conversation/tests/apply-inject.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/apply-inject.client.spec.tsx) — A test under the owning area exercises or imports `atSeq`. A test under the owning area exercises or imports `forkAt`.
- [`apps/web/tests/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/README.zh.md) — Contains the exact code literal `dsh-client-runtime` named by the note.
- [`scripts/client-bundle-purity.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-purity.spec.ts) — Contains the exact code literal `dsh-client-runtime` named by the note.

## How to read the implementation

1. Start with [`packages/client/runtime/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/index.ts) because it has the strongest evidence link to the note.
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

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/agent-loop`, `domain/llm`, `domain/session-state`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `aborted`, `turnEnd.seq - 0.9`, `session.fork`, `turn/end`, `SessionRuntime.fork`, `atSeq`, `dsh-client-runtime`, `turn/start`, `turnEnd.seq - 1`, `forkAt`, `ui-conversation`, `assistant/message`, `Fork anchor floors to an event seq`, `bug fix`
- Regex: `(?i)(aborted|turnEnd\.seq[- ]\-[- ]0\.9|session\.fork|turn/end|SessionRuntime\.fork|atSeq|dsh\-client\-runtime|turn/start)`

```bash
rg -n --pcre2 "(?i)(aborted|turnEnd\\.seq[- ]\\-[- ]0\\.9|session\\.fork|turn/end|SessionRuntime\\.fork|atSeq|dsh\\-client\\-runtime|turn/start)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0253. Web turn and window latency/throughput metrics](0253-web-turn-and-window-latency-throughput-metrics.md): Shares source implementation: `packages/client/ui-conversation`, `packages/client/ui-conversation/README.md`.
- **`shares-code-with`** — [0486. Remove the steering interjection caption](0486-remove-the-steering-interjection-caption.md): Shares source implementation: `packages/client/ui-conversation`, `packages/client/ui-conversation/README.md`.
- **`shares-code-with`** — [0675. Web UI drops steer entry and interjection chrome](0675-web-ui-drops-steer-entry-and-interjection-chrome.md): Shares source implementation: `packages/client/ui-conversation/README.md`, `packages/client/ui-conversation/src/index.ts`.
- **`shares-code-with`** — [0076. Projected token usage and context occupancy](0076-projected-token-usage-and-context-occupancy.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0398. GUI testing system --- the three-tier structure](0398-gui-testing-system-the-three-tier-structure.md): Shares source implementation: `packages/client/runtime/src/index.ts`, `packages/client/runtime/src/invariant.ts`.
- **`shares-code-with`** — [0177. Safe assistant Markdown in the Web conversation](0177-safe-assistant-markdown-in-the-web-conversation.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0539. Prune unused skill registry API](0539-prune-unused-skill-registry-api.md): Shares source implementation: `packages/client/runtime`, `packages/client/runtime/src/index.ts`.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0330-fork-anchor-floors-to-an-event-seq.md`.
