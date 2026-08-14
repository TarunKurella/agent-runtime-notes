---
id: "dsh-note-0657"
title: "Prune producer-less vocabulary variants (block cache hints, the `agent` message source, the `continuation` turn trigger)"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-07-04-prune-producerless-vocabulary-variants.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "TurnEndReasonMap"
  - "cache"
  - "rejected"
  - "TextBlock"
  - "ToolResultBlock"
  - "source"
  - "packages/core/session/src/types.ts"
  - "CacheHint"
  - "cache?: CacheHint"
  - "packages/llm/llm/src/types.ts"
  - ".cache"
  - "prompt_cache_hit_tokens"
  - "cache_control"
  - "MessageSourceMap.agent"
search_regex: "(?i)(TurnEndReasonMap|cache|rejected|TextBlock|ToolResultBlock|source|packages/core/session/src/types\\.ts|CacheHint)"
---

# 0657. Prune producer-less vocabulary variants (block cache hints, the `agent` message source, the `continuation` turn trigger) — implementation context

## Open this when

The merge-extensible vocabulary maps are designed to grow by declaration merging, and the codebase already states the admission policy on TurnEndReasonMap (packages/core/session/src/types.ts): a variant like refusal is "deliberately omitted until" an adapter or loop first emits it. Three declared vocabulary items violated that policy --- each had no producer and no consumer, and two had not even a test: CacheHint and its cache?: CacheHint block fields on TextBlock/ToolResultBlock (packages/llm/llm/src/types.ts; the image block carried a third such field, which left with it --- see the drop-image Agent Note).

## Source decision

CacheHint, its cache? block fields, the agent message-source variant, and the continuation turn-trigger variant are deleted: the shipped vocabulary carries none of them. The llm-replay fixture uses an injection trigger (any non-message trigger serves its purpose). The type-equiv pastes in core.md and session.md match the pruned maps --- both symbols keep their rows in scripts/type-equiv.manifest.json, since each map survives minus a member --- and the content-block vocabulary Agent Note's consequences record cache hints as producer-gated rather than as having a home, per implemented/AGENTS.md.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-07-04-prune-producerless-vocabulary-variants.md](../02-notes/archived/simplification/2026-07-04-prune-producerless-vocabulary-variants.md)
- Pinned source: [.agents/notes/archived/simplification/2026-07-04-prune-producerless-vocabulary-variants.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-07-04-prune-producerless-vocabulary-variants.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | The source note names this file directly. Defines `TextBlock`, a construct named by the note. | `named-file, symbol-definition` |
| [`scripts/type-equiv.manifest.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/type-equiv.manifest.json) | repository automation | The source note names this file directly. Contains the exact code literal `packages/core/session/src/types.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | The source note names this file directly. Defines `TurnEndReasonMap`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/hmr/src/error.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/error.ts) | runtime implementation | Defines `source`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts) | package entry point | Defines `cache`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `rejected`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `TurnEndReasonMap` | `interface` | [`packages/core/session/src/types.ts:155`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L155) | `export interface TurnEndReasonMap {` |
| `cache` | `const` | [`packages/goal/goal/src/index.ts:224`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L224) | `const cache = this.cache(agent.session)` |
| `rejected` | `let` | [`packages/host/apiproxy/src/api-proxy.ts:1768`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1768) | `let rejected = false` |
| `TextBlock` | `interface` | [`packages/llm/llm/src/types.ts:54`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L54) | `export interface TextBlock {` |
| `ToolResultBlock` | `interface` | [`packages/llm/llm/src/types.ts:88`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L88) | `export interface ToolResultBlock {` |
| `source` | `const` | [`vendor/hmr/src/error.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/error.ts#L24) | `const source = readFileSync(file, 'utf8')` |

### Tests and executable evidence

- [`packages/core/agent/tests/verify-export-jsdoc.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/verify-export-jsdoc.spec.ts) — A test under the owning area exercises or imports `refusal`.
- Source verification intent: rg for CacheHint, the agent message-source spelling, and the continuation trigger spelling returns only Agent Note records (this one, and the drop-image Agent Note's account of the image block's own cache field); the llm-replay fixture asserts the same replay behavior with an injection trigger; the core-data-structures pastes and the type-equiv manifest are in sync.

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

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/simplification`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `TurnEndReasonMap`, `cache`, `rejected`, `TextBlock`, `ToolResultBlock`, `source`, `packages/core/session/src/types.ts`, `CacheHint`, `cache?: CacheHint`, `packages/llm/llm/src/types.ts`, `.cache`, `prompt_cache_hit_tokens`, `cache_control`, `MessageSourceMap.agent`
- Regex: `(?i)(TurnEndReasonMap|cache|rejected|TextBlock|ToolResultBlock|source|packages/core/session/src/types\.ts|CacheHint)`

```bash
rg -n --pcre2 "(?i)(TurnEndReasonMap|cache|rejected|TextBlock|ToolResultBlock|source|packages/core/session/src/types\\.ts|CacheHint)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0685. AGENTS.md --- Archived Agent Notes](0685-agents-md-archived-agent-notes.md): The source note links to this decision directly.
- **`source-link`** — [0653. Stop mirroring the token stream as an agent event](0653-stop-mirroring-the-token-stream-as-an-agent-event.md): The source note links to this decision directly.
- **`shares-code-with`** — [0450. Unify the agent id and the session id](0450-unify-the-agent-id-and-the-session-id.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0038. Initiating Agent scope over AsyncLocalStorage](0038-initiating-agent-scope-over-asynclocalstorage.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0449. Stop mirroring durable boundaries as agent events](0449-stop-mirroring-durable-boundaries-as-agent-events.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0150. Expose agent session identity and JSONL location to tools and hooks](0150-expose-agent-session-identity-and-jsonl-location-to-tools-and-hooks.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0265. Settlement delivery belongs to the continuation manager](0265-settlement-delivery-belongs-to-the-continuation-manager.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0071. Create every message as an identified immutable value](0071-create-every-message-as-an-identified-immutable-value.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0657-prune-producer-less-vocabulary-variants-block-cache-hints-the-agent-mess.md`.
