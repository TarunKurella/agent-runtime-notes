---
id: "dsh-note-0335"
title: "Goal-round wrap-up message"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-02-goal-round-wrapup-message.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/agent-loop"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/streaming"
aliases:
  - "blocked"
  - "concludesTurn"
  - "complete"
  - "completed"
  - "summary"
  - "update_goal"
  - "concludeTurn"
  - "{ kind: 'plugin', plugin: 'tool-goal' }"
  - "<goal_complete>"
  - "<goal_blocked>"
  - "deepseek-v4-pro"
  - "dsh-llm-replay"
  - "{{fromRequest:<regex>}}"
  - "tool-goal"
search_regex: "(?i)(blocked|concludesTurn|complete|completed|summary|update_goal|concludeTurn|\\{[- ]kind:[- ]'plugin',[- ]plugin:[- ]'tool\\-goal'[- ]\\})"
---

# 0335. Goal-round wrap-up message — implementation context

## Open this when

An autonomous goal round that reported update_goal complete or blocked concluded the physical turn at the tool result, so the model never spoke after the call. Sessions ended on a bare update_goal card, and internal testers read that as the agent stopping mid-sentence: the model's pre-call text routinely announces a report ("goal achieved, marking complete:") that never arrives, because the standard tool-use expectation is one more assistant message after a tool result and neither the goal-round prompt nor the tool description said the call was terminal.

## Source decision

A goal-round complete or blocked success no longer calls concludeTurn(). Instead the tool defers one wrap-up context onto its own result: a { kind: 'plugin', plugin: 'tool-goal' }-sourced user message carrying a / instruction to write a grounded closing message to the user and call no more tools. The turn then ends through the agent loop's ordinary no-tool-calls stop, so no new loop primitive exists and steering semantics are untouched. Direct-human mutations remain uninstructed exactly as before. The cost is one additional model request per goal lifecycle, not per round.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-02-goal-round-wrapup-message.md](../02-notes/implemented/bug-fix/2026-08-02-goal-round-wrapup-message.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-02-goal-round-wrapup-message.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-02-goal-round-wrapup-message.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/goal/tool-goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/src/index.ts) | package entry point | Core file in the package named by the note: `packages/goal/tool-goal`. | `named-package-member` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/goal/tool-goal/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/goal/tool-goal`. | `named-package-member` |
| [`packages/core/agent-loop/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/test-support/llm-replay/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`packages/test-support/llm-replay/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`packages/goal/tool-goal`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/agent-loop`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/test-support/llm-replay`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `completed`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `concludesTurn`, a construct named by the note. | `symbol-definition` |
| [`packages/jobs/tool-jobs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts) | package entry point | Defines `complete`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `blocked` | `const` | [`packages/client/ui-settings-plugins/src/client/PluginCard.tsx:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/client/PluginCard.tsx#L52) | `const blocked = !state.dirty \|\| state.invalid \|\| state.saving` |
| `concludesTurn` | `const` | [`packages/core/tools/src/index.ts:1815`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1815) | `const concludesTurn = this.concludingExecutions.has(exec)` |
| `complete` | `const` | [`packages/jobs/tool-jobs/src/index.ts:129`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L129) | `const complete = \`${content}${suffix}\`` |
| `completed` | `let` | [`packages/llm/llm/src/index.ts:872`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L872) | `let completed = false` |
| `summary` | `const` | [`packages/skill/tool-skill/src/index.ts:134`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts#L134) | `const summary = (await ctx.skills.list(lookup)).find(skill => skill.name === args.name)` |

### Tests and executable evidence

- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `concludesTurn`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `concludeTurn`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `concludesTurn`.
- [`packages/core/agent-loop/tests/cancel.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/cancel.spec.ts) — A test under the owning area exercises or imports `blocked`.
- [`packages/goal/tool-goal/tests/tool-goal.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/tests/tool-goal.spec.ts) — A test under the owning area exercises or imports `update_goal`. A test under the owning area exercises or imports `tool-goal`.
- [`packages/core/agent-loop/tests/tool-calls.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/tool-calls.spec.ts) — A test under the owning area exercises or imports `blocked`.
- [`packages/core/agent-loop/tests/interception.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/interception.spec.ts) — A test under the owning area exercises or imports `blocked`.
- [`packages/core/agent-loop/tests/coverage-edges.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/coverage-edges.spec.ts) — A test under the owning area exercises or imports `blocked`.
- Source verification intent: tool-goal package tests pin the injected context (source, tag, objective, no-more-tools clause) and the absent concludesTurn for both terminal actions, plus the uninstructed direct-human pause and complete paths, at 100% file coverage. llm-replay unit tests pin the placeholder contract: last-match-wins capture, whole-match fallback, and loud failures for unmatched, invalid, and unterminated patterns.

## How to read the implementation

1. Start with [`packages/goal/tool-goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/agent-loop`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/streaming`
- Aliases: `blocked`, `concludesTurn`, `complete`, `completed`, `summary`, `update_goal`, `concludeTurn`, `{ kind: 'plugin', plugin: 'tool-goal' }`, `<goal_complete>`, `<goal_blocked>`, `deepseek-v4-pro`, `dsh-llm-replay`, `{{fromRequest:<regex>}}`, `tool-goal`
- Regex: `(?i)(blocked|concludesTurn|complete|completed|summary|update_goal|concludeTurn|\{[- ]kind:[- ]'plugin',[- ]plugin:[- ]'tool\-goal'[- ]\})`

```bash
rg -n --pcre2 "(?i)(blocked|concludesTurn|complete|completed|summary|update_goal|concludeTurn|\\{[- ]kind:[- ]'plugin',[- ]plugin:[- ]'tool\\-goal'[- ]\\})" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0161. Model-facing same-session goal tools](0161-model-facing-same-session-goal-tools.md): The source note links to this decision directly.
- **`shares-code-with`** — [0571. Auto-titled terminal from the first message](0571-auto-titled-terminal-from-the-first-message.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/test-support/llm-replay`.
- **`shares-code-with`** — [0157. Harness-level goal-based execution](0157-harness-level-goal-based-execution.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/goal/tool-goal/src/index.ts`.
- **`shares-code-with`** — [0683. Snapshot semantic terminal state for the TUI](0683-snapshot-semantic-terminal-state-for-the-tui.md): Shares source implementation: `packages/test-support/llm-replay`, `packages/test-support/llm-replay/src/index.ts`.
- **`shares-code-with`** — [0533. Persist assembled assistant messages, not stream chunks](0533-persist-assembled-assistant-messages-not-stream-chunks.md): Shares source implementation: `packages/test-support/llm-replay`, `packages/test-support/llm-replay/src/index.ts`.
- **`shares-code-with`** — [0305. Semantic session checkpoints](0305-semantic-session-checkpoints.md): Shares source implementation: `packages/core/agent-loop`, `packages/core/agent-loop/src/index.ts`.
- **`shares-code-with`** — [0163. Plugin-owned human command registration](0163-plugin-owned-human-command-registration.md): Shares source implementation: `packages/core/agent-loop`, `packages/core/agent-loop/src/index.ts`.
- **`shares-code-with`** — [0562. The session prefix --- request-only messages in front of the derived history](0562-the-session-prefix-request-only-messages-in-front-of-the-derived-history.md): Shares source implementation: `packages/core/agent-loop`, `packages/core/agent-loop/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0335-goal-round-wrap-up-message.md`.
