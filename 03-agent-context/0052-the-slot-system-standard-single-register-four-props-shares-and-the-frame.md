---
id: "dsh-note-0052"
title: "The slot system standard --- single register, four props shares, and the framework store seat"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-22-slot-type-chain-implementation.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "sessionId"
  - "actions"
  - "SlotRegistry"
  - "select"
  - "empty"
  - "SlotMap"
  - "SlotKind"
  - "OwnerOf"
  - "ChainRenderOpts"
  - "ChainSelect"
  - "PropsRenderSlots"
  - "SlotComponent"
  - "priority"
  - "SlotRenderer"
search_regex: "(?i)(sessionId|actions|SlotRegistry|select|empty|SlotMap|SlotKind|OwnerOf)"
---

# 0052. The slot system standard --- single register, four props shares, and the framework store seat — implementation context

## Open this when

The page is composed at runtime from independently loaded plugins, so the UI needs a composition mechanism that answers four questions with static force. Who may render into a region --- and is that authority enforceable, or merely conventional? How does a component receive everything it needs while staying a pure function (no ctx, no framework imports), without every value being hand-threaded through assembly code? Where does business live-data live so that streaming updates re-render precisely the subscribers --- without every plugin building its own subscription machinery?

## Source decision

One sentence: the shell renders only 'root'; a plugin composes UI through a single register call that simultaneously occupies a slot, declares+authorizes its child slots, declares its store, and injects its business face; components are pure functions whose props arrive in four shares, each auto-derived from its single source of truth. SlotRegistry (client runtime) declares 'root' at construction --- single/root, owner: {} --- and its SlotMap merge lives in the runtime package.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-22-slot-type-chain-implementation.md](../02-notes/implemented/architecture/2026-07-22-slot-type-chain-implementation.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-22-slot-type-chain-implementation.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-22-slot-type-chain-implementation.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/scope/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/scope`. Defines `scope`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/scope/src/store.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/store.ts) | runtime implementation | Core file in the package named by the note: `packages/core/scope`. Defines `scope`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/scope/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/scope`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`.`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/.) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/scope`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope) | package or module directory | The note names this package or capability. | `named-package` |
| [`scripts/slot-walk.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/slot-walk.ts) | repository automation | Defines `entryKey`, a construct named by the note. | `symbol-definition` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `root`, a construct named by the note. Defines `children`, a construct named by the note. | `symbol-definition` |
| [`scripts/rescope-vendor.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/rescope-vendor.ts) | repository automation | Defines `find`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/fiber.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts) | runtime implementation | Defines `args`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts) | runtime implementation | Defines `hooks`, a construct named by the note. | `symbol-definition` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `sessionId`, a construct named by the note. | `symbol-definition` |
| [`packages/client/web/src/app.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/app.tsx) | runtime implementation | Defines `useSessions`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `sessionId` | `const` | [`packages/acp/acp/src/index.ts:254`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L254) | `const sessionId = SessionId(randomUUID())` |
| `actions` | `const` | [`packages/client/runtime/src/client/contract/store.ts:218`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/store.ts#L218) | `const actions = {} as Record<string, (...params: unknown[]) => void>` |
| `SlotRegistry` | `class` | [`packages/client/runtime/src/client/slots.ts:93`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/slots.ts#L93) | `export class SlotRegistry extends Service {` |
| `select` | `const` | [`packages/client/ui-permission-presets/src/client/index.ts:118`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-permission-presets/src/client/index.ts#L118) | `const select = (preset: string): Promise<void> => controller.select(preset)` |
| `empty` | `const` | [`packages/client/ui-primitives/src/WebBlock.tsx:160`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/WebBlock.tsx#L160) | `const empty = (answer === undefined \|\| answer === '') && sources.length === 0` |
| `SlotMap` | `interface` | [`packages/client/ui-slots/src/index.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L24) | `export interface SlotMap {}` |
| `SlotKind` | `type` | [`packages/client/ui-slots/src/index.ts:88`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L88) | `export type SlotKind = 'single' \| 'list' \| 'keyed' \| 'chain'` |
| `OwnerOf` | `type` | [`packages/client/ui-slots/src/index.ts:148`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L148) | `export type OwnerOf<K extends keyof SlotMap & string> =` |
| `ChainRenderOpts` | `interface` | [`packages/client/ui-slots/src/index.ts:233`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L233) | `export interface ChainRenderOpts {` |
| `ChainSelect` | `type` | [`packages/client/ui-slots/src/index.ts:257`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L257) | `export type ChainSelect<O extends object, M> = (owner: O) => M \| null` |
| `PropsRenderSlots` | `type` | [`packages/client/ui-slots/src/index.ts:336`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L336) | `export type PropsRenderSlots<S extends keyof SlotMap & string> = {` |
| `SlotComponent` | `type` | [`packages/client/ui-slots/src/index.ts:370`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L370) | `export type SlotComponent<P> = (props: P) => ReactNode` |
| `priority` | `const` | [`packages/client/ui-slots/src/index.ts:796`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L796) | `const priority = options.priority ?? 0` |
| `SlotRenderer` | `interface` | [`packages/client/ui-slots/src/renderer.ts:189`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/renderer.ts#L189) | `export interface SlotRenderer {` |
| `PropsStore` | `type` | [`packages/client/ui-slots/src/store.ts:123`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/store.ts#L123) | `export type PropsStore<H> = H extends StoreHandle<infer T, infer A>` |
| `createSlotRenderer` | `function` | [`packages/client/web-react/src/scoped-slots.tsx:897`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/scoped-slots.tsx#L897) | `export function createSlotRenderer(): SlotRenderer {` |

### Tests and executable evidence

- [`packages/core/scope/tests/store.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/tests/store.spec.ts) — A test under the owning area exercises or imports `actions`.
- [`packages/lsp/lsp-stdio/tests/fixture-server.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/fixture-server.ts) — A test under the owning area exercises or imports `send`.
- [`packages/client/ui-slots/tests/core.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/tests/core.client.spec.ts) — A test under the owning area exercises or imports `SlotMap`. A test under the owning area exercises or imports `priority`.
- [`packages/client/runtime/tests/invariant.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/invariant.client.spec.ts) — A test under the owning area exercises or imports `SlotRegistry`.
- [`packages/client/runtime/tests/client-apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/client-apply.client.spec.ts) — A test under the owning area exercises or imports `SlotRegistry`.
- [`packages/client/ui-slots/tests/type-chain.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/tests/type-chain.client.spec.tsx) — A test under the owning area exercises or imports `SlotMap`. A test under the owning area exercises or imports `PropsRenderSlots`.
- [`packages/client/runtime/tests/slots-service.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/slots-service.client.spec.ts) — A test under the owning area exercises or imports `SlotRegistry`.
- [`packages/client/ui-slots/tests/dynamic-keys.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/tests/dynamic-keys.client.spec.ts) — A test under the owning area exercises or imports `SlotMap`. A test under the owning area exercises or imports `SlotComponent`.

## How to read the implementation

1. Start with [`packages/core/scope/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `sessionId`, `actions`, `SlotRegistry`, `select`, `empty`, `SlotMap`, `SlotKind`, `OwnerOf`, `ChainRenderOpts`, `ChainSelect`, `PropsRenderSlots`, `SlotComponent`, `priority`, `SlotRenderer`
- Regex: `(?i)(sessionId|actions|SlotRegistry|select|empty|SlotMap|SlotKind|OwnerOf)`

```bash
rg -n --pcre2 "(?i)(sessionId|actions|SlotRegistry|select|empty|SlotMap|SlotKind|OwnerOf)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0045. Web client architecture --- the client cordis plugin tree, the slot system, and the React-free object layer](0045-web-client-architecture-the-client-cordis-plugin-tree-the-slot-system-an.md): The source note links to this decision directly.
- **`source-link`** — [0103. Slot declaration injection and reload lifetimes](0103-slot-declaration-injection-and-reload-lifetimes.md): The source note links to this decision directly.
- **`shares-code-with`** — [0061. Web client Agent-scope parity model and the provisioning channel (agents/scope / blank reuse / provide)](0061-web-client-agent-scope-parity-model-and-the-provisioning-channel-agents.md): Shares source implementation: `.`, `packages/core/scope`.
- **`shares-code-with`** — [0201. Cross-workspace session resume](0201-cross-workspace-session-resume.md): Shares source implementation: `packages/core/scope`, `packages/core/scope/src/index.ts`.
- **`shares-code-with`** — [0035. Agent-scope runtime design and correctness](0035-agent-scope-runtime-design-and-correctness.md): Shares source implementation: `packages/core/scope/src/index.ts`, `packages/core/scope/src/invariant.ts`.
- **`shares-code-with`** — [0024. Prompt variables and tool-guidance ownership](0024-prompt-variables-and-tool-guidance-ownership.md): Shares source implementation: `packages/core/scope/src/index.ts`, `packages/core/scope/src/invariant.ts`.
- **`shares-code-with`** — [0173. Durable subagent catalog and list_agents](0173-durable-subagent-catalog-and-list-agents.md): Shares source implementation: `packages/core/scope/src/index.ts`, `packages/core/scope/src/invariant.ts`.
- **`shares-code-with`** — [0036. Shared scoped-layer storage](0036-shared-scoped-layer-storage.md): Shares source implementation: `packages/core/scope/src/index.ts`, `packages/core/scope/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0052-the-slot-system-standard-single-register-four-props-shares-and-the-frame.md`.
