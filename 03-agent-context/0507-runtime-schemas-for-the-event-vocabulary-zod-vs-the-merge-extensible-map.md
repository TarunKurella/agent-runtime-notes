---
id: "dsh-note-0507"
title: "Runtime schemas for the event vocabulary (Zod vs the merge-extensible-map pattern)"
status: "proposed"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/architecture/2026-06-16-typed-event-schemas.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/proposed"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "SessionEventMap"
  - "isJsonValue"
  - "TurnEndReasonMap"
  - "TurnEndReason"
  - "InferArgs"
  - "defineTool"
  - "BlockAssembler"
  - "MessageSourceMap"
  - "MessageSource"
  - "assertNever"
  - "ContentBlockMap"
  - "FinishReasonMap"
  - "FinishReason"
  - "HeaderLine"
search_regex: "(?i)(SessionEventMap|isJsonValue|TurnEndReasonMap|TurnEndReason|InferArgs|defineTool|BlockAssembler|MessageSourceMap)"
---

# 0507. Runtime schemas for the event vocabulary (Zod vs the merge-extensible-map pattern) — implementation context

## Open this when

The harness models its core vocabulary --- content blocks, message sources, finish reasons, turn triggers, turn-end reasons, and session events --- as merge-extensible maps: a TypeScript interface (e.g. SessionEventMap, ContentBlockMap) that plugins augment via declaration merging, with the public union derived as Map[keyof Map]. This is the repo's universal extension pattern, documented in docs/architecture.md ("The same merge-extensible-map pattern is used for MessageSource, FinishReason, TurnTrigger, and TurnEndReason") and relied on by the defineTool InferArgs DSL and the assertNever exhaustiveness.

## Source decision

Defer. If runtime validation is wanted at the durable boundary, Option B (schemastery on closed header and metadata shapes) is the proportionate step within the existing convention. Option C is an architecture decision that requires its own implementation Agent Note, including a choice between Zod and schemastery.

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/architecture/2026-06-16-typed-event-schemas.md](../02-notes/proposed/architecture/2026-06-16-typed-event-schemas.md)
- Pinned source: [.agents/notes/proposed/architecture/2026-06-16-typed-event-schemas.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/architecture/2026-06-16-typed-event-schemas.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. Defines `ContentBlockMap`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/never.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/never.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm`. Defines `assertNever`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/message.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/message.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm`. Defines `MessageSource`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. Defines `SessionEventMap`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. Defines `SessionEventMap`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/json.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/json.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `isJsonValue`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `defineTool`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `SessionEventMap` | `interface` | [`packages/core/agent/src/types.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts#L13) | `interface SessionEventMap {` |
| `isJsonValue` | `function` | [`packages/core/session/src/json.ts:188`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/json.ts#L188) | `export function isJsonValue(value: unknown): boolean {` |
| `TurnEndReasonMap` | `interface` | [`packages/core/session/src/types.ts:155`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L155) | `export interface TurnEndReasonMap {` |
| `TurnEndReason` | `type` | [`packages/core/session/src/types.ts:177`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L177) | `export type TurnEndReason = TurnEndReasonMap[keyof TurnEndReasonMap]` |
| `SessionEventMap` | `interface` | [`packages/core/session/src/types.ts:236`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L236) | `export interface SessionEventMap {` |
| `InferArgs` | `type` | [`packages/core/tools/src/schema.ts:175`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L175) | `export type InferArgs<S> = InferProperties<S, []>` |
| `defineTool` | `function` | [`packages/core/tools/src/schema.ts:545`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L545) | `export function defineTool<const S extends ParameterSchemaSpec, const O extends ValueSchemaSpec>(` |
| `SessionEventMap` | `interface` | [`packages/core/tools/src/types.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts#L26) | `interface SessionEventMap {` |
| `BlockAssembler` | `class` | [`packages/llm/llm/src/assembler.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/assembler.ts#L36) | `export class BlockAssembler {` |
| `MessageSourceMap` | `interface` | [`packages/llm/llm/src/message.ts:100`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/message.ts#L100) | `export interface MessageSourceMap {` |
| `MessageSource` | `type` | [`packages/llm/llm/src/message.ts:126`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/message.ts#L126) | `export type MessageSource = MessageSourceMap[keyof MessageSourceMap]` |
| `assertNever` | `function` | [`packages/llm/llm/src/never.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/never.ts#L16) | `export function assertNever(value: never, context?: string): never {` |
| `ContentBlockMap` | `interface` | [`packages/llm/llm/src/types.ts:99`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L99) | `export interface ContentBlockMap {` |
| `FinishReasonMap` | `interface` | [`packages/llm/llm/src/types.ts:116`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L116) | `export interface FinishReasonMap {` |
| `FinishReason` | `type` | [`packages/llm/llm/src/types.ts:125`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L125) | `export type FinishReason = FinishReasonMap[keyof FinishReasonMap]` |
| `HeaderLine` | `interface` | [`packages/session/session-persistence-jsonl/src/format.ts:33`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/format.ts#L33) | `export interface HeaderLine {` |

### Tests and executable evidence

- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `defineTool`. A test under the owning area exercises or imports `InferArgs`.
- [`packages/core/session/tests/fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/fork.spec.ts) — A test under the owning area exercises or imports `SessionEventMap`. A test under the owning area exercises or imports `TurnEndReason`.
- [`packages/llm/llm-pi-ai/tests/assemble.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/assemble.ts) — A test under the owning area exercises or imports `FinishReason`. A test under the owning area exercises or imports `BlockAssembler`.
- [`packages/core/tools/tests/schema.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/schema.spec.ts) — A test under the owning area exercises or imports `InferArgs`.
- [`packages/llm/llm/tests/assembler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/assembler.spec.ts) — A test under the owning area exercises or imports `assertNever`. A test under the owning area exercises or imports `BlockAssembler`.
- [`packages/core/session/tests/json.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/json.spec.ts) — A test under the owning area exercises or imports `isJsonValue`.
- [`packages/llm/llm/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/properties.spec.ts) — A test under the owning area exercises or imports `BlockAssembler`.
- [`packages/core/tools/tests/py-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/py-types.spec.ts) — A test under the owning area exercises or imports `defineTool`.
- Source verification intent: Option C proceeds only through its own implementation Agent Note, never as a persistence side effect. If Option B is taken up, the closed header/metadata shapes (the JSONL isHeaderLine guard and kin) validate through schemastery in place of hand-rolled guards, with the merge-extensible maps untouched.

## How to read the implementation

1. Start with [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/schema-types`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/proposed`, `mechanism/policy`, `mechanism/registry`
- Aliases: `SessionEventMap`, `isJsonValue`, `TurnEndReasonMap`, `TurnEndReason`, `InferArgs`, `defineTool`, `BlockAssembler`, `MessageSourceMap`, `MessageSource`, `assertNever`, `ContentBlockMap`, `FinishReasonMap`, `FinishReason`, `HeaderLine`
- Regex: `(?i)(SessionEventMap|isJsonValue|TurnEndReasonMap|TurnEndReason|InferArgs|defineTool|BlockAssembler|MessageSourceMap)`

```bash
rg -n --pcre2 "(?i)(SessionEventMap|isJsonValue|TurnEndReasonMap|TurnEndReason|InferArgs|defineTool|BlockAssembler|MessageSourceMap)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0002. Source-owned session immutability and dev-mode invariants](0002-source-owned-session-immutability-and-dev-mode-invariants.md): The source note links to this decision directly.
- **`source-link`** — [0009. Session persistence as an abstract service over the existing `SessionEvent`](0009-session-persistence-as-an-abstract-service-over-the-existing-sessioneven.md): The source note links to this decision directly.
- **`shares-code-with`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares source implementation: `docs/architecture.md`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0140. Interception extension points --- the typed-Decision surface a hook programs against](0140-interception-extension-points-the-typed-decision-surface-a-hook-programs.md): Shares source implementation: `docs/architecture.md`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0071. Create every message as an identified immutable value](0071-create-every-message-as-an-identified-immutable-value.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0561. Plan mode --- a logged per-agent session mode](0561-plan-mode-a-logged-per-agent-session-mode.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0158. persistent PTY sessions](0158-persistent-pty-sessions.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0142. Dynamic workflows --- a script-driven multi-agent orchestration seam](0142-dynamic-workflows-a-script-driven-multi-agent-orchestration-seam.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0507-runtime-schemas-for-the-event-vocabulary-zod-vs-the-merge-extensible-map.md`.
