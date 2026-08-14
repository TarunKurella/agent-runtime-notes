---
id: "dsh-note-0632"
title: "Generated cordis events + services catalog"
status: "archived"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/process/2026-06-20-generated-cordis-catalog.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/ownership"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/archived"
  - "mechanism/generation"
aliases:
  - "ctx"
  - "logger"
  - "ToolDefinition"
  - "baseUrl"
  - "StreamChunk"
  - "GenerateOptions"
  - "Context"
  - "root"
  - "next"
  - "ctx.<key>"
  - "docs/architecture.md"
  - "verify-event-taxonomy"
  - "interface Events"
  - "interface Context"
search_regex: "(?i)(logger|ToolDefinition|baseUrl|StreamChunk|GenerateOptions|Context|root|next)"
---

# 0632. Generated cordis events + services catalog — implementation context

## Open this when

A plugin author needs two reference surfaces that no single document gave them: every cordis event they can listen to (with its exact signature and dispatch mode) and every ctx. service they can call (with its exact interface). The pieces existed but were scattered --- a hand-maintained event-taxonomy table in docs/architecture.md (names + prose Mode/Purpose, name-set-checked by verify-event-taxonomy), a Service-map table (8 rows of role prose), and the interface Events / interface Context declarations themselves.

## Source decision

Generate the catalog from source instead of hand-maintaining a table and verifying a subset. scripts/gen-cordis-catalog.ts uses the TypeScript compiler API to emit separate event and service references from declarations and source JSDoc. Events include dispatch modes and their original member JSDoc; services include public signatures with each method's original JSDoc. Deterministic --write and --check modes make both pages generated artifacts, with freshness enforced by doc-sync.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/process/2026-06-20-generated-cordis-catalog.md](../02-notes/archived/process/2026-06-20-generated-cordis-catalog.md)
- Pinned source: [.agents/notes/archived/process/2026-06-20-generated-cordis-catalog.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/process/2026-06-20-generated-cordis-catalog.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `docs/architecture.md` named by the note. | `exact-code-occurrence, named-file` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `agent/pre-step` named by the note. | `exact-code-occurrence, named-file` |
| [`scripts/gen-cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`.`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/.) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `Context`, a construct named by the note. Defines `root`, a construct named by the note. | `symbol-definition` |
| [`vendor/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts) | package entry point | Defines `next`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/src/profile-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts) | runtime implementation | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `logger`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Defines `GenerateOptions`, a construct named by the note. Defines `StreamChunk`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `ToolDefinition`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-pi-ai/src/catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts) | runtime implementation | Defines `baseUrl`, a construct named by the note. | `symbol-definition` |
| [`CLAUDE.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/CLAUDE.md) | package contract and examples | Contains the exact code literal `docs/architecture.md` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ctx` | `const` | [`apps/cli/src/profile-boot.ts:248`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L248) | `const ctx = await boot(NAME, rootConfig, structuredClone(allPatches(composed)), (hostCtx) => {` |
| `logger` | `const` | [`packages/acp/acp/src/index.ts:109`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L109) | `const logger = ctx.logger` |
| `ToolDefinition` | `interface` | [`packages/core/tools/src/index.ts:222`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L222) | `export interface ToolDefinition extends ToolSchema {` |
| `baseUrl` | `const` | [`packages/llm/llm-pi-ai/src/catalog.ts:502`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts#L502) | `const baseUrl = request.baseURL ?? base?.baseUrl ?? providerBaseUrl` |
| `StreamChunk` | `type` | [`packages/llm/llm/src/types.ts:291`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L291) | `export type StreamChunk =` |
| `GenerateOptions` | `interface` | [`packages/llm/llm/src/types.ts:320`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L320) | `export interface GenerateOptions {` |
| `Context` | `interface` | [`vendor/hmr/src/index.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L16) | `interface Context {` |
| `root` | `let` | [`vendor/hmr/src/index.ts:65`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L65) | `let root = dirname(filename)` |
| `next` | `const` | [`vendor/loader/src/index.ts:181`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts#L181) | `const next = fiber.parent.fiber` |

### Tests and executable evidence

- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `GenerateOptions`.
- [`packages/llm/llm/tests/topology.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/topology.spec.ts) — A test under the owning area exercises or imports `GenerateOptions`.
- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `ToolDefinition`.
- [`packages/llm/llm/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/invariant.spec.ts) — A test under the owning area exercises or imports `GenerateOptions`.
- [`packages/core/tools/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/scoped.spec.ts) — A test under the owning area exercises or imports `ToolDefinition`.
- [`packages/llm/llm/tests/call-config.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/call-config.spec.ts) — A test under the owning area exercises or imports `GenerateOptions`.
- [`packages/core/tools/tests/execution-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/execution-mode.spec.ts) — A test under the owning area exercises or imports `ToolDefinition`.
- [`scripts/translation-prompt.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-prompt.spec.ts) — Contains the exact code literal `docs/architecture.md` named by the note.

## How to read the implementation

1. Start with [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

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

- Tags: `class/process`, `concern/boundary`, `concern/evidence`, `concern/ownership`, `domain/agent-loop`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/storage`, `lifecycle/archived`, `mechanism/generation`
- Aliases: `ctx`, `logger`, `ToolDefinition`, `baseUrl`, `StreamChunk`, `GenerateOptions`, `Context`, `root`, `next`, `ctx.<key>`, `docs/architecture.md`, `verify-event-taxonomy`, `interface Events`, `interface Context`
- Regex: `(?i)(logger|ToolDefinition|baseUrl|StreamChunk|GenerateOptions|Context|root|next)`

```bash
rg -n --pcre2 "(?i)(logger|ToolDefinition|baseUrl|StreamChunk|GenerateOptions|Context|root|next)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0630. Doc-sync enforcement](0630-doc-sync-enforcement.md): The source note links to this decision directly.
- **`shares-code-with`** — [0636. Generated plugin config catalog](0636-generated-plugin-config-catalog.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `scripts/gen-cordis-catalog.ts`.
- **`shares-code-with`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares source implementation: `docs/architecture.md`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0651. Drop unconsumed assembled LLM convenience surfaces](0651-drop-unconsumed-assembled-llm-convenience-surfaces.md): Shares source implementation: `AGENTS.md`, `packages/acp/acp/src/index.ts`.
- **`shares-code-with`** — [0573. TUI banner brand gradient](0573-tui-banner-brand-gradient.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0507. Runtime schemas for the event vocabulary (Zod vs the merge-extensible-map pattern)](0507-runtime-schemas-for-the-event-vocabulary-zod-vs-the-merge-extensible-map.md): Shares source implementation: `docs/architecture.md`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0634. JSDoc completeness gate for the cordis surface](0634-jsdoc-completeness-gate-for-the-cordis-surface.md): Shares source implementation: `AGENTS.md`, `scripts/gen-cordis-catalog.ts`.
- **`shares-code-with`** — [0639. Generate the Cordis core API reference](0639-generate-the-cordis-core-api-reference.md): Shares source implementation: `apps/cli/src/profile-boot.ts`, `packages/core/tools/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0632-generated-cordis-events-services-catalog.md`.
