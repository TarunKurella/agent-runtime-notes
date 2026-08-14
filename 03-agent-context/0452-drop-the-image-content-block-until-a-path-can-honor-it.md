---
id: "dsh-note-0452"
title: "Drop the `image` content block until a path can honor it"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-07-04-drop-image-content-block.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
aliases:
  - "image"
  - "ImageBlock"
  - "ContentBlockMap"
  - "packages/llm/llm/src/types.ts"
  - "Drop the `image` content block until a path can honor it"
  - "simplification"
  - "boundary"
  - "evidence"
  - "recovery"
  - "build release"
  - "context"
  - "extensions"
  - "llm"
  - "protocols"
search_regex: "(?i)(image|ImageBlock|ContentBlockMap|packages/llm/llm/src/types\\.ts|simplification|boundary|evidence|recovery)"
---

# 0452. Drop the `image` content block until a path can honor it — implementation context

## Open this when

ImageBlock (packages/llm/llm/src/types.ts) had no production producer, and every consumer on every path DROPPED it: the DeepSeek adapter's serializer skipped image blocks (a documented MVP limitation), the pi-ai converter skipped them as unrepresentable, and the compaction estimator charged a flat token constant and rendered [image]. ACP independently rejected image prompt content. An ImageBlock constructed then would silently vanish from the provider wire --- the vocabulary advertised a capability no path honored, which is the silent-data-loss shape AGENTS.md's defensive patterns warn against.

## Source decision

Remove ImageBlock, its map entry, and image-specific branches from adapters and compaction. Update the owning vocabulary docs and generated references in the same change. Unknown extension blocks still exercise default branches, and ACP continues to reject inbound image prompt content independently of the harness vocabulary.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-07-04-drop-image-content-block.md](../02-notes/implemented/simplification/2026-07-04-drop-image-content-block.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-07-04-drop-image-content-block.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-07-04-drop-image-content-block.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | The source note names this file directly. Defines `ImageBlock`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/attachment/attachment-local/src/image.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/attachment/attachment-local/src/image.ts) | runtime implementation | Defines `image`, a construct named by the note. | `symbol-definition` |
| [`tsconfig.base.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.base.json) | composition and configuration | Contains the exact code literal `packages/llm/llm/src/types.ts` named by the note. | `exact-code-occurrence` |
| [`docs/event-producer-consumer.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/event-producer-consumer.md) | package contract and examples | Contains the exact code literal `packages/llm/llm/src/types.ts` named by the note. | `exact-code-occurrence` |
| [`scripts/type-equiv.manifest.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/type-equiv.manifest.json) | repository automation | Contains the exact code literal `packages/llm/llm/src/types.ts` named by the note. | `exact-code-occurrence` |
| [`docs/subsystems/llm-streaming.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/llm-streaming.md) | package contract and examples | Contains the exact code literal `packages/llm/llm/src/types.ts` named by the note. | `exact-code-occurrence` |
| [`docs/event-producer-consumer.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/event-producer-consumer.zh.md) | package contract and examples | Contains the exact code literal `packages/llm/llm/src/types.ts` named by the note. | `exact-code-occurrence` |
| [`docs/subsystems/llm-streaming.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/llm-streaming.zh.md) | package contract and examples | Contains the exact code literal `packages/llm/llm/src/types.ts` named by the note. | `exact-code-occurrence` |
| [`docs/cookbook/adding-an-llm-adapter.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cookbook/adding-an-llm-adapter.md) | package contract and examples | Contains the exact code literal `packages/llm/llm/src/types.ts` named by the note. | `exact-code-occurrence` |
| [`docs/cookbook/adding-an-llm-adapter.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cookbook/adding-an-llm-adapter.zh.md) | package contract and examples | Contains the exact code literal `packages/llm/llm/src/types.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `image` | `const` | [`packages/attachment/attachment-local/src/image.ts:55`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/attachment/attachment-local/src/image.ts#L55) | `const image = sharp(data, { failOn: 'error', limitInputPixels: false })` |
| `ImageBlock` | `interface` | [`packages/llm/llm/src/types.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L71) | `export interface ImageBlock {` |
| `ContentBlockMap` | `interface` | [`packages/llm/llm/src/types.ts:99`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L99) | `export interface ContentBlockMap {` |

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.
- Source verification intent: No harness ImageBlock is constructed outside Agent Note records. ACP's independent inbound-image rejection remains tested, while adapter, codec, and compaction default branches are covered with plugin-defined block types.

## How to read the implementation

1. Start with [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/evidence`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/testing`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`
- Aliases: `image`, `ImageBlock`, `ContentBlockMap`, `packages/llm/llm/src/types.ts`, `Drop the `image` content block until a path can honor it`, `simplification`, `boundary`, `evidence`, `recovery`, `build release`, `context`, `extensions`, `llm`, `protocols`
- Regex: `(?i)(image|ImageBlock|ContentBlockMap|packages/llm/llm/src/types\.ts|simplification|boundary|evidence|recovery)`

```bash
rg -n --pcre2 "(?i)(image|ImageBlock|ContentBlockMap|packages/llm/llm/src/types\\.ts|simplification|boundary|evidence|recovery)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0654. Drop `GenerateOptions.prefill` and `ToolSchema.strict` --- request knobs with no working end-to-end path](0654-drop-generateoptions-prefill-and-toolschema-strict-request-knobs-with-no.md): Shares source implementation: `docs/cookbook/adding-an-llm-adapter.md`, `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0657. Prune producer-less vocabulary variants (block cache hints, the `agent` message source, the `continuation` turn trigger)](0657-prune-producer-less-vocabulary-variants-block-cache-hints-the-agent-mess.md): Shares source implementation: `packages/llm/llm/src/types.ts`, `scripts/type-equiv.manifest.json`.
- **`shares-code-with`** — [0284. A minimal read_image tool over existing seams](0284-a-minimal-read-image-tool-over-existing-seams.md): Shares source implementation: `packages/attachment/attachment-local/src/image.ts`, `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0017. Mandatory `User-Agent` attribution for provider requests](0017-mandatory-user-agent-attribution-for-provider-requests.md): Shares source implementation: `docs/subsystems/llm-streaming.md`, `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0651. Drop unconsumed assembled LLM convenience surfaces](0651-drop-unconsumed-assembled-llm-convenience-surfaces.md): Shares source implementation: `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares source implementation: `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0567. Dedicated full-screen TUI front door](0567-dedicated-full-screen-tui-front-door.md): Shares source implementation: `packages/llm/llm/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0452-drop-the-image-content-block-until-a-path-can-honor-it.md`.
