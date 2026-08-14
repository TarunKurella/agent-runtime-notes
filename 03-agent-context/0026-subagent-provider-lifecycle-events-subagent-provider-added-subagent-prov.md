---
id: "dsh-note-0026"
title: "Subagent provider-lifecycle events --- `subagent/provider-added` / `subagent/provider-removed`"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-05-subagent-provider-lifecycle-events.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "apply"
  - "subagents"
  - "prompt"
  - "providerName"
  - "toolName"
  - "subagent/provider-added"
  - "subagent/provider-removed"
  - "dsh-tool-subagent"
  - "SubagentProvider.inheritsParentContext"
  - "inheritsParentContext"
  - "Entry.init"
  - "ctx.subagents"
  - "-removed"
  - "tools/change"
search_regex: "(?i)(apply|subagents|prompt|providerName|toolName|subagent/provider\\-added|subagent/provider\\-removed|dsh\\-tool\\-subagent)"
---

# 0026. Subagent provider-lifecycle events --- `subagent/provider-added` / `subagent/provider-removed` — implementation context

## Open this when

The prompt-variables Agent Note makes dsh-tool-subagent DERIVE its model-facing wording from its provider: SubagentProvider.inheritsParentContext (spawn/ACP false, fork true) drives both the tool description and the prompt parameter description, so the fork tool stops lying about context inheritance. That fix created a cross-fiber data dependency: a tool's description is fixed at TOOL REGISTRATION (deliberately --- the description is where tool-choice guidance lives), but the provider arrives on its own plugin fiber, on no particular schedule.

## Source decision

The registry announces provider membership as typed events, and the consumer mirrors them instead of assuming order: subagent/provider-added(provider) --- a provider became resolvable in the ctx.subagents registry. Emitted on registration. subagent/provider-removed(name) --- a provider left the registry (its plugin's fiber was disposed --- an unload or an HMR reload). Emitted from the registration's disposer.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-05-subagent-provider-lifecycle-events.md](../02-notes/implemented/architecture/2026-07-05-subagent-provider-lifecycle-events.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-05-subagent-provider-lifecycle-events.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-05-subagent-provider-lifecycle-events.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/defensive-patterns.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/defensive-patterns.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/subsystems/subagent.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/subagent.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `subagent/provider-added` named by the note. | `exact-code-occurrence, named-file` |
| [`docs/event-producer-consumer.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/event-producer-consumer.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `subagent/provider-added` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Core file in the package named by the note: `packages/acp/acp`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/acp/acp/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/acp/acp`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/tool-subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/tool-subagent`. Defines `apply`, a construct named by the note. | `exact-code-occurrence, named-package-member, symbol-definition` |
| [`packages/subagent/tool-subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/tool-subagent`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/acp/acp`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/subagent/tool-subagent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/schedule/schedule/src/domain.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/domain.ts) | runtime implementation | Defines `prompt`, a construct named by the note. | `symbol-definition` |
| [`packages/subagent/subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/invariant.ts) | runtime contract checks | Defines `providerName`, a construct named by the note. | `symbol-definition` |
| [`packages/acp/acp/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/README.md) | package contract and examples | Core file in the package named by the note: `packages/acp/acp`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `apply` | `function` | [`packages/acp/acp/src/index.ts:105`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L105) | `export function apply(ctx: Context, config: AcpConfig): void {` |
| `subagents` | `const` | [`packages/acp/acp/src/index.ts:376`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L376) | `const subagents = ctx.get('subagents') as ContinuableDrain \| undefined` |
| `apply` | `const` | [`packages/acp/acp/src/invariant.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/invariant.ts#L28) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `prompt` | `const` | [`packages/schedule/schedule/src/domain.ts:389`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/domain.ts#L389) | `const prompt = value['prompt']` |
| `providerName` | `const` | [`packages/subagent/subagent/src/invariant.ts:39`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/invariant.ts#L39) | `const providerName = args[0] as string` |
| `apply` | `function` | [`packages/subagent/tool-subagent/src/index.ts:267`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts#L267) | `export function apply(ctx: Context, config: Config): void {` |
| `toolName` | `const` | [`packages/subagent/tool-subagent/src/index.ts:277`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts#L277) | `const toolName = config.toolName ?? 'subagent'` |
| `apply` | `const` | [`packages/subagent/tool-subagent/src/invariant.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/invariant.ts#L28) | `export const apply = (ctx: Context): Promise<() => void> =>` |

### Tests and executable evidence

- [`packages/subagent/subagent/tests/continuation.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/continuation.spec.ts) — A test under the owning area exercises or imports `providerName`.
- [`packages/subagent/subagent/tests/list-children.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/list-children.spec.ts) — A test under the owning area exercises or imports `providerName`.
- [`packages/subagent/tool-subagent/tests/scripted-provider.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/tests/scripted-provider.ts) — A test under the owning area exercises or imports `inheritsParentContext`.
- [`packages/subagent/tool-subagent/tests/tool-subagent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/tests/tool-subagent.spec.ts) — A test under the owning area exercises or imports `inheritsParentContext`. A test under the owning area exercises or imports `providerName`.
- [`packages/subagent/subagent/tests/continuation-inheritance.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/continuation-inheritance.spec.ts) — A test under the owning area exercises or imports `providerName`.
- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — Contains the exact code literal `tools/change` named by the note.
- [`packages/core/tools/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/invariant.spec.ts) — Contains the exact code literal `tools/change` named by the note.
- [`packages/typert/registry/tests/typert.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/tests/typert.spec.ts) — Contains the exact code literal `tools/change` named by the note.

## How to read the implementation

1. Start with [`docs/defensive-patterns.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/defensive-patterns.md) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/discovery-routing`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/jobs-tasks`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/tools`, `lifecycle/implemented`, `mechanism/projection`, `mechanism/registry`
- Aliases: `apply`, `subagents`, `prompt`, `providerName`, `toolName`, `subagent/provider-added`, `subagent/provider-removed`, `dsh-tool-subagent`, `SubagentProvider.inheritsParentContext`, `inheritsParentContext`, `Entry.init`, `ctx.subagents`, `-removed`, `tools/change`
- Regex: `(?i)(apply|subagents|prompt|providerName|toolName|subagent/provider\-added|subagent/provider\-removed|dsh\-tool\-subagent)`

```bash
rg -n --pcre2 "(?i)(apply|subagents|prompt|providerName|toolName|subagent/provider\\-added|subagent/provider\\-removed|dsh\\-tool\\-subagent)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0024. Prompt variables and tool-guidance ownership](0024-prompt-variables-and-tool-guidance-ownership.md): The source note links to this decision directly.
- **`shares-code-with`** — [0120. Product subagent providers live in the shared profile host](0120-product-subagent-providers-live-in-the-shared-profile-host.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/subagent/tool-subagent`.
- **`shares-code-with`** — [0662. Drop unconsumed skill provider events](0662-drop-unconsumed-skill-provider-events.md): Shares source implementation: `docs/subsystems/subagent.md`, `packages/subagent/tool-subagent`.
- **`shares-code-with`** — [0148. Background subagent tasks](0148-background-subagent-tasks.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.
- **`shares-code-with`** — [0153. Configure subagent persona, tool visibility, and depth](0153-configure-subagent-persona-tool-visibility-and-depth.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.
- **`shares-code-with`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares source implementation: `packages/acp/acp`, `packages/acp/acp/src/index.ts`.
- **`shares-code-with`** — [0203. SDK max output tokens](0203-sdk-max-output-tokens.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.
- **`shares-code-with`** — [0462. Merge subagent control into the subagent service](0462-merge-subagent-control-into-the-subagent-service.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0026-subagent-provider-lifecycle-events-subagent-provider-added-subagent-prov.md`.
