---
id: "dsh-note-0671"
title: "Replace the hand-rolled SSE parser in llm-deepseek with eventsource-parser"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-07-26-eventsource-parser-for-deepseek-sse.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/evidence"
  - "domain/llm"
  - "domain/protocols"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/adapter"
  - "mechanism/streaming"
aliases:
  - "LlmError"
  - "packages/llm/llm-deepseek/src/sse.ts"
  - "TextDecoder"
  - "[DONE]"
  - "STREAM_CLOSED"
  - "tests/sse.spec.ts"
  - "adapter.ts"
  - "yield* translate(parseSse(response.body))"
  - "eventsource-parser"
  - "@modelcontextprotocol/sdk"
  - "sse.ts"
  - "EventSourceParserStream"
  - "eventsource-parser/stream"
  - "parseSse"
search_regex: "(?i)(LlmError|packages/llm/llm\\-deepseek/src/sse\\.ts|TextDecoder|\\[DONE\\]|STREAM_CLOSED|tests/sse\\.spec\\.ts|adapter\\.ts|yield\\*[- ]translate\\(parseSse\\(response\\.body\\)\\))"
---

# 0671. Replace the hand-rolled SSE parser in llm-deepseek with eventsource-parser — implementation context

## Open this when

packages/llm/llm-deepseek/src/sse.ts hand-implemented Server-Sent Events parsing: a streaming TextDecoder, event-block splitting on \r?\n\r?\n, data: payload extraction and joining, comment/field skipping, the [DONE] sentinel, a STREAM_CLOSED error on EOF without it, and a flush of a final unterminated event block. The file was ~67 lines with ~108 lines of dedicated tests (tests/sse.spec.ts) re-proving SSE spec behavior --- UTF-8 split across chunks, CRLF handling, multi-data: joining, no-space-after-colon --- that a maintained parser already guarantees.

## Source decision

sse.ts delegates SSE framing to EventSourceParserStream from eventsource-parser/stream: parseSse pipes the response body through new TextDecoderStream() then new EventSourceParserStream() and keeps only the DeepSeek protocol shim --- yield each event's data, terminate on [DONE], and throw LlmError('STREAM_CLOSED') when the stream ends without the sentinel. All required builtins (TextDecoderStream, pipeThrough, async-iterable ReadableStream) exist at the Node ^22.19 engine floor. The spec-conformance tests are gone; tests/sse.spec.ts pins only the [DONE]/STREAM_CLOSED/EOF contract.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-07-26-eventsource-parser-for-deepseek-sse.md](../02-notes/archived/simplification/2026-07-26-eventsource-parser-for-deepseek-sse.md)
- Pinned source: [.agents/notes/archived/simplification/2026-07-26-eventsource-parser-for-deepseek-sse.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-07-26-eventsource-parser-for-deepseek-sse.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm-deepseek/src/sse.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/sse.ts) | runtime implementation | The source note names this file directly. Contains the exact code literal `eventsource-parser/stream` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. Defines `LlmError`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/test-support/llm-mock-server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-mock-server/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/llm-mock-server`. | `named-package-member` |
| [`packages/test-support/llm-mock-server/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-mock-server/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/test-support/llm-mock-server`. | `named-package-member` |
| [`packages/llm/llm`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/llm-deepseek`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/test-support/llm-mock-server`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-mock-server) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `LlmError` | `class` | [`packages/llm/llm/src/index.ts:83`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L83) | `export class LlmError extends HarnessError {` |

### Tests and executable evidence

- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `LlmError`.
- [`packages/llm/llm/tests/topology.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/topology.spec.ts) — A test under the owning area exercises or imports `LlmError`.
- [`packages/llm/llm-deepseek/tests/sse.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/tests/sse.spec.ts) — A test under the owning area exercises or imports `STREAM_CLOSED`. A test under the owning area exercises or imports `eventsource-parser`.
- [`packages/llm/llm-deepseek/tests/adapter.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/tests/adapter.spec.ts) — A test under the owning area exercises or imports `parseSse`. A test under the owning area exercises or imports `ReadableStream`.
- [`packages/llm/llm-deepseek/tests/translate.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/tests/translate.spec.ts) — A test under the owning area exercises or imports `STREAM_CLOSED`. A test under the owning area exercises or imports `LlmError`.
- [`packages/test-support/llm-mock-server/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-mock-server/tests/invariant.spec.ts) — A test under the owning area exercises or imports `dsh-llm-mock-server`.
- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — Contains the exact code literal `dsh-llm-mock-server` named by the note.
- [`packages/llm/llm-retry/tests/transport-recovery.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/tests/transport-recovery.spec.ts) — Contains the exact code literal `dsh-llm-mock-server` named by the note.

## How to read the implementation

1. Start with [`packages/llm/llm-deepseek/src/sse.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/sse.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/evidence`, `domain/llm`, `domain/protocols`, `domain/testing`, `lifecycle/archived`, `mechanism/adapter`, `mechanism/streaming`
- Aliases: `LlmError`, `packages/llm/llm-deepseek/src/sse.ts`, `TextDecoder`, `[DONE]`, `STREAM_CLOSED`, `tests/sse.spec.ts`, `adapter.ts`, `yield* translate(parseSse(response.body))`, `eventsource-parser`, `@modelcontextprotocol/sdk`, `sse.ts`, `EventSourceParserStream`, `eventsource-parser/stream`, `parseSse`
- Regex: `(?i)(LlmError|packages/llm/llm\-deepseek/src/sse\.ts|TextDecoder|\[DONE\]|STREAM_CLOSED|tests/sse\.spec\.ts|adapter\.ts|yield\*[- ]translate\(parseSse\(response\.body\)\))`

```bash
rg -n --pcre2 "(?i)(LlmError|packages/llm/llm\\-deepseek/src/sse\\.ts|TextDecoder|\\[DONE\\]|STREAM_CLOSED|tests/sse\\.spec\\.ts|adapter\\.ts|yield\\*[- ]translate\\(parseSse\\(response\\.body\\)\\))" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/types.ts`.
- **`shares-code-with`** — [0008. Two LLM adapters as a design-verification twin](0008-two-llm-adapters-as-a-design-verification-twin.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/types.ts`.
- **`shares-code-with`** — [0017. Mandatory `User-Agent` attribution for provider requests](0017-mandatory-user-agent-attribution-for-provider-requests.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/types.ts`.
- **`shares-code-with`** — [0650. Drop the unconsumed `llm/adapter-change` event](0650-drop-the-unconsumed-llm-adapter-change-event.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0567. Dedicated full-screen TUI front door](0567-dedicated-full-screen-tui-front-door.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0501. Scriptable LLM wire fault server](0501-scriptable-llm-wire-fault-server.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/invariant.ts`.
- **`shares-code-with`** — [0181. Per-provider request retry policies](0181-per-provider-request-retry-policies.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0571. Auto-titled terminal from the first message](0571-auto-titled-terminal-from-the-first-message.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0671-replace-the-hand-rolled-sse-parser-in-llm-deepseek-with-eventsource-pars.md`.
