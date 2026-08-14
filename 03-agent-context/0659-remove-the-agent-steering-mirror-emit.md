---
id: "dsh-note-0659"
title: "Remove the `agent/steering` mirror emit"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-07-04-remove-agent-steering-mirror.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/simplification"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/streaming"
aliases:
  - "ctx"
  - "source"
  - "agent/steering"
  - "steering/message { turn, content, source }"
  - "packages/core/agent-loop/src/loop.ts"
  - "drainSteering"
  - "steering/message"
  - "agent/queued"
  - "inbox.steer"
  - "packages/core/agent/src/types.ts"
  - "packages/core/agent/README.md"
  - "session/event"
  - "Remove the `agent/steering` mirror emit"
  - "simplification"
search_regex: "(?i)(source|agent/steering|steering/message[- ]\\{[- ]turn,[- ]content,[- ]source[- ]\\}|packages/core/agent\\-loop/src/loop\\.ts|drainSteering|steering/message|agent/queued|inbox\\.steer)"
---

# 0659. Remove the `agent/steering` mirror emit — implementation context

## Open this when

agent/steering was the last remaining transient mirror of a durable session event. The loop's steering drain appends the durable steering/message { turn, content, source } and, on the very next line, emitted agent/steering(agent, turn, content, source) --- the identical fact as a fire-and-forget event (packages/core/agent-loop/src/loop.ts, drainSteering). It had zero production listeners: the only subscriber anywhere was a loop regression test asserting the emit carried source --- the same fact the durable event already records one line above.

## Source decision

agent/steering is removed from the agent event taxonomy: the declaration in packages/core/agent/src/types.ts (and its mention in the live-events JSDoc list there), the emit in drainSteering (whose then-unused ctx parameter went with it), the row in packages/core/agent/README.md, and the emit line in the loop-pseudocode blocks (the packages/core/agent-loop/src/loop.ts module doc and architecture.md); the cordis catalog is regenerated without it. The one regression test pins source preservation on the durable steering/message event --- the fact it pins lives on the log.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-07-04-remove-agent-steering-mirror.md](../02-notes/archived/simplification/2026-07-04-remove-agent-steering-mirror.md)
- Pinned source: [.agents/notes/archived/simplification/2026-07-04-remove-agent-steering-mirror.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-07-04-remove-agent-steering-mirror.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `session/event` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/core/agent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | The source note names this file directly. | `named-file` |
| [`vendor/hmr/src/error.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/error.ts) | runtime implementation | Defines `source`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/src/profile-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts) | runtime implementation | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `source`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `source`, a construct named by the note. | `symbol-definition` |
| [`vendor/loader/src/config/tree.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/tree.ts) | runtime implementation | Defines `source`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`packages/client/web/src/boot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx) | runtime implementation | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`packages/api/gateway/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts) | package entry point | Defines `source`, a construct named by the note. | `symbol-definition` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Defines `ctx`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ctx` | `const` | [`apps/cli/src/profile-boot.ts:248`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L248) | `const ctx = await boot(NAME, rootConfig, structuredClone(allPatches(composed)), (hostCtx) => {` |
| `source` | `const` | [`packages/api/gateway/src/index.ts:562`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts#L562) | `const source = Function.prototype.toString.call(implementation)` |
| `ctx` | `const` | [`packages/boot/app-boot/src/index.ts:764`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L764) | `const ctx = new Context()` |
| `ctx` | `const` | [`packages/client/web/src/boot.tsx:162`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L162) | `const ctx = this.ctx` |
| `ctx` | `const` | [`packages/client/web/src/boot.tsx:217`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L217) | `const ctx = this.ctx` |
| `ctx` | `const` | [`packages/core/tools/src/index.ts:947`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L947) | `const ctx = this.ctx` |
| `source` | `const` | [`packages/goal/goal/src/fold.ts:322`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L322) | `const source = goalSource(event.data.source)` |
| `source` | `const` | [`packages/llm/llm/src/index.ts:825`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L825) | `const source = message.source` |
| `source` | `const` | [`vendor/hmr/src/error.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/error.ts#L24) | `const source = readFileSync(file, 'utf8')` |
| `source` | `const` | [`vendor/loader/src/config/tree.ts:116`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/tree.ts#L116) | `const source = entry.parent` |

### Tests and executable evidence

- [`apps/web/tests/steering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/steering.e2e.ts) — A test under the owning area exercises or imports `steer`. A test under the owning area exercises or imports `steering`.
- [`apps/web/tests/settings-chrome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/settings-chrome.e2e.ts) — A test under the owning area exercises or imports `steer`.
- [`packages/core/agent/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/agent.spec.ts) — A test under the owning area exercises or imports `steer`.
- [`packages/e2b/e2b/tests/composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/tests/composition.e2e.ts) — A test under the owning area exercises or imports `steer`.
- [`packages/jobs/jobs-local/tests/jobs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs-local/tests/jobs.spec.ts) — A test under the owning area exercises or imports `steer`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `steer`. A test under the owning area exercises or imports `steering`.
- [`packages/core/agent-loop/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/agent.spec.ts) — A test under the owning area exercises or imports `steer`.
- [`apps/cli/tests/fixtures/dsh-badge/snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/dsh-badge/snapshot.ts) — A test under the owning area exercises or imports `steer`.
- Source verification intent: The agent/steering spelling survives only in Agent Note prose (this Agent Note, the three amended Agent Notes above, and the frozen rejected steering-capability Agent Note, whose text records the proposal it declined); the catalog is regenerated; the retargeted test pins source preservation on steering/message.

## How to read the implementation

1. Start with [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) because it has the strongest evidence link to the note.
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
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/evidence`, `concern/human-control`, `concern/simplification`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/archived`, `mechanism/streaming`
- Aliases: `ctx`, `source`, `agent/steering`, `steering/message { turn, content, source }`, `packages/core/agent-loop/src/loop.ts`, `drainSteering`, `steering/message`, `agent/queued`, `inbox.steer`, `packages/core/agent/src/types.ts`, `packages/core/agent/README.md`, `session/event`, `Remove the `agent/steering` mirror emit`, `simplification`
- Regex: `(?i)(source|agent/steering|steering/message[- ]\{[- ]turn,[- ]content,[- ]source[- ]\}|packages/core/agent\-loop/src/loop\.ts|drainSteering|steering/message|agent/queued|inbox\.steer)`

```bash
rg -n --pcre2 "(?i)(source|agent/steering|steering/message[- ]\\{[- ]turn,[- ]content,[- ]source[- ]\\}|packages/core/agent\\-loop/src/loop\\.ts|drainSteering|steering/message|agent/queued|inbox\\.steer)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0685. AGENTS.md --- Archived Agent Notes](0685-agents-md-archived-agent-notes.md): The source note links to this decision directly.
- **`source-link`** — [0653. Stop mirroring the token stream as an agent event](0653-stop-mirroring-the-token-stream-as-an-agent-event.md): The source note links to this decision directly.
- **`shares-code-with`** — [0554. dsh-tui chat channel module split](0554-dsh-tui-chat-channel-module-split.md): Shares source implementation: `apps/cli/src/profile-boot.ts`, `packages/client/web/src/boot.tsx`.
- **`shares-code-with`** — [0200. Continuable subagents](0200-continuable-subagents.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0606. Web context injection disclosure](0606-web-context-injection-disclosure.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0507. Runtime schemas for the event vocabulary (Zod vs the merge-extensible-map pattern)](0507-runtime-schemas-for-the-event-vocabulary-zod-vs-the-merge-extensible-map.md): Shares source implementation: `docs/architecture.md`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0196. Web session fork actions](0196-web-session-fork-actions.md): Shares source implementation: `apps/cli/src/profile-boot.ts`, `packages/client/web/src/boot.tsx`.
- **`shares-code-with`** — [0449. Stop mirroring durable boundaries as agent events](0449-stop-mirroring-durable-boundaries-as-agent-events.md): Shares source implementation: `docs/architecture.md`, `packages/core/agent/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0659-remove-the-agent-steering-mirror-emit.md`.
