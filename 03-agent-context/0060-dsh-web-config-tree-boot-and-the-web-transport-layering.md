---
id: "dsh-note-0060"
title: "dsh web config-tree boot and the web transport layering"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-24-web-config-tree-boot-and-transport-layering.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "Context"
  - "installFailLoud"
  - "port"
  - "apiProxy"
  - "modules"
  - "BootManifest"
  - "client"
  - "ClientModuleRegistry"
  - "AppWebEntry"
  - "assembly"
  - "createApiProxy"
  - "toFetchHandler"
  - "ApiProxyService"
  - "distIndex"
search_regex: "(?i)(Context|installFailLoud|port|apiProxy|modules|BootManifest|client|ClientModuleRegistry)"
---

# 0060. dsh web config-tree boot and the web transport layering — implementation context

## Open this when

dsh web was the only hand-assembled surface left: bootHost mounted 32 plugins with configs pinned in code (violating no-hardcoded-tunables), the client roster was a web.ts constant, and TUI/headless had long been yml compositions. The transport layer misplaced responsibilities to match: the webserver self-described as a dumb carrier yet knew the DSH_BOOT graph, owned the SSE channel, and hard-coded the /api/ prefix; the dev bundle watch lived inside the prod registry behind a watch?

## Source decision

Composition is one flat assembled tree. apps/cli/config/base.cordis.yml plus apps/cli/config/web.cordis.yml holds every row --- the host runtime (32 rows), the api-gateway row, the webserver row, and the dsh.client rows (the browser roster; the modules row is simultaneously a host row). No spine bundle: every plugin is one row and every config field is yml-editable. That stance later became repository-wide, with the rows both surfaces share factored into apps/cli/config/base.cordis.yml and each surface reduced to an overlay (shared-base overlays).

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-24-web-config-tree-boot-and-transport-layering.md](../02-notes/implemented/architecture/2026-07-24-web-config-tree-boot-and-transport-layering.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-24-web-config-tree-boot-and-transport-layering.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-24-web-config-tree-boot-and-transport-layering.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/hmr`. | `named-package-member` |
| [`packages/api/gateway/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts) | package entry point | Core file in the package named by the note: `packages/api/gateway`. | `named-package-member` |
| [`packages/api/gateway/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/api/gateway`. Defines `Context`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/host/apiproxy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/host/apiproxy`. Defines `ApiProxyService`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/hmr/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/hmr`. | `named-package-member` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/modules`. Defines `client`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/host/webserver/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/webserver/src/index.ts) | package entry point | Core file in the package named by the note: `packages/host/webserver`. Defines `WebServer`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/api/gateway/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/api/gateway`. | `named-package-member` |
| [`packages/host/apiproxy/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/host/apiproxy`. | `named-package-member` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Core file in the package named by the note: `packages/host/apiproxy`. Defines `createApiProxy`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/api/gateway/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts) | package entry point | Core file in the package named by the note: `packages/api/gateway`. Defines `Context`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/modules/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/modules`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `Context` | `interface` | [`packages/api/gateway/src/client/index.ts:59`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts#L59) | `interface Context {` |
| `Context` | `interface` | [`packages/api/gateway/src/types.ts:50`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/types.ts#L50) | `interface Context {` |
| `installFailLoud` | `function` | [`packages/boot/app-boot/src/index.ts:609`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L609) | `export function installFailLoud(` |
| `port` | `const` | [`packages/bundle/web-app/src/index.ts:110`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/src/index.ts#L110) | `const port = ctx.get('webServer')?.port` |
| `apiProxy` | `const` | [`packages/client/connection/src/index.ts:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/index.ts#L156) | `const apiProxy = ctx.get('apiProxy')` |
| `modules` | `const` | [`packages/client/modules/src/client/index.ts:27`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/client/index.ts#L27) | `const modules = (globalThis as DshWindow).__DSH_MODULES__` |
| `BootManifest` | `interface` | [`packages/client/modules/src/client/manifest.ts:92`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/client/manifest.ts#L92) | `export interface BootManifest {` |
| `modules` | `const` | [`packages/client/modules/src/client/manifest.ts:119`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/client/manifest.ts#L119) | `const modules: BootModuleRow[] = []` |
| `Context` | `interface` | [`packages/client/modules/src/index.ts:40`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L40) | `interface Context {` |
| `client` | `const` | [`packages/client/modules/src/index.ts:134`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L134) | `const client = (exportsField as Record<string, unknown>)['./client']` |
| `ClientModuleRegistry` | `class` | [`packages/client/modules/src/index.ts:184`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L184) | `export class ClientModuleRegistry extends Service {` |
| `AppWebEntry` | `class` | [`packages/client/web/src/boot.tsx:68`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L68) | `export class AppWebEntry {` |
| `assembly` | `const` | [`packages/core/agent-loop/src/agent.ts:230`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L230) | `const assembly = await this.loopCtx.systemPrompt.assemble(assembleContextFor(this, signal))` |
| `createApiProxy` | `function` | [`packages/host/apiproxy/src/api-proxy.ts:1106`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1106) | `export function createApiProxy(ctx: Context, defaults: ApiProxyDefaults): ApiProxy {` |
| `toFetchHandler` | `function` | [`packages/host/apiproxy/src/fetch/handler.ts:243`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/fetch/handler.ts#L243) | `export function toFetchHandler(api: ApiProxy): { fetch: typeof fetch } {` |
| `Context` | `interface` | [`packages/host/apiproxy/src/index.ts:34`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/index.ts#L34) | `interface Context {` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/client/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/client`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/client`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/client/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/client`.
- [`packages/boot/app-boot/tests/app-boot.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/app-boot.spec.ts) — A test under the owning area exercises or imports `installFailLoud`.
- [`packages/host/webserver/tests/webserver.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/webserver/tests/webserver.spec.ts) — A test under the owning area exercises or imports `dsh-host-webserver`. A test under the owning area exercises or imports `webServer`.
- [`packages/api/gateway/tests/gateway.host.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/tests/gateway.host.spec.ts) — A test under the owning area exercises or imports `WebServer`. A test under the owning area exercises or imports `webServer`.
- [`packages/client/hmr/tests/node-half.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/tests/node-half.client.spec.ts) — A test under the owning area exercises or imports `dsh-host-webserver`. A test under the owning area exercises or imports `WebServer`.
- [`packages/host/apiproxy/tests/fetch-carrier.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/fetch-carrier.spec.ts) — A test under the owning area exercises or imports `toFetchHandler`.

## How to read the implementation

1. Start with [`packages/client/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/shell-terminal`, `domain/storage`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `Context`, `installFailLoud`, `port`, `apiProxy`, `modules`, `BootManifest`, `client`, `ClientModuleRegistry`, `AppWebEntry`, `assembly`, `createApiProxy`, `toFetchHandler`, `ApiProxyService`, `distIndex`
- Regex: `(?i)(Context|installFailLoud|port|apiProxy|modules|BootManifest|client|ClientModuleRegistry)`

```bash
rg -n --pcre2 "(?i)(Context|installFailLoud|port|apiProxy|modules|BootManifest|client|ClientModuleRegistry)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0054. Client plugin loading --- plain packages, dsh.client plugins, and the two-phase boot](0054-client-plugin-loading-plain-packages-dsh-client-plugins-and-the-two-phas.md): The source note links to this decision directly.
- **`source-link`** — [0115. headless is a direct core entry point](0115-headless-is-a-direct-core-entry-point.md): The source note links to this decision directly.
- **`source-link`** — [0467. One shared base config with per-surface overlays](0467-one-shared-base-config-with-per-surface-overlays.md): The source note links to this decision directly.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0105. Apps own their command line through `ctx.cmdlineArgs`](0105-apps-own-their-command-line-through-ctx-cmdlineargs.md): Shares source implementation: `packages/api/gateway/src/index.ts`, `packages/api/gateway/src/invariant.ts`.
- **`shares-code-with`** — [0197. Web past-session search](0197-web-past-session-search.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0070. A capability-discriminated directory-picker seam for the web-GUI host](0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0224. Web result card --- a structured render intent for web_search and web_fetch](0224-web-result-card-a-structured-render-intent-for-web-search-and-web-fetch.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0060-dsh-web-config-tree-boot-and-the-web-transport-layering.md`.
