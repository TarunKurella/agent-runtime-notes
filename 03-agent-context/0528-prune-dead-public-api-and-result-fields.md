---
id: "dsh-note-0528"
title: "Prune dead public API and result fields"
status: "proposed"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/simplification/2026-07-04-prune-dead-core-spine-api.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/proposed"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
aliases:
  - "AcpConfig"
  - "agents"
  - "agentOptions"
  - "level"
  - "summarize"
  - "language"
  - "BootstrapPort"
  - "PatchableStream"
  - "WorkerThreadCodeRuntime"
  - "resolveConfig"
  - "config"
  - "BasicCompactionEngine"
  - "summary"
  - "ReactLoopAgent"
search_regex: "(?i)(AcpConfig|agents|agentOptions|level|summarize|language|BootstrapPort|PatchableStream)"
---

# 0528. Prune dead public API and result fields — implementation context

## Open this when

Several package-root exports, result fields, and convenience methods have no production consumer. They survive because tests import internals through public entry points or because a type anticipated a caller that never arrived. Each item is small in isolation, but together they enlarge the SDK contract, generated catalogs, documentation, and regression matrix without enabling a shipped path. The production corpus is packages///src, example sources/config, and runtime scripts. Tests, package READMEs, and Agent Note prose are evidence of publication but not fixed callers.

## Source decision

Remove or demote every row as one bounded coordinated public-surface cleanup. Update package READMEs, JSDoc, generated API/event catalogs, type-equivalence records, exports maps where needed, and tests so they exercise the owning public contract instead of preserving test-only entry points. Do not collapse any capability seam, LLM adapter, persistence backend, or lifecycle quiescence contract.

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/simplification/2026-07-04-prune-dead-core-spine-api.md](../02-notes/proposed/simplification/2026-07-04-prune-dead-core-spine-api.md)
- Pinned source: [.agents/notes/proposed/simplification/2026-07-04-prune-dead-core-spine-api.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/simplification/2026-07-04-prune-dead-core-spine-api.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/extensions/tool-cordis/src/api-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/src/api-catalog.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/fs/tool-fs/src/edit.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/edit.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/tool-fs`. Defines `applyEditTool`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/tool-fs/src/read.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/tool-fs`. Defines `READ_LIMIT`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. Defines `run`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/fs/fs-local/src/fsio.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/fs-local`. Defines `applyLiteralEdit`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/tool-fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/tool-fs`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/tool-fs/src/write.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/write.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/tool-fs`. Defines `applyWriteTool`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/fs-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-local`. Defines `Config`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. Defines `source`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. Defines `SessionHeader`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/web/tool-web/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/index.ts) | package entry point | Core file in the package named by the note: `packages/web/tool-web`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `AcpConfig` | `interface` | [`packages/acp/acp/src/index.ts:70`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L70) | `export interface AcpConfig {` |
| `agents` | `const` | [`packages/acp/acp/src/index.ts:108`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L108) | `const agents = ctx.agents` |
| `agentOptions` | `function` | [`packages/acp/acp/src/index.ts:422`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L422) | `function agentOptions(config: AcpConfig): { provider?: string; model?: string } {` |
| `level` | `let` | [`packages/attachment/attachment-local/src/store.ts:105`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/attachment/attachment-local/src/store.ts#L105) | `let level = target` |
| `summarize` | `function` | [`packages/bundle/headless/src/index.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts#L61) | `function summarize(events: readonly SessionEvent[], firstSeq: number): RunOutcome {` |
| `language` | `const` | [`packages/client/ui-primitives/src/markdown/render.tsx:301`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/render.tsx#L301) | `const language = node.lang ?? undefined` |
| `BootstrapPort` | `interface` | [`packages/code-runtime/code-runtime-worker-thread/src/bootstrap.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/bootstrap.ts#L26) | `export interface BootstrapPort {` |
| `PatchableStream` | `interface` | [`packages/code-runtime/code-runtime-worker-thread/src/bootstrap.ts:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/bootstrap.ts#L37) | `export interface PatchableStream {` |
| `WorkerThreadCodeRuntime` | `class` | [`packages/code-runtime/code-runtime-worker-thread/src/index.ts:238`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/index.ts#L238) | `export class WorkerThreadCodeRuntime extends CodeRuntime {` |
| `resolveConfig` | `function` | [`packages/compaction/compaction-basic/src/config.ts:67`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/config.ts#L67) | `export function resolveConfig(config: BasicCompactionConfig = {}): ResolvedConfig {` |
| `config` | `const` | [`packages/compaction/compaction-basic/src/index.ts:55`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/index.ts#L55) | `const config = session.requestHeader()?.config` |
| `BasicCompactionEngine` | `class` | [`packages/compaction/compaction-basic/src/index.ts:103`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/index.ts#L103) | `export class BasicCompactionEngine extends CompactionEngine {` |
| `config` | `const` | [`packages/compaction/compaction-basic/src/index.ts:242`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/index.ts#L242) | `const config = target === undefined` |
| `summary` | `const` | [`packages/compaction/compaction-basic/src/summarizer.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/summarizer.ts#L169) | `const summary = summaryText(rawOutput)` |
| `ReactLoopAgent` | `class` | [`packages/core/agent-loop/src/agent.ts:64`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L64) | `export class ReactLoopAgent implements Agent {` |
| `run` | `const` | [`packages/core/agent/src/index.ts:642`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L642) | `const run: InitiatorRun = {` |

### Tests and executable evidence

- [`packages/acp/acp/tests/harness.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/harness.ts) — A test under the owning area exercises or imports `AcpConfig`.
- [`packages/acp/acp/tests/edges.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/edges.spec.ts) — A test under the owning area exercises or imports `agentOptions`.
- [`packages/fs/tool-fs/tests/diff.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/diff.spec.ts) — A test under the owning area exercises or imports `DIFF_CONTEXT`. A test under the owning area exercises or imports `computeHunkDiffs`.
- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `dispatches`.
- [`packages/core/agent/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/agent.spec.ts) — A test under the owning area exercises or imports `dispatches`.
- [`packages/fs/fs-local/tests/fsio.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/tests/fsio.spec.ts) — A test under the owning area exercises or imports `applyLiteralEdit`. A test under the owning area exercises or imports `listDirectory`.
- [`packages/fs/tool-fs/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/tools.spec.ts) — A test under the owning area exercises or imports `STREAM_MIN_SIZE`. A test under the owning area exercises or imports `parseEditArgs`.
- [`packages/core/tools/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/scoped.spec.ts) — A test under the owning area exercises or imports `ToolExecution`.
- Source verification intent: Exact-symbol searches show no removed API outside this Agent Note and any implemented-Agent Note amendments. Every API element listed in this Agent Note is absent or demoted as specified; deliberately retained extension/test contracts outside the inventory are unchanged. Tool execution, compaction, both LLM adapters, both persistence backends, workflow isolation, and agent creation/resume retain their shipped behavior. Typecheck, coverage, snapshots, doc-sync, module-graph verification, build, and hygiene pass.

## How to read the implementation

1. Start with [`packages/extensions/tool-cordis/src/api-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/src/api-catalog.ts) because it has the strongest evidence link to the note.
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
- **Status gate** — Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/proposed`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`
- Aliases: `AcpConfig`, `agents`, `agentOptions`, `level`, `summarize`, `language`, `BootstrapPort`, `PatchableStream`, `WorkerThreadCodeRuntime`, `resolveConfig`, `config`, `BasicCompactionEngine`, `summary`, `ReactLoopAgent`
- Regex: `(?i)(AcpConfig|agents|agentOptions|level|summarize|language|BootstrapPort|PatchableStream)`

```bash
rg -n --pcre2 "(?i)(AcpConfig|agents|agentOptions|level|summarize|language|BootstrapPort|PatchableStream)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0022. Resolve filesystem paths against the caller's session cwd](0022-resolve-filesystem-paths-against-the-caller-s-session-cwd.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0019. Make `dsh-fs-observation-policy` an event-gate plugin, not a method interface](0019-make-dsh-fs-observation-policy-an-event-gate-plugin-not-a-method-interfa.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/tool-fs/src/edit.ts`.
- **`shares-code-with`** — [0658. Prune write-only fields and a dead routing knob from the fs seam](0658-prune-write-only-fields-and-a-dead-routing-knob-from-the-fs-seam.md): Shares source implementation: `packages/fs/fs-local/src/fsio.ts`, `packages/fs/fs-local/src/index.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/fs/fs-local/src/fsio.ts`, `packages/fs/fs-local/src/index.ts`.
- **`shares-code-with`** — [0155. Cross-family file sandbox --- one policy home, a sandboxed fs provider, and fs escalation parity](0155-cross-family-file-sandbox-one-policy-home-a-sandboxed-fs-provider-and-fs.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/tool-fs/src/edit.ts`.
- **`shares-code-with`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0561. Plan mode --- a logged per-agent session mode](0561-plan-mode-a-logged-per-agent-session-mode.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0035. Agent-scope runtime design and correctness](0035-agent-scope-runtime-design-and-correctness.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0528-prune-dead-public-api-and-result-fields.md`.
