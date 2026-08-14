---
id: "dsh-note-0001"
title: "Provider-neutral content-block vocabulary owned by dsh-llm"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-06-11-content-block-vocabulary.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/performance"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/context"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "SessionId"
  - "TurnEndReason"
  - "BlockAssembler"
  - "CallId"
  - "MessageSource"
  - "ContentBlockMap"
  - "FinishReason"
  - "tool-call"
  - "tool-result"
  - "TurnTrigger"
  - "context/message"
  - "Provider-neutral content-block vocabulary owned by dsh-llm"
  - "architecture"
  - "boundary"
search_regex: "(?i)(SessionId|TurnEndReason|BlockAssembler|CallId|MessageSource|ContentBlockMap|FinishReason|tool\\-call)"
---

# 0001. Provider-neutral content-block vocabulary owned by dsh-llm — implementation context

## Open this when

The harness needs one internal language for messages that the loop, session log, and all plugins speak.

## Source decision

Own the vocabulary: messages are arrays of typed content blocks (text, reasoning, tool-call, tool-result), with the union derived from the merge-extensible ContentBlockMap so plugins add block types via declaration merging. The same merge-extensible-map pattern types every "stringly" field (MessageSource, FinishReason, TurnTrigger, TurnEndReason). Streaming is a raw chunk protocol; BlockAssembler is the single shared assembly implementation. Adapters translate to provider wire formats --- mapping cost lives in adapters, where it belongs.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-06-11-content-block-vocabulary.md](../02-notes/implemented/architecture/2026-06-11-content-block-vocabulary.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-06-11-content-block-vocabulary.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-06-11-content-block-vocabulary.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. Defines `ContentBlockMap`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/brand.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm`. Defines `CallId`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/message.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/message.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm`. Defines `MessageSource`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/assembler.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/assembler.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm`. Defines `BlockAssembler`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Defines `TurnEndReason`, a construct named by the note. Defines `SessionId`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/README.md) | package contract and examples | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/package.json) | composition and configuration | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `SessionId` | `type` | [`packages/core/session/src/types.ts:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L22) | `export type SessionId = Branded<'SessionId'>` |
| `TurnEndReason` | `type` | [`packages/core/session/src/types.ts:177`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L177) | `export type TurnEndReason = TurnEndReasonMap[keyof TurnEndReasonMap]` |
| `BlockAssembler` | `class` | [`packages/llm/llm/src/assembler.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/assembler.ts#L36) | `export class BlockAssembler {` |
| `CallId` | `type` | [`packages/llm/llm/src/brand.ts:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts#L31) | `export type CallId = Branded<'CallId'>` |
| `CallId` | `function` | [`packages/llm/llm/src/brand.ts:38`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts#L38) | `export function CallId(id: string): CallId {` |
| `MessageSource` | `type` | [`packages/llm/llm/src/message.ts:126`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/message.ts#L126) | `export type MessageSource = MessageSourceMap[keyof MessageSourceMap]` |
| `ContentBlockMap` | `interface` | [`packages/llm/llm/src/types.ts:99`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L99) | `export interface ContentBlockMap {` |
| `FinishReason` | `type` | [`packages/llm/llm/src/types.ts:125`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L125) | `export type FinishReason = FinishReasonMap[keyof FinishReasonMap]` |

### Tests and executable evidence

- [`packages/core/session/tests/fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/fork.spec.ts) — A test under the owning area exercises or imports `TurnEndReason`.
- [`packages/llm/llm/tests/assembler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/assembler.spec.ts) — A test under the owning area exercises or imports `BlockAssembler`.
- [`packages/llm/llm/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/properties.spec.ts) — A test under the owning area exercises or imports `BlockAssembler`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `TurnEndReason`.
- [`packages/session-query/tool-session-query/tests/tool-session-query.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/tests/tool-session-query.spec.ts) — Contains the exact code literal `context/message` named by the note.

## How to read the implementation

1. Start with [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/performance`, `concern/schema-types`, `concern/simplification`, `domain/context`, `domain/llm`, `domain/protocols`, `domain/session-state`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `SessionId`, `TurnEndReason`, `BlockAssembler`, `CallId`, `MessageSource`, `ContentBlockMap`, `FinishReason`, `tool-call`, `tool-result`, `TurnTrigger`, `context/message`, `Provider-neutral content-block vocabulary owned by dsh-llm`, `architecture`, `boundary`
- Regex: `(?i)(SessionId|TurnEndReason|BlockAssembler|CallId|MessageSource|ContentBlockMap|FinishReason|tool\-call)`

```bash
rg -n --pcre2 "(?i)(SessionId|TurnEndReason|BlockAssembler|CallId|MessageSource|ContentBlockMap|FinishReason|tool\\-call)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0654. Drop `GenerateOptions.prefill` and `ToolSchema.strict` --- request knobs with no working end-to-end path](0654-drop-generateoptions-prefill-and-toolschema-strict-request-knobs-with-no.md): The source note links to this decision directly.
- **`source-link`** — [0657. Prune producer-less vocabulary variants (block cache hints, the `agent` message source, the `continuation` turn trigger)](0657-prune-producer-less-vocabulary-variants-block-cache-hints-the-agent-mess.md): The source note links to this decision directly.
- **`source-link`** — [0452. Drop the `image` content block until a path can honor it](0452-drop-the-image-content-block-until-a-path-can-honor-it.md): The source note links to this decision directly.
- **`source-link`** — [0457. Project injected content verbatim, dropping the XML envelopes](0457-project-injected-content-verbatim-dropping-the-xml-envelopes.md): The source note links to this decision directly.
- **`shares-code-with`** — [0056. Adapter-owned reasoning effort capabilities](0056-adapter-owned-reasoning-effort-capabilities.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0623. TUI model-context resolution defers on the adapter-registration race](0623-tui-model-context-resolution-defers-on-the-adapter-registration-race.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0650. Drop the unconsumed `llm/adapter-change` event](0650-drop-the-unconsumed-llm-adapter-change-event.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0567. Dedicated full-screen TUI front door](0567-dedicated-full-screen-tui-front-door.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/llm/llm`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0001-provider-neutral-content-block-vocabulary-owned-by-dsh-llm.md`.
