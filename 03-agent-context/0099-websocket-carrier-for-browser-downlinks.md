---
id: "dsh-note-0099"
title: "WebSocket carrier for browser downlinks"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-04-websocket-downlink-carrier.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "host"
  - "ConnectionController"
  - "WebApiClient"
  - "describe"
  - "MuxFrame"
  - "HostFrame"
  - "RpcRequest"
  - "ServerRequest"
  - "IApiClient"
  - "InProcessApiClient"
  - "respond"
  - "events.mux"
  - "events.host"
  - "/api/events.mux"
search_regex: "(?i)(host|ConnectionController|WebApiClient|describe|MuxFrame|HostFrame|RpcRequest|ServerRequest)"
---

# 0099. WebSocket carrier for browser downlinks — implementation context

## Open this when

The browser Web GUI has long used two SSE responses for events.mux and events.host. HTTP/1.1 browsers typically allow only about six concurrent connections per origin; each page permanently occupying two makes same-origin tabs, plugin resources, and ordinary RPCs contend for connection slots, and reaching the limit causes requests to queue rather than merely slowing them down. The RPC protocol itself is channel-independent: a constraint of the browser's physical carrier must not leak into the session/runtime object layer.

## Source decision

The real browser carrier opens one independent WebSocket for each downlink stream class: /api/events.mux sends only MuxFrame, and /api/events.host sends only HostFrame. Each text message is one complete ServerRequest JSON document; the client continues to validate the envelope first, then the concrete frame union for that path, and passes the narrow RpcRequest form to the existing ConnectionController. The streams retain independent lifecycles and provide no cross-stream ordering guarantee; either one ending still fails the entire connection generation and rebuilds it under the existing backoff policy.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-04-websocket-downlink-carrier.md](../02-notes/implemented/architecture/2026-08-04-websocket-downlink-carrier.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-04-websocket-downlink-carrier.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-04-websocket-downlink-carrier.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/host/webserver/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/webserver/src/index.ts) | package entry point | Core file in the package named by the note: `packages/host/webserver`. | `named-package-member` |
| [`packages/client/connection/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/connection`. | `named-package-member` |
| [`packages/host/webserver/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/webserver/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/host/webserver`. | `named-package-member` |
| [`packages/client/connection/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/connection`. | `named-package-member` |
| [`packages/client/connection/src/client/connection.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/connection.ts) | runtime implementation | Core file in the package named by the note: `packages/client/connection`. Defines `ConnectionController`, a construct named by the note. | `exact-code-occurrence, named-package-member, symbol-definition` |
| [`packages/client/connection/src/api-request-trust.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/api-request-trust.ts) | runtime implementation | Core file in the package named by the note: `packages/client/connection`. Defines `host`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/connection/src/client/web-api-client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/web-api-client.ts) | runtime implementation | Core file in the package named by the note: `packages/client/connection`. Defines `WebApiClient`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/host/webserver`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/host/webserver) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/connection`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Defines `describe`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api/rpc.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/rpc.ts) | runtime implementation | Defines `ServerRequest`, a construct named by the note. Defines `RpcRequest`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/events.ts) | runtime implementation | Defines `MuxFrame`, a construct named by the note. Defines `HostFrame`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `host` | `const` | [`packages/client/connection/src/api-request-trust.ts:104`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/api-request-trust.ts#L104) | `const host = header(request.headers, 'host')` |
| `ConnectionController` | `class` | [`packages/client/connection/src/client/connection.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/connection.ts#L61) | `export class ConnectionController {` |
| `WebApiClient` | `class` | [`packages/client/connection/src/client/web-api-client.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/web-api-client.ts#L13) | `export class WebApiClient extends AbstractApiClient {` |
| `describe` | `function` | [`packages/core/tools/src/py-types.ts:223`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L223) | `function describe(schema: object): string \| undefined {` |
| `MuxFrame` | `type` | [`packages/host/apiproxy/src/api/events.ts:69`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/events.ts#L69) | `export type MuxFrame =` |
| `HostFrame` | `type` | [`packages/host/apiproxy/src/api/events.ts:127`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/events.ts#L127) | `export type HostFrame =` |
| `RpcRequest` | `interface` | [`packages/host/apiproxy/src/api/rpc.ts:137`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/rpc.ts#L137) | `export interface RpcRequest<P> {` |
| `ServerRequest` | `interface` | [`packages/host/apiproxy/src/api/rpc.ts:171`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/rpc.ts#L171) | `export interface ServerRequest {` |
| `IApiClient` | `interface` | [`packages/host/apiproxy/src/fetch/client.ts:87`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/fetch/client.ts#L87) | `export interface IApiClient {` |
| `InProcessApiClient` | `class` | [`packages/host/apiproxy/src/fetch/client.ts:520`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/fetch/client.ts#L520) | `export class InProcessApiClient extends AbstractApiClient {` |
| `respond` | `def` | [`python/sdk/src/deepseek_harness/client.py:212`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/src/deepseek_harness/client.py#L212) | `def respond(self, request_id: str \| int, result: JsonValue) -> None:` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/host/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/host/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/host`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/host`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/host) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/host/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/host/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/host`.
- [`python/sdk/tests/test_client.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_client.py) — A test under the owning area exercises or imports `respond`.
- [`packages/host/webserver/tests/webserver.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/webserver/tests/webserver.spec.ts) — A test under the owning area exercises or imports `dsh-host-webserver`.
- [`packages/host/apiproxy/tests/fetch-carrier.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/fetch-carrier.spec.ts) — A test under the owning area exercises or imports `MuxFrame`. A test under the owning area exercises or imports `HostFrame`.
- [`packages/client/connection/tests/fake-api.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/tests/fake-api.client.ts) — A test under the owning area exercises or imports `mux`. A test under the owning area exercises or imports `MuxFrame`.
- [`packages/host/apiproxy/tests/client-handler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/client-handler.spec.ts) — A test under the owning area exercises or imports `MuxFrame`. A test under the owning area exercises or imports `HostFrame`.
- Source verification intent: Webserver contract tests pin upgrade-pathname dispatch, duplicate-registration rejection, disposal, and teardown; connection real-network tests pin each WebSocket's trust check, open, schema envelope, frame order, stream error, and close cancellation; client tests also prove that downlinks create ws:/wss: URLs while unary calls and respond still use HTTP fetch. The assembled keyless browser replay continues to cover Chromium, a real host, HTTP uplink, and the full WebSocket downlink chain.

## How to read the implementation

1. Start with [`packages/host/webserver/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/webserver/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/extensions`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/policy`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `host`, `ConnectionController`, `WebApiClient`, `describe`, `MuxFrame`, `HostFrame`, `RpcRequest`, `ServerRequest`, `IApiClient`, `InProcessApiClient`, `respond`, `events.mux`, `events.host`, `/api/events.mux`
- Regex: `(?i)(host|ConnectionController|WebApiClient|describe|MuxFrame|HostFrame|RpcRequest|ServerRequest)`

```bash
rg -n --pcre2 "(?i)(host|ConnectionController|WebApiClient|describe|MuxFrame|HostFrame|RpcRequest|ServerRequest)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0069. One carrier-level browser-trust boundary for all `/api` routes](0069-one-carrier-level-browser-trust-boundary-for-all-api-routes.md): Shares source implementation: `packages/client/connection`, `packages/client/connection/src/index.ts`.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/host/webserver/src/index.ts`, `packages/host/webserver/src/invariant.ts`.
- **`shares-code-with`** — [0114. An independent Events backstop closes the cordis-surface exhaustiveness gap](0114-an-independent-events-backstop-closes-the-cordis-surface-exhaustiveness.md): Shares source implementation: `packages/client/connection`, `packages/client/connection/src/index.ts`.
- **`shares-code-with`** — [0082. what the configuration plane exposes, and who may overwrite what](0082-what-the-configuration-plane-exposes-and-who-may-overwrite-what.md): Shares source implementation: `packages/client/connection/src/index.ts`, `packages/typert/generator/tests/fixtures/type-model/packages/host/src/index.ts`.
- **`shares-code-with`** — [0553. Parse `dsh` argv through one Commander adapter](0553-parse-dsh-argv-through-one-commander-adapter.md): Shares source implementation: `packages/host/webserver/src/index.ts`, `packages/host/webserver/src/invariant.ts`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`.
- **`same-design-pressure`** — [0092. Typert Gateway Targeted Method Calls](0092-typert-gateway-targeted-method-calls.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`.
- **`same-design-pressure`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares design concerns: `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0099-websocket-carrier-for-browser-downlinks.md`.
