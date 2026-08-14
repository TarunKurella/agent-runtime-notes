---
id: "dsh-note-0514"
title: "Cordis Host/Client Dynamic Plugin Runtime"
status: "proposed"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/architecture/2026-08-08-cordis-web-dynamic-packages.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/proposed"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "onEntryError"
  - "call"
  - "running"
  - "stack"
  - "timer"
  - "CordisDynamicPluginId"
  - "CordisDynamicPackageId"
  - "CordisDynamicPluginRunId"
  - "DynamicCordisRunnerService"
  - "pluginId"
  - "packageId"
  - "run"
  - "starting"
  - "handle"
search_regex: "(?i)(onEntryError|call|running|stack|timer|CordisDynamicPluginId|CordisDynamicPackageId|CordisDynamicPluginRunId)"
---

# 0514. Cordis Host/Client Dynamic Plugin Runtime — implementation context

## Open this when

The model needs to extend the current DSH process temporarily without modifying repository source, rebuilding the application, or refreshing the browser. An extension may run in the Host Node.js process, in a Client browser page, or as one plugin whose Host half retrieves data and whose Client half presents it. This capability cannot be limited to "execute some code." Before writing code, the model needs to discover the Services, Events, Builtins, Slots, and theme tokens available on both platforms. The user needs to preview the code before deciding whether Client code may enter the page.

## Source decision

The Host is the sole process-wide authority for Plugins, Packages, Runs, approvals, and version pointers. The Client stores only the current page's approval interaction, load results, Slot contributions, business views, and page-local errors. Define creates only immutable code versions; Run activates only a defined version. A version switch commits currentPackageId only after the target Package completes its required Host/Client activation. Before writing code, the model queries capabilities through Inspect Providers. Inspect results assist coding and are not plugin runtime business data.

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/architecture/2026-08-08-cordis-web-dynamic-packages.md](../02-notes/proposed/architecture/2026-08-08-cordis-web-dynamic-packages.md)
- Pinned source: [.agents/notes/proposed/architecture/2026-08-08-cordis-web-dynamic-packages.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/architecture/2026-08-08-cordis-web-dynamic-packages.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/timer/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/timer/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/timer`. Defines `timer`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`vendor/cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |
| [`vendor/cordis/src/logger.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/logger.ts) | runtime implementation | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |
| [`vendor/cordis/src/reflect.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/reflect.ts) | runtime implementation | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |
| [`vendor/cordis/src/registry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts) | runtime implementation | Core file in the package named by the note: `vendor/cordis`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/extensions/ui-cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/index.ts) | package entry point | Core file in the package named by the note: `packages/extensions/ui-cordis`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/extensions/tool-cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/src/index.ts) | package entry point | Core file in the package named by the note: `packages/extensions/tool-cordis`. Defines `pluginId`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/extensions/ui-cordis/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/extensions/ui-cordis`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/extensions/tool-cordis/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/extensions/tool-cordis`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/extensions/ui-cordis/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/index.ts) | package entry point | Core file in the package named by the note: `packages/extensions/ui-cordis`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/extensions/ui-cordis/src/client/slots.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/slots.ts) | runtime implementation | Core file in the package named by the note: `packages/extensions/ui-cordis`. Defines `SlotMap`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/extensions/ui-cordis/src/client/status.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/status.ts) | runtime implementation | Core file in the package named by the note: `packages/extensions/ui-cordis`. Defines `run`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `onEntryError` | `const` | [`packages/client/web-react/src/scoped-slots.tsx:714`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/scoped-slots.tsx#L714) | `const onEntryError = (error: unknown) => {` |
| `call` | `const` | [`packages/core/session/src/chunk-rows.ts:164`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts#L164) | `const call = toolCallOf(first)` |
| `running` | `const` | [`packages/extensions/cordis-client-runner/src/client/orchestrator.ts:313`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-client-runner/src/client/orchestrator.ts#L313) | `const running = this.inFlight.get(plan.pluginId)` |
| `stack` | `const` | [`packages/extensions/cordis-client-runner/src/client/runtime.ts:491`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-client-runner/src/client/runtime.ts#L491) | `const stack = 'stack' in error && typeof error.stack === 'string' ? error.stack : undefined` |
| `timer` | `const` | [`packages/extensions/cordis-client-runner/src/client/timer.ts:77`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-client-runner/src/client/timer.ts#L77) | `const timer = globalThis.setTimeout(() => {` |
| `CordisDynamicPluginId` | `function` | [`packages/extensions/cordis-host-runner/src/index.ts:49`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/index.ts#L49) | `export function CordisDynamicPluginId(id: string): CordisDynamicPluginId {` |
| `CordisDynamicPackageId` | `function` | [`packages/extensions/cordis-host-runner/src/index.ts:58`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/index.ts#L58) | `export function CordisDynamicPackageId(id: string): CordisDynamicPackageId {` |
| `CordisDynamicPluginRunId` | `function` | [`packages/extensions/cordis-host-runner/src/index.ts:67`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/index.ts#L67) | `export function CordisDynamicPluginRunId(id: string): CordisDynamicPluginRunId {` |
| `DynamicCordisRunnerService` | `class` | [`packages/extensions/cordis-host-runner/src/index.ts:124`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/index.ts#L124) | `export class DynamicCordisRunnerService extends TypertRemoteService {` |
| `pluginId` | `const` | [`packages/extensions/cordis-host-runner/src/index.ts:168`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/index.ts#L168) | `const pluginId = CordisDynamicPluginId(this.registry.mintPluginId(prefix))` |
| `packageId` | `const` | [`packages/extensions/cordis-host-runner/src/index.ts:185`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/index.ts#L185) | `const packageId = CordisDynamicPackageId(this.registry.mintPackageId())` |
| `run` | `const` | [`packages/extensions/cordis-host-runner/src/index.ts:391`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/index.ts#L391) | `const run = plugin.run` |
| `packageId` | `const` | [`packages/extensions/cordis-host-runner/src/index.ts:585`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/index.ts#L585) | `const packageId = plugin.nextPackageId` |
| `run` | `const` | [`packages/extensions/cordis-host-runner/src/index.ts:692`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/index.ts#L692) | `const run = plugin.run` |
| `run` | `const` | [`packages/extensions/cordis-host-runner/src/index.ts:725`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/index.ts#L725) | `const run = plugin?.run` |
| `run` | `const` | [`packages/extensions/cordis-host-runner/src/index.ts:751`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/index.ts#L751) | `const run = plugin.run` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/remote-model/packages/remote/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/remote-model/packages/remote/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/remote-model/packages/remote`.
- [`packages/typert/generator/tests/fixtures/remote-model/packages/remote/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/remote-model/packages/remote/src/types.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/remote-model/packages/remote`.
- [`packages/typert/generator/tests/fixtures/remote-model/packages/remote`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/remote-model/packages/remote) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/remote-model/packages/remote/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/remote-model/packages/remote/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/remote-model/packages/remote`.
- [`packages/extensions/cordis-host-runner/tests/helpers.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/tests/helpers.ts) — A test under the owning area exercises or imports `CordisDynamicPluginId`. A test under the owning area exercises or imports `pluginId`.
- [`packages/extensions/cordis-host-runner/tests/runner.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/tests/runner.spec.ts) — A test under the owning area exercises or imports `currentPackageId`. A test under the owning area exercises or imports `CordisDynamicPluginId`.
- [`packages/extensions/cordis-host-runner/tests/sandbox.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/tests/sandbox.spec.ts) — A test under the owning area exercises or imports `idPrefix`.
- [`packages/extensions/ui-cordis/tests/versioning.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/tests/versioning.client.spec.ts) — A test under the owning area exercises or imports `currentPackageId`. A test under the owning area exercises or imports `CordisDynamicPluginId`.
- Source verification intent: A new Plugin can be created only from a 3-to-6-character lowercase English prefix; final Plugin, Package, and Run IDs are allocated by the Host and use branded types. cordis_define validates only parameters and plain JavaScript syntax and returns an immutable Package; one Plugin can append versions while old source remains inspectable. cordis_run strictly validates run/update; Host-only activation completes synchronously, while a Client-bearing activation returns awaiting-approval or starting without waiting for the final browser outcome.

## How to read the implementation

1. Start with [`vendor/timer/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/timer/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/proposed`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `onEntryError`, `call`, `running`, `stack`, `timer`, `CordisDynamicPluginId`, `CordisDynamicPackageId`, `CordisDynamicPluginRunId`, `DynamicCordisRunnerService`, `pluginId`, `packageId`, `run`, `starting`, `handle`
- Regex: `(?i)(onEntryError|call|running|stack|timer|CordisDynamicPluginId|CordisDynamicPackageId|CordisDynamicPluginRunId)`

```bash
rg -n --pcre2 "(?i)(onEntryError|call|running|stack|timer|CordisDynamicPluginId|CordisDynamicPackageId|CordisDynamicPluginRunId)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares source implementation: `vendor/cordis/src/index.ts`, `vendor/cordis/src/logger.ts`.
- **`shares-code-with`** — [0121. Remote event delivery (ctx.remote.$on)](0121-remote-event-delivery-ctx-remote-on.md): Shares source implementation: `vendor/cordis/src/index.ts`, `vendor/cordis/src/logger.ts`.
- **`shares-code-with`** — [0010. Filesystem capability seam --- ctx.fs, local backend, and model-facing filesystem tools](0010-filesystem-capability-seam-ctx-fs-local-backend-and-model-facing-filesys.md): Shares source implementation: `vendor/cordis/src/index.ts`, `vendor/cordis/src/registry.ts`.
- **`shares-code-with`** — [0355. Broken presets are roster rows, not gaps](0355-broken-presets-are-roster-rows-not-gaps.md): Shares source implementation: `vendor/cordis/src/index.ts`, `vendor/cordis/src/logger.ts`.
- **`shares-code-with`** — [0561. Plan mode --- a logged per-agent session mode](0561-plan-mode-a-logged-per-agent-session-mode.md): Shares source implementation: `vendor/cordis/src/index.ts`.
- **`same-design-pressure`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`.
- **`shares-code-with`** — [0439. Private npm publication as three independent sequences](0439-private-npm-publication-as-three-independent-sequences.md): Shares source implementation: `vendor/cordis/src/index.ts`, `vendor/timer/src/index.ts`.
- **`shares-code-with`** — [0365. The preset-authoring agent mount-validates its own composition](0365-the-preset-authoring-agent-mount-validates-its-own-composition.md): Shares source implementation: `vendor/cordis/src/index.ts`, `vendor/cordis/src/registry.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0514-cordis-host-client-dynamic-plugin-runtime.md`.
