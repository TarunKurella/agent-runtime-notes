---
id: "dsh-note-0103"
title: "Slot declaration injection and reload lifetimes"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-05-slot-declaration-injection.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/registry"
aliases:
  - "inject"
  - "SlotMap"
  - "spec"
  - "ConversationController"
  - "Context"
  - "SlotRegistry.inject"
  - "ctx.plugin"
  - "slots.inject"
  - "slots.register"
  - "Slot declaration injection and reload lifetimes"
  - "architecture"
  - "boundary"
  - "compatibility"
  - "lifecycle"
search_regex: "(?i)(inject|SlotMap|spec|ConversationController|Context|SlotRegistry\\.inject|ctx\\.plugin|slots\\.inject)"
---

# 0103. Slot declaration injection and reload lifetimes — implementation context

## Open this when

Client plugins may contribute to a slot before or after the plugin that declares it. Cordis service injection cannot express this dependency: a service is only an indirect ordering signal, client manifest dependency rows do not sequence activation, and a slot can disappear and return while every related service remains mounted. Registering immediately therefore races an undeclared slot, while waiting on an unrelated service couples independently reloadable features. Slot-level hot replacement also requires two independent owners.

## Source decision

SlotRegistry.inject(name, callback) makes the declared slot itself the dependency. The full SlotMap key is statically checked; there is no namespace builder, synthetic Cordis service, or slot-specific Context. The callback runs immediately when the declaration exists, otherwise waits, and returns either one synchronous disposer or a synchronous iterable of disposers. Iterable effects install transactionally: a later setup failure disposes every earlier yielded effect in reverse order. The ledger records a declaration epoch distinct from the slot's ordinary entry version.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-05-slot-declaration-injection.md](../02-notes/implemented/architecture/2026-08-05-slot-declaration-injection.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-05-slot-declaration-injection.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-05-slot-declaration-injection.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `Context`, a construct named by the note. | `symbol-definition` |
| [`vendor/timer/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/timer/src/index.ts) | package entry point | Defines `Context`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/fiber.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts) | runtime implementation | Defines `Context`, a construct named by the note. | `symbol-definition` |
| [`vendor/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts) | package entry point | Defines `Context`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts) | runtime implementation | Defines `Context`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/registry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts) | runtime implementation | Defines `inject`, a construct named by the note. | `symbol-definition` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `inject`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/invariant.ts) | runtime contract checks | Defines `inject`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts) | package entry point | Defines `spec`, a construct named by the note. | `symbol-definition` |
| [`packages/client/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts) | package entry point | Defines `inject`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts) | package entry point | Defines `inject`, a construct named by the note. | `symbol-definition` |
| [`packages/lsp/lsp-stdio/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/index.ts) | package entry point | Defines `spec`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `inject` | `const` | [`packages/acp/acp/src/index.ts:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L44) | `export const inject = ['agents']` |
| `inject` | `const` | [`packages/client/hmr/src/index.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts#L28) | `export const inject = ['clientModules', 'webServer']` |
| `SlotMap` | `interface` | [`packages/client/runtime/src/client/slots.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/slots.ts#L26) | `interface SlotMap {` |
| `spec` | `const` | [`packages/client/runtime/src/client/slots.ts:165`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/slots.ts#L165) | `const spec = this._core.specDynamic(key)` |
| `ConversationController` | `class` | [`packages/client/ui-conversation/src/client/service.ts:91`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/service.ts#L91) | `export class ConversationController extends Service implements IConversation {` |
| `SlotMap` | `interface` | [`packages/client/ui-layout/src/client/index.ts:34`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/index.ts#L34) | `interface SlotMap {` |
| `SlotMap` | `interface` | [`packages/client/ui-slots/src/index.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L24) | `export interface SlotMap {}` |
| `spec` | `const` | [`packages/client/ui-slots/src/index.ts:792`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L792) | `const spec = rec.spec` |
| `SlotMap` | `interface` | [`packages/client/ui-tool/src/client/contract/slots.ts:8`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/contract/slots.ts#L8) | `interface SlotMap {` |
| `SlotMap` | `interface` | [`packages/extensions/ui-cordis/src/client/slots.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/slots.ts#L24) | `interface SlotMap {` |
| `inject` | `const` | [`packages/fs/fs/src/invariant.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/invariant.ts#L12) | `export const inject = ['invariants']` |
| `spec` | `const` | [`packages/goal/goal/src/index.ts:252`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L252) | `const spec = resolveCreateGoal(request, this.resolved.defaultMaxGoalRounds)` |
| `spec` | `const` | [`packages/lsp/lsp-stdio/src/index.ts:336`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/index.ts#L336) | `const spec: InstanceSpec = {` |
| `inject` | `const` | [`packages/sdk/server/src/index.ts:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts#L22) | `export const inject = ['agents']` |
| `spec` | `const` | [`packages/subagent/subagent-acp/src/index.ts:154`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/src/index.ts#L154) | `const spec: AcpRunSpec = {` |
| `Context` | `interface` | [`vendor/cordis/src/events.ts:35`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts#L35) | `export interface Context {` |

### Tests and executable evidence

- [`packages/client/ui-slots/tests/core.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/tests/core.client.spec.ts) — A test under the owning area exercises or imports `SlotMap`.
- [`packages/client/ui-slots/tests/type-chain.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/tests/type-chain.client.spec.tsx) — A test under the owning area exercises or imports `SlotMap`.
- [`packages/client/runtime/tests/client-apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/client-apply.client.spec.ts) — A test under the owning area exercises or imports `SlotMap`.
- [`packages/client/ui-slots/tests/dynamic-keys.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/tests/dynamic-keys.client.spec.ts) — A test under the owning area exercises or imports `SlotMap`.
- [`packages/client/runtime/tests/slots-service.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/slots-service.client.spec.ts) — A test under the owning area exercises or imports `SlotMap`.
- [`packages/client/ui-conversation/tests/service-orchestration.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/service-orchestration.client.spec.ts) — A test under the owning area exercises or imports `ConversationController`.

## How to read the implementation

1. Start with [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/extensions`, `domain/filesystem`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/registry`
- Aliases: `inject`, `SlotMap`, `spec`, `ConversationController`, `Context`, `SlotRegistry.inject`, `ctx.plugin`, `slots.inject`, `slots.register`, `Slot declaration injection and reload lifetimes`, `architecture`, `boundary`, `compatibility`, `lifecycle`
- Regex: `(?i)(inject|SlotMap|spec|ConversationController|Context|SlotRegistry\.inject|ctx\.plugin|slots\.inject)`

```bash
rg -n --pcre2 "(?i)(inject|SlotMap|spec|ConversationController|Context|SlotRegistry\\.inject|ctx\\.plugin|slots\\.inject)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0656. Fold the stdio UI helper into the stdio app](0656-fold-the-stdio-ui-helper-into-the-stdio-app.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0058. Separate context injection from turn execution](0058-separate-context-injection-from-turn-execution.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/fs/fs/src/invariant.ts`.
- **`shares-code-with`** — [0636. Generated plugin config catalog](0636-generated-plugin-config-catalog.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0573. TUI banner brand gradient](0573-tui-banner-brand-gradient.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0387. One gated in-file format for Agent Notes](0387-one-gated-in-file-format-for-agent-notes.md): Shares source implementation: `vendor/cordis/src/events.ts`, `vendor/cordis/src/fiber.ts`.
- **`shares-code-with`** — [0179. Web todo display --- snapshot side-effect channel + two render surfaces](0179-web-todo-display-snapshot-side-effect-channel-two-render-surfaces.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0455. Remove implicit batching from ordinary sends](0455-remove-implicit-batching-from-ordinary-sends.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/client/hmr/src/index.ts`.
- **`shares-code-with`** — [0388. Export JSDoc gate](0388-export-jsdoc-gate.md): Shares source implementation: `vendor/cordis/src/events.ts`, `vendor/cordis/src/registry.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0103-slot-declaration-injection-and-reload-lifetimes.md`.
