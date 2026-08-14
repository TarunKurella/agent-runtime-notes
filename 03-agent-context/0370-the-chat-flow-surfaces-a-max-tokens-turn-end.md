---
id: "dsh-note-0370"
title: "The chat flow surfaces a max-tokens turn end"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-12-max-tokens-turn-end-notice.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/agent-loop"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
aliases:
  - "node"
  - "max-tokens"
  - "turn/end"
  - "reason.kind === 'error"
  - "turn-max-tokens"
  - "reason.kind === 'max-tokens"
  - "conversation.chat.node"
  - "turn-error"
  - "The chat flow surfaces a max-tokens turn end"
  - "bug fix"
  - "boundary"
  - "compatibility"
  - "evidence"
  - "ownership"
search_regex: "(?i)(node|max\\-tokens|turn/end|reason\\.kind[- ]===[- ]'error|turn\\-max\\-tokens|reason\\.kind[- ]===[- ]'max\\-tokens|conversation\\.chat\\.node|turn\\-error)"
---

# 0370. The chat flow surfaces a max-tokens turn end — implementation context

## Open this when

The agent loop records max-tokens as its own turn/end reason, but no user surface consumed it. In the Web chat flow only reason.kind === 'error' built a conversation node, and the unknown-surface fallback claims append-surface events only, so a turn the provider cut at its output cap ended with no visible sign: the truncated answer read as a normal completion, and the user had no way to tell why the run stopped (issue #1522).

## Source decision

A turn-max-tokens conversation node Definition matches turn/end with reason.kind === 'max-tokens' and materializes a persistent chat row at the turn position: a warning StateDot, a localized title, and guidance that the truncated output is preserved and sending "continue" resumes in a new turn. The node derives from the durable session event alone, so refresh, restore, and history replay rebuild it identically. It shows no token numbers: the event carries none, and the notice must not fabricate budget data the provider did not report.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-12-max-tokens-turn-end-notice.md](../02-notes/implemented/bug-fix/2026-08-12-max-tokens-turn-end-notice.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-12-max-tokens-turn-end-notice.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-12-max-tokens-turn-end-notice.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/tools/src/schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts) | runtime implementation | Defines `node`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/ts-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/ts-types.ts) | runtime implementation | Defines `node`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Defines `node`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/call-config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/call-config.ts) | runtime implementation | Defines `node`, a construct named by the note. | `symbol-definition` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.zh.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/persistence-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/persistence-catalog.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |
| [`docs/persistence-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/persistence-catalog.zh.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `node` | `const` | [`packages/core/tools/src/py-types.ts:558`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L558) | `const node = frame.node` |
| `node` | `const` | [`packages/core/tools/src/py-types.ts:597`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L597) | `const node = frame.schema` |
| `node` | `const` | [`packages/core/tools/src/schema.ts:336`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L336) | `const node: JsonSchemaNode = {}` |
| `node` | `const` | [`packages/core/tools/src/ts-types.ts:175`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/ts-types.ts#L175) | `const node = frame.node` |
| `node` | `const` | [`packages/llm/llm/src/call-config.ts:102`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/call-config.ts#L102) | `const node = task.node` |

### Tests and executable evidence

- [`python/sdk/tests/test_client.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_client.py) — A test under the owning area exercises or imports `max-tokens`.
- [`packages/core/session/tests/fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/fork.spec.ts) — A test under the owning area exercises or imports `max-tokens`.
- [`packages/jobs/jobs-local/tests/jobs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs-local/tests/jobs.spec.ts) — A test under the owning area exercises or imports `max-tokens`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `max-tokens`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `max-tokens`.
- [`apps/web/tests/max-tokens-notice.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/max-tokens-notice.snapshot.ts) — A test under the owning area exercises or imports `max-tokens`. A test under the owning area exercises or imports `turn-error`.
- [`packages/core/agent-loop/tests/mock-adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/mock-adapter.ts) — A test under the owning area exercises or imports `max-tokens`.
- [`packages/core/agent/tests/consumed-work.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/consumed-work.spec.ts) — A test under the owning area exercises or imports `max-tokens`.

## How to read the implementation

1. Start with [`packages/core/tools/src/schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/agent-loop`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`
- Aliases: `node`, `max-tokens`, `turn/end`, `reason.kind === 'error`, `turn-max-tokens`, `reason.kind === 'max-tokens`, `conversation.chat.node`, `turn-error`, `The chat flow surfaces a max-tokens turn end`, `bug fix`, `boundary`, `compatibility`, `evidence`, `ownership`
- Regex: `(?i)(node|max\-tokens|turn/end|reason\.kind[- ]===[- ]'error|turn\-max\-tokens|reason\.kind[- ]===[- ]'max\-tokens|conversation\.chat\.node|turn\-error)`

```bash
rg -n --pcre2 "(?i)(node|max\\-tokens|turn/end|reason\\.kind[- ]===[- ]'error|turn\\-max\\-tokens|reason\\.kind[- ]===[- ]'max\\-tokens|conversation\\.chat\\.node|turn\\-error)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/core/tools/src/schema.ts`.
- **`shares-code-with`** — [0599. TUI hidden mode folds a turn's assistant steps into one message](0599-tui-hidden-mode-folds-a-turn-s-assistant-steps-into-one-message.md): Shares source implementation: `packages/core/agent-loop/tests/loop.spec.ts`, `packages/core/session/tests/fork.spec.ts`.
- **`shares-code-with`** — [0651. Drop unconsumed assembled LLM convenience surfaces](0651-drop-unconsumed-assembled-llm-convenience-surfaces.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/core/tools/src/ts-types.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/core/tools/src/schema.ts`.
- **`shares-code-with`** — [0447. Fold trace-only session facts into load-bearing events](0447-fold-trace-only-session-facts-into-load-bearing-events.md): Shares source implementation: `packages/core/session/tests/fork.spec.ts`, `packages/core/session/tests/session.spec.ts`.
- **`shares-code-with`** — [0142. Dynamic workflows --- a script-driven multi-agent orchestration seam](0142-dynamic-workflows-a-script-driven-multi-agent-orchestration-seam.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/core/tools/src/schema.ts`.
- **`shares-code-with`** — [0228. Code Mode language dispatch and the Python SDK renderer](0228-code-mode-language-dispatch-and-the-python-sdk-renderer.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/core/tools/src/ts-types.ts`.
- **`shares-code-with`** — [0543. Custom typed tool-schema DSL instead of schemastery](0543-custom-typed-tool-schema-dsl-instead-of-schemastery.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/core/tools/src/schema.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0370-the-chat-flow-surfaces-a-max-tokens-turn-end.md`.
