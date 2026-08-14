---
id: "dsh-note-0228"
title: "Code Mode language dispatch and the Python SDK renderer"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-31-code-mode-language-dispatch.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "language"
  - "errorClass"
  - "IDENTIFIER"
  - "code"
  - "constructor"
  - "prototype"
  - "mode"
  - "PYTHON_FLAVOR"
  - "CodeSdkLanguage"
  - "RUN_CODE_FLAVORS"
  - "resolveFlavor"
  - "SDK_RENDERERS"
  - "ToolRuntime"
  - "schemas"
search_regex: "(?i)(language|errorClass|IDENTIFIER|code|constructor|prototype|mode|PYTHON_FLAVOR)"
---

# 0228. Code Mode language dispatch and the Python SDK renderer — implementation context

## Open this when

Code Mode generated one SDK flavor: TypeScript. ToolRuntime hard-coded renderToolsSdk for the tools:sdk section and requireCodeRuntime rejected any ctx.codeRuntime.language !== 'typescript'. Adding a CPython backend means a program's source language is no longer fixed: the same visible tool registry must project a Python SDK when a Python runtime is loaded, and the model-facing run_code schema strings ("Execute a Python program …") must match the SDK section's language so the model never sees a TypeScript instruction over a Python runtime.

## Source decision

Language selection is a lookup on ctx.codeRuntime.language, resolved lazily at prompt assembly, against two parallel tables in dsh-tools: SDK_RENDERERS (index.ts) maps a language to its tools:sdk renderer --- typescript → renderToolsSdk, python → renderToolsSdkPy. The tools:sdk section reads the loaded runtime's language and picks the renderer; requireCodeRuntime rejects a mode: code/both runtime whose language is absent from the table, naming the known languages.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-31-code-mode-language-dispatch.md](../02-notes/implemented/feature/2026-07-31-code-mode-language-dispatch.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-31-code-mode-language-dispatch.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-31-code-mode-language-dispatch.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/subsystems/code-runtime.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/code-runtime.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `dsh-code-runtime` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/code-runtime/code-runtime/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/code-runtime/code-runtime`. | `named-file, named-package-member` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. Defines `ToolRuntime`, a construct named by the note. | `exact-code-occurrence, named-package-member, symbol-definition` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/ts-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/ts-types.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `renderToolsSdk`, a construct named by the note. | `exact-code-occurrence, named-package-member, symbol-definition` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `description`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/code-mode.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `RUN_CODE_FLAVORS`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/core/agent-loop/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `language` | `const` | [`packages/client/ui-primitives/src/markdown/render.tsx:301`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/render.tsx#L301) | `const language = node.lang ?? undefined` |
| `errorClass` | `const` | [`packages/code-runtime/code-runtime-worker-thread/src/bootstrap.ts:323`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/bootstrap.ts#L323) | `const errorClass = errorClasses.get(global)` |
| `errorClass` | `const` | [`packages/code-runtime/code-runtime-worker-thread/src/bootstrap.ts:393`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/bootstrap.ts#L393) | `const errorClass = errorClasses.get(namespace.global)` |
| `IDENTIFIER` | `const` | [`packages/code-runtime/code-runtime-worker-thread/src/index.ts:73`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/index.ts#L73) | `const IDENTIFIER = /^[A-Za-z_][A-Za-z0-9_]*$/` |
| `code` | `const` | [`packages/code-runtime/code-runtime-worker-thread/src/output-json.ts:69`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/output-json.ts#L69) | `const code = intrinsicReflectApply(intrinsicStringCharCodeAt, character, [0]) as number` |
| `constructor` | `const` | [`packages/code-runtime/code-runtime-worker-thread/src/worker-json.ts:85`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/worker-json.ts#L85) | `const constructor: unknown = descriptor?.value` |
| `prototype` | `const` | [`packages/code-runtime/code-runtime-worker-thread/src/worker-json.ts:103`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/worker-json.ts#L103) | `const prototype: unknown = intrinsicObjectGetPrototypeOf(value)` |
| `prototype` | `const` | [`packages/code-runtime/code-runtime-worker-thread/src/worker-json.ts:114`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/worker-json.ts#L114) | `const prototype: unknown = intrinsicObjectGetPrototypeOf(value)` |
| `mode` | `const` | [`packages/core/agent-loop/src/tool-calls.ts:88`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/tool-calls.ts#L88) | `const mode = ctx.tools.executionMode(first.exec).kind` |
| `PYTHON_FLAVOR` | `const` | [`packages/core/tools/src/code-mode.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L61) | `const PYTHON_FLAVOR: RunCodeFlavor = {` |
| `CodeSdkLanguage` | `type` | [`packages/core/tools/src/code-mode.ts:80`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L80) | `export type CodeSdkLanguage = 'typescript' \| 'python'` |
| `RUN_CODE_FLAVORS` | `const` | [`packages/core/tools/src/code-mode.ts:83`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L83) | `const RUN_CODE_FLAVORS: Record<string, RunCodeFlavor> = {` |
| `resolveFlavor` | `function` | [`packages/core/tools/src/code-mode.ts:113`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L113) | `function resolveFlavor(peekRuntime: () => CodeRuntime \| undefined): RunCodeFlavor {` |
| `mode` | `const` | [`packages/core/tools/src/code-mode.ts:419`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L419) | `const mode = head.classify()` |
| `SDK_RENDERERS` | `const` | [`packages/core/tools/src/index.ts:60`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L60) | `const SDK_RENDERERS: Record<string, (schemas: ToolSdkSchema[]) => string> = {` |
| `ToolRuntime` | `class` | [`packages/core/tools/src/index.ts:787`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L787) | `export class ToolRuntime extends Service {` |

### Tests and executable evidence

- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `ToolRuntime`. A test under the owning area exercises or imports `prototype`.
- [`packages/core/tools/tests/schema.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/schema.spec.ts) — A test under the owning area exercises or imports `hasOwn`. A test under the owning area exercises or imports `satisfies`.
- [`packages/core/tools/tests/ts-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/ts-types.spec.ts) — A test under the owning area exercises or imports `renderToolsSdk`. A test under the owning area exercises or imports `jsonSchemaToTs`.
- [`packages/core/tools/tests/py-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/py-types.spec.ts) — A test under the owning area exercises or imports `satisfies`. A test under the owning area exercises or imports `jsonSchemaToPy`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `ToolRuntime`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `requireCodeRuntime`. A test under the owning area exercises or imports `run_code`.
- [`packages/core/tools/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/invariant.spec.ts) — A test under the owning area exercises or imports `run_code`.
- [`packages/core/agent-loop/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/agent.spec.ts) — A test under the owning area exercises or imports `ToolRuntime`.

## How to read the implementation

1. Start with [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
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

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `language`, `errorClass`, `IDENTIFIER`, `code`, `constructor`, `prototype`, `mode`, `PYTHON_FLAVOR`, `CodeSdkLanguage`, `RUN_CODE_FLAVORS`, `resolveFlavor`, `SDK_RENDERERS`, `ToolRuntime`, `schemas`
- Regex: `(?i)(language|errorClass|IDENTIFIER|code|constructor|prototype|mode|PYTHON_FLAVOR)`

```bash
rg -n --pcre2 "(?i)(language|errorClass|IDENTIFIER|code|constructor|prototype|mode|PYTHON_FLAVOR)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0165. Typed tool returns in Code Mode](0165-typed-tool-returns-in-code-mode.md): Shares source implementation: `packages/core/tools/src/code-mode.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0138. dsh-hooks-claude-code + dsh-hooks-codex --- the Claude Code / Codex hook bridges](0138-dsh-hooks-claude-code-dsh-hooks-codex-the-claude-code-codex-hook-bridges.md): Shares source implementation: `packages/core/tools/src/code-mode.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0021. Event-domain semantics --- session is the fact log, agent is the live event channel](0021-event-domain-semantics-session-is-the-fact-log-agent-is-the-live-event-c.md): Shares source implementation: `AGENTS.md`, `packages/core/tools/src/code-mode.ts`.
- **`shares-code-with`** — [0189. Code Mode live dispatch lifecycle and native-contract parallelism](0189-code-mode-live-dispatch-lifecycle-and-native-contract-parallelism.md): Shares source implementation: `packages/core/tools/src/code-mode.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0032. Tool output spill policy](0032-tool-output-spill-policy.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0159. Fresh-agent Ralph workflow tool](0159-fresh-agent-ralph-workflow-tool.md): Shares source implementation: `packages/core/tools/src/code-mode.ts`, `packages/core/tools/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0228-code-mode-language-dispatch-and-the-python-sdk-renderer.md`.
