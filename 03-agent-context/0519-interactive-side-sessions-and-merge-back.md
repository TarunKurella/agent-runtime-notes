---
id: "dsh-note-0519"
title: "Interactive side sessions and merge-back"
status: "proposed"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/feature/2026-07-08-interactive-side-sessions.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/proposed"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "parentSession"
  - "seedLength"
  - "ctx.agents.create"
  - "context/message"
  - "forkName"
  - "mergedInto"
  - "sidechat/*"
  - "tools/pre-execute"
  - "Interactive side sessions and merge-back"
  - "feature"
  - "boundary"
  - "evidence"
  - "ownership"
  - "performance"
search_regex: "(?i)(parentSession|seedLength|ctx\\.agents\\.create|context/message|forkName|mergedInto|sidechat/\\*|tools/pre\\-execute)"
---

# 0519. Interactive side sessions and merge-back — implementation context

## Open this when

A user may want to explore a question from a live session without changing its main context. Existing primitives do not expose that product shape: session-store fork creates an unattached session, while fork subagents are model-driven tasks whose transcript collapses into one tool result. Neither gives the user a separate conversation, and neither records a conclusion in the parent together with the side session that produced it.

## Source decision

A side session is an ordinary live session forked at the source's last completed turn, attached to its own agent, framed as a read-only advisor, and able to merge back one condensed note. Fork and attach: create the child with the parent's balanced completed-turn prefix and stamp parentSession and seedLength in its metadata. This composes ctx.agents.create({ seed, meta }); it adds no core service or session-store method. Advisor framing: inject one plugin-sourced context/message after creation that tells the child to explain without mutating or continuing the task.

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/feature/2026-07-08-interactive-side-sessions.md](../02-notes/proposed/feature/2026-07-08-interactive-side-sessions.md)
- Pinned source: [.agents/notes/proposed/feature/2026-07-08-interactive-side-sessions.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/feature/2026-07-08-interactive-side-sessions.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/sdk/server/src/server.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/server.ts) | runtime implementation | Defines `parentSession`, a construct named by the note. | `symbol-definition` |
| [`packages/subagent/subagent/src/continuation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/continuation.ts) | runtime implementation | Defines `parentSession`, a construct named by the note. | `symbol-definition` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `tools/pre-execute` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `tools/pre-execute` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Contains the exact code literal `tools/pre-execute` named by the note. | `exact-code-occurrence` |
| [`docs/event-producer-consumer.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/event-producer-consumer.md) | package contract and examples | Contains the exact code literal `tools/pre-execute` named by the note. | `exact-code-occurrence` |
| [`docs/tool-execution-pipeline.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-execution-pipeline.md) | package contract and examples | Contains the exact code literal `tools/pre-execute` named by the note. | `exact-code-occurrence` |
| [`docs/cookbook/adding-a-tool.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cookbook/adding-a-tool.zh.md) | package contract and examples | Contains the exact code literal `tools/pre-execute` named by the note. | `exact-code-occurrence` |
| [`docs/event-producer-consumer.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/event-producer-consumer.zh.md) | package contract and examples | Contains the exact code literal `tools/pre-execute` named by the note. | `exact-code-occurrence` |
| [`docs/tool-execution-pipeline.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-execution-pipeline.zh.md) | package contract and examples | Contains the exact code literal `tools/pre-execute` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `parentSession` | `const` | [`packages/sdk/server/src/server.ts:79`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/server.ts#L79) | `const parentSession = session.header.parentSession` |
| `parentSession` | `let` | [`packages/subagent/subagent/src/continuation.ts:822`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/continuation.ts#L822) | `let parentSession = agent.session.header.parentSession` |

### Tests and executable evidence

- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `seedLength`.
- [`packages/core/session/tests/fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/fork.spec.ts) — A test under the owning area exercises or imports `seedLength`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `seedLength`.
- [`apps/web/tests/chat-long-interactions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/chat-long-interactions.e2e.ts) — A test under the owning area exercises or imports `seedLength`.
- [`packages/core/agent-loop/tests/resume.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/resume.spec.ts) — A test under the owning area exercises or imports `seedLength`.
- [`packages/host/apiproxy/tests/api-proxy-cold.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-cold.spec.ts) — A test under the owning area exercises or imports `seedLength`.
- [`packages/workflow/tool-ralph/tests/integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/tests/integration.spec.ts) — A test under the owning area exercises or imports `seedLength`.
- [`packages/test-support/llm-replay/tests/llm-replay.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/tests/llm-replay.spec.ts) — A test under the owning area exercises or imports `seedLength`.
- Source verification intent: Forking leaves the source untouched and creates a child with the balanced completed-turn prefix, parentSession, seedLength, and a byte-identical system prompt. Advisor framing adds exactly one plugin-sourced context/message at the head of the child's appended history, rather than changing its system prompt. Merge-back adds exactly one length-capped context/message with source plugin: sidechat; the next parent request and replay see it at the same position. Parent and child run concurrently without log or stream cross-talk.

## How to read the implementation

1. Start with [`packages/sdk/server/src/server.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/server.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/evidence`, `concern/ownership`, `concern/performance`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/proposed`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `parentSession`, `seedLength`, `ctx.agents.create`, `context/message`, `forkName`, `mergedInto`, `sidechat/*`, `tools/pre-execute`, `Interactive side sessions and merge-back`, `feature`, `boundary`, `evidence`, `ownership`, `performance`
- Regex: `(?i)(parentSession|seedLength|ctx\.agents\.create|context/message|forkName|mergedInto|sidechat/\*|tools/pre\-execute)`

```bash
rg -n --pcre2 "(?i)(parentSession|seedLength|ctx\\.agents\\.create|context/message|forkName|mergedInto|sidechat/\\*|tools/pre\\-execute)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0025. Every LLM request is reconstructable from the session log](0025-every-llm-request-is-reconstructable-from-the-session-log.md): The source note links to this decision directly.
- **`source-link`** — [0134. Subagent capability seam](0134-subagent-capability-seam.md): The source note links to this decision directly.
- **`source-link`** — [0140. Interception extension points --- the typed-Decision surface a hook programs against](0140-interception-extension-points-the-typed-decision-surface-a-hook-programs.md): The source note links to this decision directly.
- **`source-link`** — [0141. SessionStore fork API](0141-sessionstore-fork-api.md): The source note links to this decision directly.
- **`shares-code-with`** — [0599. TUI hidden mode folds a turn's assistant steps into one message](0599-tui-hidden-mode-folds-a-turn-s-assistant-steps-into-one-message.md): Shares source implementation: `packages/core/agent-loop/tests/resume.spec.ts`, `packages/core/session/tests/fork.spec.ts`.
- **`shares-code-with`** — [0447. Fold trace-only session facts into load-bearing events](0447-fold-trace-only-session-facts-into-load-bearing-events.md): Shares source implementation: `packages/core/session/tests/fork.spec.ts`, `packages/core/session/tests/session.spec.ts`.
- **`shares-code-with`** — [0635. Generated persistence log event catalog](0635-generated-persistence-log-event-catalog.md): Shares source implementation: `packages/core/session/tests/fork.spec.ts`, `packages/core/session/tests/session.spec.ts`.
- **`shares-code-with`** — [0370. The chat flow surfaces a max-tokens turn end](0370-the-chat-flow-surfaces-a-max-tokens-turn-end.md): Shares source implementation: `packages/core/session/tests/fork.spec.ts`, `packages/core/session/tests/session.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0519-interactive-side-sessions-and-merge-back.md`.
