---
id: "dsh-note-0045"
title: "Web client architecture --- the client cordis plugin tree, the slot system, and the React-free object layer"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-19-gui-web-client-architecture.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "internal"
  - "view"
  - "sessions"
  - "match"
  - "lastSeq"
  - "children"
  - "inject"
  - "connection"
  - "Context"
  - "loader"
  - "runtime"
  - "modules"
  - "client"
  - "json"
search_regex: "(?i)(internal|view|sessions|match|lastSeq|children|inject|connection)"
---

# 0045. Web client architecture --- the client cordis plugin tree, the slot system, and the React-free object layer — implementation context

## Open this when

Two forces shape the browser client. First, streaming: in an event-driven conversation UI, if business state (the event window, streaming accumulation, pending interactions, the connection state machine) scatters across React components and a global store, every token chunk shakes the render tree, and swapping the UI library means rewriting the business logic.

## Source decision

Both ends run cordis. The host is a cordis plugin tree; the browser runs a second, client-side cordis tree whose every UI capability is a plugin loaded dynamically by a shell-held loader. Inside that tree, cordis ctx hosts all runtime facts (services, stores, session scopes) and React is pure projection: components import nothing from the framework, receive everything through props, and subscribe to immutable snapshots via useSyncExternalStore (uSES below).

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-19-gui-web-client-architecture.md](../02-notes/implemented/architecture/2026-07-19-gui-web-client-architecture.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-19-gui-web-client-architecture.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-19-gui-web-client-architecture.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/verify-client-domain-graph.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-client-domain-graph.ts) | repository automation | The source note names this file directly. Contains the exact code literal `src/client/` named by the note. | `exact-code-occurrence, named-file` |
| [`vendor/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/loader`. Defines `Context`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`vendor/loader/src/internal.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/internal.ts) | runtime implementation | Core file in the package named by the note: `vendor/loader`. Defines `require`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/hmr`. Defines `inject`, a construct named by the note. | `exact-code-occurrence, named-package-member, symbol-definition` |
| [`vendor/loader/src/config/tree.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/tree.ts) | runtime implementation | Core file in the package named by the note: `vendor/loader`. | `named-package-member` |
| [`vendor/loader/src/config/entry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/entry.ts) | runtime implementation | Core file in the package named by the note: `vendor/loader`. | `named-package-member` |
| [`vendor/loader/src/config/group.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/group.ts) | runtime implementation | Core file in the package named by the note: `vendor/loader`. | `named-package-member` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. Defines `Context`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`vendor/loader/src/config/isolate.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/isolate.ts) | runtime implementation | Core file in the package named by the note: `vendor/loader`. Defines `label`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/client/modules`. Core file in the package named by the note: `packages/client/modules`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/client/hmr/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/hmr`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `internal` | `const` | [`packages/boot/app-boot/src/index.ts:498`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L498) | `const internal = this.ctx.loader.internal` |
| `view` | `const` | [`packages/client/connection/src/client/fixture.ts:724`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L724) | `const view = presentCall(event.data.name, event.data.arguments)` |
| `view` | `const` | [`packages/client/connection/src/client/fixture.ts:735`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L735) | `const view = presentResult(candidate.data.name, candidate.data.arguments, resultText)` |
| `view` | `const` | [`packages/client/connection/src/client/fixture.ts:1200`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1200) | `const view = viewFor(event, log)` |
| `sessions` | `const` | [`packages/client/connection/src/client/fixture.ts:1515`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1515) | `const sessions: SessionSummary[] = options.empty ? [] : [` |
| `view` | `const` | [`packages/client/connection/src/client/fixture.ts:1686`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1686) | `const view = viewFor(event, log)` |
| `match` | `const` | [`packages/client/connection/src/client/fixture.ts:1747`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1747) | `const match = /^\/(\S+)((?:\s.*)?)$/.exec(line.trim())` |
| `match` | `const` | [`packages/client/connection/src/client/fixture.ts:2198`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L2198) | `const match = phraseMatch(document.tokens, query)` |
| `lastSeq` | `const` | [`packages/client/connection/src/client/fixture.ts:2322`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L2322) | `const lastSeq = log.at(-1)?.seq ?? -1` |
| `children` | `const` | [`packages/client/connection/src/client/fixture.ts:2534`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L2534) | `const children = childrenOf(target)` |
| `children` | `const` | [`packages/client/connection/src/client/fixture.ts:2550`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L2550) | `const children = childrenOf(parent)` |
| `view` | `const` | [`packages/client/connection/src/client/random-uuid.ts:9`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/random-uuid.ts#L9) | `const view = new DataView(bytes.buffer, bytes.byteOffset, bytes.byteLength)` |
| `inject` | `const` | [`packages/client/connection/src/index.ts:47`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/index.ts#L47) | `export const inject = ['webServer']` |
| `connection` | `const` | [`packages/client/connection/src/index.ts:138`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/index.ts#L138) | `const connection = new HostConnectionService(ctx, trustedHosts)` |
| `Context` | `interface` | [`packages/client/connection/src/rpc-host.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/rpc-host.ts#L36) | `interface Context {` |
| `inject` | `const` | [`packages/client/hmr/src/client/index.ts:76`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/client/index.ts#L76) | `export const inject = ['loader', 'modules']` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/client/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/client`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/client`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/client/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/client`.
- [`packages/core/session/tests/fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/fork.spec.ts) — A test under the owning area exercises or imports `lastSeq`.
- [`packages/core/session/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/scoped.spec.ts) — A test under the owning area exercises or imports `scopeOf`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `turnEnds`.
- [`packages/client/runtime/tests/fake-api.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/fake-api.client.ts) — A test under the owning area exercises or imports `IApiClient`.
- [`packages/client/hmr/tests/node-half.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/tests/node-half.client.spec.ts) — A test under the owning area exercises or imports `rebuilt`.

## How to read the implementation

1. Start with [`scripts/verify-client-domain-graph.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-client-domain-graph.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `internal`, `view`, `sessions`, `match`, `lastSeq`, `children`, `inject`, `connection`, `Context`, `loader`, `runtime`, `modules`, `client`, `json`
- Regex: `(?i)(internal|view|sessions|match|lastSeq|children|inject|connection)`

```bash
rg -n --pcre2 "(?i)(internal|view|sessions|match|lastSeq|children|inject|connection)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): The source note links to this decision directly.
- **`source-link`** — [0052. The slot system standard --- single register, four props shares, and the framework store seat](0052-the-slot-system-standard-single-register-four-props-shares-and-the-frame.md): The source note links to this decision directly.
- **`source-link`** — [0054. Client plugin loading --- plain packages, dsh.client plugins, and the two-phase boot](0054-client-plugin-loading-plain-packages-dsh-client-plugins-and-the-two-phas.md): The source note links to this decision directly.
- **`source-link`** — [0055. Toolview dissolution --- tool rows are per-view keyed slots](0055-toolview-dissolution-tool-rows-are-per-view-keyed-slots.md): The source note links to this decision directly.
- **`shares-code-with`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0113. Client Conversation business-node assembly and keyed Chat snapshots](0113-client-conversation-business-node-assembly-and-keyed-chat-snapshots.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0061. Web client Agent-scope parity model and the provisioning channel (agents/scope / blank reuse / provide)](0061-web-client-agent-scope-parity-model-and-the-provisioning-channel-agents.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0045-web-client-architecture-the-client-cordis-plugin-tree-the-slot-system-an.md`.
