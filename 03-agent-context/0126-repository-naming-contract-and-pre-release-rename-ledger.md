---
id: "dsh-note-0126"
title: "Repository naming contract and pre-release rename ledger"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-11-repository-naming-contract-and-rename-ledger.md"
implementation_evidence: "medium"
target_anchor: "repository tests and release policy"
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
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "plugins"
  - "TypertGatewayService"
  - "run"
  - "LocaleRuntime"
  - "ClientModuleLoader"
  - "ClientModuleRegistry"
  - "SnapshotStore"
  - "schedule"
  - "workspaces"
  - "SessionRemotes"
  - "SessionRuntime"
  - "SlotRegistry"
  - "ctx"
  - "WorkspaceRuntime"
search_regex: "(?i)(plugins|TypertGatewayService|LocaleRuntime|ClientModuleLoader|ClientModuleRegistry|SnapshotStore|schedule|workspaces)"
---

# 0126. Repository naming contract and pre-release rename ledger — implementation context

## Open this when

The repository had grown faster than some names. Several package names described the first implementation instead of the capability. Several classes used Service even when they were registries, runtimes, engines, controllers, or resolvers. Some ctx keys were singular for registries and plural for one engine. Some provider names said local even though they used replaceable filesystem or subprocess services and could run in another execution world. These names are not harmless. A name tells a contributor where a responsibility starts and stops. Store suggests data access.

## Source decision

The repository uses every current name in this ledger. This decision changes names only; package responsibilities, service boundaries, behavior, defaults, and data models stay the same. A name that exposes a bad boundary requires a separate proposed Agent Note for that boundary change. Each renamed family has one vocabulary. Its directory, npm package name, imports, Cordis plugin name, ctx key, public types, directly coupled event or tool identifiers, configuration, tests, fixtures, examples, generated references, and current documentation use the current name where the ledger names those interfaces.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-11-repository-naming-contract-and-rename-ledger.md](../02-notes/implemented/architecture/2026-08-11-repository-naming-contract-and-rename-ledger.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-11-repository-naming-contract-and-rename-ledger.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-11-repository-naming-contract-and-rename-ledger.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/AGENTS.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `docs/cookbook/adding-a-package.md` named by the note. | `exact-code-occurrence, named-file` |
| [`docs/subsystems/jobs.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/jobs.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/subsystems/shell.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/shell.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/subsystems/terminal.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/terminal.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/cookbook/adding-a-package.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cookbook/adding-a-package.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/subsystems/user-questions.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/user-questions.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/subsystems/session-telemetry.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/session-telemetry.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `session-telemetry/record` named by the note. | `exact-code-occurrence, named-file` |
| [`docs/subsystems/permission-presets.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/permission-presets.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/e2b/e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/src/index.ts) | package entry point | Core file in the package named by the note: `packages/e2b/e2b`. Defines `Context`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. Defines `Context`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/lsp/lsp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp/src/index.ts) | package entry point | Core file in the package named by the note: `packages/lsp/lsp`. Defines `Context`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `plugins` | `const` | [`apps/cli/src/plugin.ts:63`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts#L63) | `const plugins = after.dsh?.profile?.bundles ?? []` |
| `TypertGatewayService` | `class` | [`packages/api/gateway/src/index.ts:90`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts#L90) | `export class TypertGatewayService extends Service implements TypertGateway {` |
| `run` | `function` | [`packages/bundle/headless/src/index.ts:96`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts#L96) | `async function run(ctx: Context, task: string, io: HeadlessIo): Promise<void> {` |
| `LocaleRuntime` | `class` | [`packages/client/locale/src/client/index.ts:114`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/src/client/index.ts#L114) | `export class LocaleRuntime {` |
| `ClientModuleLoader` | `interface` | [`packages/client/modules/src/client/manifest.ts:190`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/client/manifest.ts#L190) | `export interface ClientModuleLoader {` |
| `ClientModuleRegistry` | `class` | [`packages/client/modules/src/index.ts:184`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L184) | `export class ClientModuleRegistry extends Service {` |
| `SnapshotStore` | `interface` | [`packages/client/runtime/src/client/contract/store.ts:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/store.ts#L30) | `export interface SnapshotStore<T> extends ObservableSnapshot<T> {` |
| `schedule` | `const` | [`packages/client/runtime/src/client/contract/store.ts:58`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/store.ts#L58) | `const schedule: (fn: () => void) => void =` |
| `workspaces` | `const` | [`packages/client/runtime/src/client/index.ts:199`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/index.ts#L199) | `const workspaces = new WorkspaceRuntime(ctx, connection.api, sessions)` |
| `SessionRemotes` | `type` | [`packages/client/runtime/src/client/sessions/remotes.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/remotes.ts#L12) | `export type SessionRemotes = Pick<Context['remote'], 'commands'>` |
| `SessionRuntime` | `class` | [`packages/client/runtime/src/client/sessions/service.ts:229`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/service.ts#L229) | `export class SessionRuntime implements ISessions {` |
| `SlotRegistry` | `class` | [`packages/client/runtime/src/client/slots.ts:93`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/slots.ts#L93) | `export class SlotRegistry extends Service {` |
| `ctx` | `const` | [`packages/client/runtime/src/client/slots.ts:144`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/slots.ts#L144) | `const ctx = this.ctx` |
| `workspaces` | `const` | [`packages/client/runtime/src/client/slots.ts:392`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/slots.ts#L392) | `const workspaces = this.ctx.get('workspaces')` |
| `WorkspaceRuntime` | `class` | [`packages/client/runtime/src/client/workspaces/service.ts:51`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/workspaces/service.ts#L51) | `export class WorkspaceRuntime implements IWorkspaces {` |
| `workspace` | `const` | [`packages/client/runtime/src/client/workspaces/service.ts:90`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/workspaces/service.ts#L90) | `const workspace = this.list.getSnapshot().items.find(item => item.workspaceId === workspaceId)` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model`.
- [`apps/web/tests/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/README.md) — A test under the owning area exercises or imports `Context`. A test under the owning area exercises or imports `HTTP`.
- [`apps/web/tests/support.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/support.ts) — A test under the owning area exercises or imports `Context`. A test under the owning area exercises or imports `Directory`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `Context`. A test under the owning area exercises or imports `JSONL`.
- [`apps/web/tests/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/README.zh.md) — A test under the owning area exercises or imports `Context`. A test under the owning area exercises or imports `HTTP`.
- [`apps/web/tests/hmr-live.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/hmr-live.e2e.ts) — A test under the owning area exercises or imports `subprocess`. A test under the owning area exercises or imports `Context`.
- [`apps/web/tests/steering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/steering.e2e.ts) — A test under the owning area exercises or imports `ask_user_question`. A test under the owning area exercises or imports `Workspace`.
- Source verification intent: Every mapping in the ledger appears in the repository. Each family has one public vocabulary; no compatibility package, re-export alias, duplicate ctx key within one Cordis context, dual plugin id, dual event id, old tool alias, or fallback parser remains. Runtime behavior, package boundaries, defaults, policy, durable semantics, and model behavior remain equivalent except where an identifier is itself visible.

## How to read the implementation

1. Start with [`packages/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/AGENTS.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and release policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/jobs-tasks`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `plugins`, `TypertGatewayService`, `run`, `LocaleRuntime`, `ClientModuleLoader`, `ClientModuleRegistry`, `SnapshotStore`, `schedule`, `workspaces`, `SessionRemotes`, `SessionRuntime`, `SlotRegistry`, `ctx`, `WorkspaceRuntime`
- Regex: `(?i)(plugins|TypertGatewayService|LocaleRuntime|ClientModuleLoader|ClientModuleRegistry|SnapshotStore|schedule|workspaces)`

```bash
rg -n --pcre2 "(?i)(plugins|TypertGatewayService|LocaleRuntime|ClientModuleLoader|ClientModuleRegistry|SnapshotStore|schedule|workspaces)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0030. Tool-call timeout policy as a plugin](0030-tool-call-timeout-policy-as-a-plugin.md): The source note links to this decision directly.
- **`source-link`** — [0075. Regroup packages/ by measured clustering](0075-regroup-packages-by-measured-clustering.md): The source note links to this decision directly.
- **`source-link`** — [0490. Remove the SDK project toolchain](0490-remove-the-sdk-project-toolchain.md): The source note links to this decision directly.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `docs/subsystems/shell.md`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares source implementation: `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0015. The background job runtime (`ctx.jobs`) and generic task control tools](0015-the-background-job-runtime-ctx-jobs-and-generic-task-control-tools.md): Shares source implementation: `docs/subsystems/jobs.md`.
- **`shares-code-with`** — [0046. Meaningful package invariant contracts](0046-meaningful-package-invariant-contracts.md): Shares source implementation: `packages/llm/llm/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0126-repository-naming-contract-and-pre-release-rename-ledger.md`.
