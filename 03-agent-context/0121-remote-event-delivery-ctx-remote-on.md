---
id: "dsh-note-0121"
title: "Remote event delivery (ctx.remote.$on)"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-10-remote-event-delivery.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "ClientRemoteService"
  - "connection"
  - "args"
  - "TypertRemoteEventSelection"
  - "ConnectionController"
  - "flush"
  - "emitHost"
  - "rpc"
  - "WebApiClient"
  - "apiProxy"
  - "json"
  - "ISessions"
  - "sessions"
  - "INLINE_SAFE"
search_regex: "(?i)(ClientRemoteService|connection|args|TypertRemoteEventSelection|ConnectionController|flush|emitHost|WebApiClient)"
---

# 0121. Remote event delivery (ctx.remote.$on) — implementation context

## Open this when

Typert Gateway targeted method calls cover only the request/response shape and deliberately leave Session event streams and stateful interactions to separate designs. Every one-way Host-to-consumer push therefore still rides the legacy API Proxy. The Host owns a family of one-way events whose payloads are already JSON and whose emission never binds an AgentScope: agent-preset/selected, commands/change, credentials/updated, llm/adapters-updated, and settings/document-updated.

## Source decision

The consumer Remote surface carries one one-way subscription verb, ctx.remote.$on(event, listener), driven by an allowlist and forwarding verbatim: packages/api/remotes/src/remote-events.ts holds the allowlist of forwardable Host events, and it is the single control point over what a consumer may subscribe to. src/types.ts beside it derives the type projection and fills the selection seat, staying type-only per the package convention. Both files are listed in the files of both of this package's faces, so the Host forwarding loop and the consumer key surface read one declaration.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-10-remote-event-delivery.md](../02-notes/implemented/architecture/2026-08-10-remote-event-delivery.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-10-remote-event-delivery.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-10-remote-event-delivery.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/cli/tsconfig.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tsconfig.json) | composition and configuration | The source note names this file directly. | `named-file` |
| [`packages/client/tsdown.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/tsdown.client.ts) | runtime implementation | The source note names this file directly. Defines `CLIENT_EXTERNALS`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/api/remotes/src/remote-events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/src/remote-events.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`vendor/cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |
| [`vendor/cordis/src/fiber.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts) | runtime implementation | Core file in the package named by the note: `vendor/cordis`. Defines `effect`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`vendor/cordis/src/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts) | runtime implementation | Core file in the package named by the note: `vendor/cordis`. Defines `Events`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`vendor/cordis/src/logger.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/logger.ts) | runtime implementation | Core file in the package named by the note: `vendor/cordis`. Defines `args`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`vendor/cordis/src/reflect.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/reflect.ts) | runtime implementation | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. Defines `Events`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. Defines `Events`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`vendor/cordis/src/registry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts) | runtime implementation | Core file in the package named by the note: `vendor/cordis`. Defines `runtime`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/goal/goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts) | package entry point | Core file in the package named by the note: `packages/goal/goal`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ClientRemoteService` | `class` | [`packages/api/gateway/src/client/index.ts:88`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts#L88) | `class ClientRemoteService extends Service implements TypertClientRemote {` |
| `connection` | `const` | [`packages/api/gateway/src/client/index.ts:399`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts#L399) | `const connection = this.ownerCtx.get('connection') as ConnectionHandle \| undefined` |
| `args` | `const` | [`packages/api/gateway/src/index.ts:159`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts#L159) | `const args = await Promise.all(descriptor.parameters.map(parameter =>` |
| `TypertRemoteEventSelection` | `interface` | [`packages/api/remotes/src/types.ts:18`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/src/types.ts#L18) | `interface TypertRemoteEventSelection extends Record<ApiRemoteForwardedEvent, true> {}` |
| `ConnectionController` | `class` | [`packages/client/connection/src/client/connection.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/connection.ts#L61) | `export class ConnectionController {` |
| `flush` | `const` | [`packages/client/connection/src/client/fixture.ts:1264`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1264) | `const flush = (end: number): void => {` |
| `emitHost` | `const` | [`packages/client/connection/src/client/fixture.ts:1644`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1644) | `const emitHost = (frame: HostFrame): void => {` |
| `rpc` | `const` | [`packages/client/connection/src/client/fixture.ts:2998`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L2998) | `const rpc: ClientConnectionRpc = {` |
| `rpc` | `const` | [`packages/client/connection/src/client/index.ts:89`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/index.ts#L89) | `const rpc = fixtureClient?.rpc ?? createWebConnectionRpc()` |
| `WebApiClient` | `class` | [`packages/client/connection/src/client/web-api-client.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/web-api-client.ts#L13) | `export class WebApiClient extends AbstractApiClient {` |
| `connection` | `const` | [`packages/client/connection/src/index.ts:138`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/index.ts#L138) | `const connection = new HostConnectionService(ctx, trustedHosts)` |
| `apiProxy` | `const` | [`packages/client/connection/src/index.ts:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/index.ts#L156) | `const apiProxy = ctx.get('apiProxy')` |
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `ISessions` | `interface` | [`packages/client/runtime/src/client/contract/sessions.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/sessions.ts#L26) | `export interface ISessions {` |
| `flush` | `const` | [`packages/client/runtime/src/client/contract/store.ts:97`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/store.ts#L97) | `const flush = rafBatch(() => { for (const fn of [...listeners]) fn() })` |
| `connection` | `const` | [`packages/client/runtime/src/client/index.ts:194`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/index.ts#L194) | `const connection = ctx.get('connection') as ConnectionHandle` |

### Tests and executable evidence

- [`apps/web/tests/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/README.md) — Entry point or contract under the directory named by the note: `apps/web/tests`. The source note names this file directly.
- [`packages/typert/generator/tests/fixtures/remote-model/packages/remote/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/remote-model/packages/remote/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/remote-model/packages/remote`. A test under the owning area exercises or imports `dsh-typert-protocol`.
- [`packages/typert/generator/tests/fixtures/remote-model/packages/remote/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/remote-model/packages/remote/src/types.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/remote-model/packages/remote`.
- [`packages/typert/generator/tests/fixtures/remote-model/packages/remote`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/remote-model/packages/remote) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/remote-model/packages/remote/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/remote-model/packages/remote/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/remote-model/packages/remote`. Contains the exact code literal `src/types.ts` named by the note.
- [`packages/core/session/tests/json.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/json.spec.ts) — A test under the owning area exercises or imports `isJsonValue`. A test under the owning area exercises or imports `JsonValue`.
- [`packages/settings/settings/tests/memory.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/tests/memory.ts) — A test under the owning area exercises or imports `SettingsNamespace`.
- [`packages/core/session/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/invariant.spec.ts) — A test under the owning area exercises or imports `prepend`.
- Source verification intent: What pins this behavior: A real composition test puts one host/remote-event frame on the real host stream per Host emit, with event the Host name and args equal element for element. Type-level negatives reject three candidate classes: a name that is not an event, a Scope-bound event (goal/changed), and an event whose return is not void. $on('slots/changed', …) (Client-local) and $on('skills/change', …) (declared but unselected) both fail to compile, so $on's key surface equals the allowlist. On the consumer side, $on('settings/document-updated', …) resolves ns as SettingsNamespace: the brand survives the wire.

## How to read the implementation

1. Start with [`apps/cli/tsconfig.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tsconfig.json) because it has the strongest evidence link to the note.
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
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `ClientRemoteService`, `connection`, `args`, `TypertRemoteEventSelection`, `ConnectionController`, `flush`, `emitHost`, `rpc`, `WebApiClient`, `apiProxy`, `json`, `ISessions`, `sessions`, `INLINE_SAFE`
- Regex: `(?i)(ClientRemoteService|connection|args|TypertRemoteEventSelection|ConnectionController|flush|emitHost|WebApiClient)`

```bash
rg -n --pcre2 "(?i)(ClientRemoteService|connection|args|TypertRemoteEventSelection|ConnectionController|flush|emitHost|WebApiClient)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0092. Typert Gateway Targeted Method Calls](0092-typert-gateway-targeted-method-calls.md): The source note links to this decision directly.
- **`shares-code-with`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `vendor/cordis/src/index.ts`.
- **`shares-code-with`** — [0514. Cordis Host/Client Dynamic Plugin Runtime](0514-cordis-host-client-dynamic-plugin-runtime.md): Shares source implementation: `vendor/cordis/src/index.ts`, `vendor/cordis/src/logger.ts`.
- **`shares-code-with`** — [0093. A session's agent is composed from a preset cordis.yml](0093-a-session-s-agent-is-composed-from-a-preset-cordis-yml.md): Shares source implementation: `vendor/cordis/src/fiber.ts`, `vendor/cordis/src/index.ts`.
- **`shares-code-with`** — [0010. Filesystem capability seam --- ctx.fs, local backend, and model-facing filesystem tools](0010-filesystem-capability-seam-ctx-fs-local-backend-and-model-facing-filesys.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `vendor/cordis/src/index.ts`.
- **`shares-code-with`** — [0355. Broken presets are roster rows, not gaps](0355-broken-presets-are-roster-rows-not-gaps.md): Shares source implementation: `vendor/cordis/src/fiber.ts`, `vendor/cordis/src/index.ts`.
- **`shares-code-with`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares source implementation: `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0597. `dsh meta` boots the TUI over the harness checkout](0597-dsh-meta-boots-the-tui-over-the-harness-checkout.md): Shares source implementation: `vendor/cordis/src/fiber.ts`, `vendor/cordis/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0121-remote-event-delivery-ctx-remote-on.md`.
