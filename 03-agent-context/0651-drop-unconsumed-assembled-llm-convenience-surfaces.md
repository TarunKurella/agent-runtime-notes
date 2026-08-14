---
id: "dsh-note-0651"
title: "Drop unconsumed assembled LLM convenience surfaces"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-06-20-drop-unconsumed-llm-assembled-surfaces.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "stream"
  - "push"
  - "usage"
  - "finish"
  - "generate"
  - "BlockAssembler"
  - "ContentBlock"
  - "StreamChunk"
  - "LlmService"
  - "llm/stream"
  - "streamBlocks"
  - "GenerateResult"
  - "llm/generate"
  - "ctx.llm.stream"
search_regex: "(?i)(stream|push|usage|finish|generate|BlockAssembler|ContentBlock|StreamChunk)"
---

# 0651. Drop unconsumed assembled LLM convenience surfaces — implementation context

## Open this when

LlmService (packages/llm/llm/src/index.ts) exposes three call surfaces over a model: stream() --- raw StreamChunks, dispatched through the llm/stream waterfall. streamBlocks() --- a "convenience view" that runs the chunks through a BlockAssembler and yields completed ContentBlocks in stream order (index.ts:137-144). generate() --- one fully-assembled GenerateResult, dispatched through a second llm/generate waterfall (index.ts:151-157).

## Source decision

stream() is the sole public LLM call surface. Remove streamBlocks, generate, its event/result types, and assembler helpers used only by that path. Adapter tests assemble the public stream through a local helper, while BlockAssembler retains only the operations with production consumers.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-06-20-drop-unconsumed-llm-assembled-surfaces.md](../02-notes/archived/simplification/2026-06-20-drop-unconsumed-llm-assembled-surfaces.md)
- Pinned source: [.agents/notes/archived/simplification/2026-06-20-drop-unconsumed-llm-assembled-surfaces.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-06-20-drop-unconsumed-llm-assembled-surfaces.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | The source note names this file directly. Defines `stream`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/llm/llm/src/assembler.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/assembler.ts) | runtime implementation | The source note names this file directly. Defines `BlockAssembler`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `stream`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Defines `StreamChunk`, a construct named by the note. Defines `ContentBlock`, a construct named by the note. | `symbol-definition` |
| [`packages/e2b/fs-e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts) | package entry point | Defines `stream`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/fs-local/src/fsio.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts) | runtime implementation | Defines `stream`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/ts-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/ts-types.ts) | runtime implementation | Defines `finish`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Defines `finish`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent-loop/src/agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts) | runtime implementation | Defines `finish`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/json-schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts) | runtime implementation | Defines `finish`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/src/read-render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read-render.ts) | runtime implementation | Defines `finish`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `stream` | `const` | [`packages/acp/acp/src/index.ts:349`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L349) | `const stream: Stream = config.stream ?? ndJsonStream(` |
| `push` | `const` | [`packages/client/connection/src/client/fixture.ts:361`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L361) | `const push = (e: Record<string, unknown>): number => {` |
| `usage` | `const` | [`packages/client/connection/src/client/fixture.ts:836`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L836) | `const usage = item.type === 'assistant/chunk' && item.data.chunk?.type === 'usage'` |
| `usage` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryView.tsx:85`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryView.tsx#L85) | `const usage = value as UsageLike \| undefined` |
| `usage` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:678`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L678) | `const usage = node.usage as UsageLike \| undefined` |
| `finish` | `const` | [`packages/core/agent-loop/src/agent.ts:353`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L353) | `const finish = assembler.finish` |
| `finish` | `const` | [`packages/core/tools/src/json-schema.ts:503`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L503) | `const finish = (result: string[]): void => {` |
| `finish` | `const` | [`packages/core/tools/src/py-types.ts:513`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L513) | `const finish = (type: string): void => {` |
| `finish` | `const` | [`packages/core/tools/src/ts-types.ts:115`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/ts-types.ts#L115) | `const finish = (document: TypeDocument): void => {` |
| `stream` | `const` | [`packages/e2b/fs-e2b/src/index.ts:254`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L254) | `const stream = await openReadStream(sandbox, target, signal)` |
| `stream` | `const` | [`packages/e2b/fs-e2b/src/index.ts:298`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L298) | `const stream = await openReadStream(sandbox, target, signal)` |
| `stream` | `const` | [`packages/fs/fs-local/src/fsio.ts:407`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L407) | `const stream = createReadStream(target.targetKey, {` |
| `finish` | `function` | [`packages/fs/tool-fs/src/read-render.ts:95`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read-render.ts#L95) | `function finish(acc: WindowAccumulator, request: ReadWindow, displayPath: string): WindowResult {` |
| `generate` | `const` | [`packages/identity/anonymous-user-id/src/index.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/identity/anonymous-user-id/src/index.ts#L75) | `const generate = options.randomUUID ?? randomUUID` |
| `BlockAssembler` | `class` | [`packages/llm/llm/src/assembler.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/assembler.ts#L36) | `export class BlockAssembler {` |
| `stream` | `const` | [`packages/llm/llm/src/index.ts:865`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L865) | `const stream = adapter.stream(this.forAdapter(resolvedOptions, adapter))` |

### Tests and executable evidence

- [`packages/llm/llm/tests/assembler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/assembler.spec.ts) — A test under the owning area exercises or imports `BlockAssembler`.
- [`packages/llm/llm/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/properties.spec.ts) — A test under the owning area exercises or imports `BlockAssembler`.
- [`packages/core/session/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/scoped.spec.ts) — A test under the owning area exercises or imports `flushed`.
- [`packages/plan/plan-mode/tests/plan-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/tests/plan-mode.spec.ts) — A test under the owning area exercises or imports `flushed`.
- [`packages/host/apiproxy/tests/session-export.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/session-export.spec.ts) — A test under the owning area exercises or imports `flushed`.
- [`packages/client/runtime/tests/notifier.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/notifier.client.spec.ts) — A test under the owning area exercises or imports `flushed`.
- [`packages/terminal/terminal-bash/tests/sanitize.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal-bash/tests/sanitize.spec.ts) — A test under the owning area exercises or imports `flushed`.
- [`packages/subagent/subagent-acp/tests/mock-acp-server.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/tests/mock-acp-server.ts) — A test under the owning area exercises or imports `flushed`.
- Source verification intent: streamBlocks, generate, llm/generate, and the assembler helpers they alone required are gone with no new dead exports; both real adapters are exercised through stream() and the shared assembler; the loop behaves identically (ACP snapshot expected outputs unchanged); and the README, architecture doc, and module docs carry no mention of the removed surfaces.

## How to read the implementation

1. Start with [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/agent-loop`, `domain/build-release`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/archived`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `stream`, `push`, `usage`, `finish`, `generate`, `BlockAssembler`, `ContentBlock`, `StreamChunk`, `LlmService`, `llm/stream`, `streamBlocks`, `GenerateResult`, `llm/generate`, `ctx.llm.stream`
- Regex: `(?i)(stream|push|usage|finish|generate|BlockAssembler|ContentBlock|StreamChunk)`

```bash
rg -n --pcre2 "(?i)(stream|push|usage|finish|generate|BlockAssembler|ContentBlock|StreamChunk)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0078. Terminal LLM stream failures](0078-terminal-llm-stream-failures.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/core/tools/src/py-types.ts`.
- **`shares-code-with`** — [0364. Owned-run finish reason reporting](0364-owned-run-finish-reason-reporting.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/e2b/fs-e2b/src/index.ts`.
- **`shares-code-with`** — [0145. Explicit model-facing tool order](0145-explicit-model-facing-tool-order.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0025. Every LLM request is reconstructable from the session log](0025-every-llm-request-is-reconstructable-from-the-session-log.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0571. Auto-titled terminal from the first message](0571-auto-titled-terminal-from-the-first-message.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0302. Render error cause chains at every diagnostic boundary](0302-render-error-cause-chains-at-every-diagnostic-boundary.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0228. Code Mode language dispatch and the Python SDK renderer](0228-code-mode-language-dispatch-and-the-python-sdk-renderer.md): Shares source implementation: `AGENTS.md`, `packages/core/tools/src/py-types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0651-drop-unconsumed-assembled-llm-convenience-surfaces.md`.
