---
id: "dsh-note-0054"
title: "Client plugin loading --- plain packages, dsh.client plugins, and the two-phase boot"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-23-client-plugin-loading-model.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "settled"
  - "inject"
  - "internal"
  - "assertEntriesLoaded"
  - "assertEntriesActivated"
  - "url"
  - "apiProxy"
  - "apply"
  - "modules"
  - "ClientModuleSystem"
  - "client"
  - "require"
  - "rev"
  - "scopeOf"
search_regex: "(?i)(settled|inject|internal|assertEntriesLoaded|assertEntriesActivated|apiProxy|apply|modules)"
---

# 0054. Client plugin loading --- plain packages, dsh.client plugins, and the two-phase boot — implementation context

## Open this when

On the host, cordis plugin loading stands on Node's module machinery --- the require cache and the internal ESM loader own module identity and bytes. The vendored @cordisjs/plugin-loader implements plugin governance and hot reload on top of that substrate, and the two meet at one boundary: Loader.internal. The browser client runs the same cordis plugin mechanism, so it needs the same substrate underneath --- and the browser has no Node module system. Conventional frontend engineering digests all dependencies at build time: one bundle, externals resolved by the bundler, nothing left to manage at runtime.

## Source decision

What makes a package a plugin? One rule: a package is a plugin package once its consumption is cordis dependency injection; until then it is a plain package. How code reaches the page is not part of the taxonomy --- arrival follows from the kind instead of defining it. Plain packages are the absolute base the module system itself needs, plus libraries not yet converted to DI: the react family, cordis, @deepseek-ai/dsh-client-modules (the module system itself --- it can never be a plugin, because modules precede all modules), the web shell kernel, and --- for now --- ui-slots, web-react, ui-primitives.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-23-client-plugin-loading-model.md](../02-notes/implemented/architecture/2026-07-23-client-plugin-loading-model.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-23-client-plugin-loading-model.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-23-client-plugin-loading-model.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/dev-web.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.ts) | repository automation | The source note names this file directly. Contains the exact code literal `lib/client.js` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/bundle/web-app/cordis.patch.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/cordis.patch.yml) | composition and configuration | The source note names this file directly. | `named-file` |
| [`packages/client/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/hmr`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/web/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/web`. | `named-package-member` |
| [`packages/api/gateway/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts) | package entry point | Core file in the package named by the note: `packages/api/gateway`. | `named-package-member` |
| [`packages/api/gateway/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/api/gateway`. | `named-package-member` |
| [`packages/client/web/src/AppRoot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/AppRoot.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/web`. Defines `settled`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/host/apiproxy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/host/apiproxy`. | `named-package-member` |
| [`packages/client/hmr/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/hmr`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/modules`. Defines `client`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/runtime/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/runtime`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/web/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/web`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `settled` | `const` | [`packages/api/gateway/src/client/index.ts:150`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts#L150) | `const settled: unknown = listener(...args as never[])` |
| `inject` | `const` | [`packages/api/gateway/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `internal` | `const` | [`packages/boot/app-boot/src/index.ts:498`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L498) | `const internal = this.ctx.loader.internal` |
| `assertEntriesLoaded` | `function` | [`packages/boot/app-boot/src/index.ts:658`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L658) | `export function assertEntriesLoaded(ctx: Context, binName: string): void {` |
| `assertEntriesActivated` | `function` | [`packages/boot/app-boot/src/index.ts:692`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L692) | `export async function assertEntriesActivated(ctx: Context, binName: string): Promise<void> {` |
| `url` | `const` | [`packages/client/connection/src/client/web-api-client.ts:40`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/web-api-client.ts#L40) | `const url = new URL(path, this.resolveBase())` |
| `apiProxy` | `const` | [`packages/client/connection/src/index.ts:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/index.ts#L156) | `const apiProxy = ctx.get('apiProxy')` |
| `inject` | `const` | [`packages/client/hmr/src/index.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts#L28) | `export const inject = ['clientModules', 'webServer']` |
| `apply` | `function` | [`packages/client/hmr/src/index.ts:57`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts#L57) | `export function apply(ctx: Context, config: Config): void {` |
| `inject` | `const` | [`packages/client/hmr/src/invariant.ts:14`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/invariant.ts#L14) | `export const inject = ['invariants']` |
| `apply` | `const` | [`packages/client/hmr/src/invariant.ts:58`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/invariant.ts#L58) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `modules` | `const` | [`packages/client/modules/src/client/index.ts:27`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/client/index.ts#L27) | `const modules = (globalThis as DshWindow).__DSH_MODULES__` |
| `modules` | `const` | [`packages/client/modules/src/client/manifest.ts:119`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/client/manifest.ts#L119) | `const modules: BootModuleRow[] = []` |
| `ClientModuleSystem` | `class` | [`packages/client/modules/src/client/system.ts:59`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/client/system.ts#L59) | `export class ClientModuleSystem implements ClientModuleLoader {` |
| `client` | `const` | [`packages/client/modules/src/index.ts:134`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L134) | `const client = (exportsField as Record<string, unknown>)['./client']` |
| `require` | `const` | [`packages/client/modules/src/index.ts:212`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L212) | `const require = createRequire(ctx.baseUrl)` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/client/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/client`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/client`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/client/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/client`.
- [`packages/boot/app-boot/tests/app-boot.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/app-boot.spec.ts) — A test under the owning area exercises or imports `assertEntriesLoaded`. A test under the owning area exercises or imports `assertEntriesActivated`.
- [`packages/client/web/tests/app.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/tests/app.client.spec.tsx) — A test under the owning area exercises or imports `dsh-client-web`.
- [`packages/host/apiproxy/tests/rpc-schemas.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/rpc-schemas.spec.ts) — A test under the owning area exercises or imports `transportError`.
- [`packages/client/runtime/tests/fake-api.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/fake-api.client.ts) — A test under the owning area exercises or imports `dsh-client-connection`.
- [`packages/client/hmr/tests/node-half.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/tests/node-half.client.spec.ts) — A test under the owning area exercises or imports `dsh-client-modules`. A test under the owning area exercises or imports `rev`.

## How to read the implementation

1. Start with [`scripts/dev-web.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `settled`, `inject`, `internal`, `assertEntriesLoaded`, `assertEntriesActivated`, `url`, `apiProxy`, `apply`, `modules`, `ClientModuleSystem`, `client`, `require`, `rev`, `scopeOf`
- Regex: `(?i)(settled|inject|internal|assertEntriesLoaded|assertEntriesActivated|apiProxy|apply|modules)`

```bash
rg -n --pcre2 "(?i)(settled|inject|internal|assertEntriesLoaded|assertEntriesActivated|apiProxy|apply|modules)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0045. Web client architecture --- the client cordis plugin tree, the slot system, and the React-free object layer](0045-web-client-architecture-the-client-cordis-plugin-tree-the-slot-system-an.md): The source note links to this decision directly.
- **`source-link`** — [0060. dsh web config-tree boot and the web transport layering](0060-dsh-web-config-tree-boot-and-the-web-transport-layering.md): The source note links to this decision directly.
- **`shares-code-with`** — [0092. Typert Gateway Targeted Method Calls](0092-typert-gateway-targeted-method-calls.md): Shares source implementation: `packages/api/gateway/src/index.ts`, `packages/api/gateway/src/types.ts`.
- **`shares-code-with`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): Shares source implementation: `packages/client/web/src/index.ts`.
- **`shares-code-with`** — [0197. Web past-session search](0197-web-past-session-search.md): Shares source implementation: `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0070. A capability-discriminated directory-picker seam for the web-GUI host](0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md): Shares source implementation: `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0093. A session's agent is composed from a preset cordis.yml](0093-a-session-s-agent-is-composed-from-a-preset-cordis-yml.md): Shares source implementation: `packages/api/gateway/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0054-client-plugin-loading-plain-packages-dsh-client-plugins-and-the-two-phas.md`.
