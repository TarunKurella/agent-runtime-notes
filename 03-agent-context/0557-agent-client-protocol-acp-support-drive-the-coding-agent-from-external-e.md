---
id: "dsh-note-0557"
title: "Agent Client Protocol (ACP) support --- drive the coding agent from external editors"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-06-14-acp-agent-client-protocol.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
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
  - "concern/simplification"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
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
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "agents"
  - "agentLoop"
  - "AgentHandle"
  - "SessionHeader"
  - "cwd"
  - "permission"
  - "custom"
  - "terminal"
  - "generic"
  - "model"
  - "presentCall"
  - "presentResult"
  - "initialize"
  - "diff"
search_regex: "(?i)(agents|agentLoop|AgentHandle|SessionHeader|permission|custom|terminal|generic)"
---

# 0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors — implementation context

## Open this when

The harness originally exposed agents only through a readline loop. That surface could carry text, but it gave an editor no structured way to create or resume sessions, correlate prompt completion, stream reasoning and tool activity, render tool-specific UI, ask for permission, or cancel one conversation without disturbing another. ACP defines those interactions as JSON-RPC over stdio, and Zed is the target client used to make concrete compatibility decisions. The bridge must preserve the harness's existing ownership boundaries.

## Source decision

@deepseek-ai/dsh-acp was a UI/client-driver plugin in the ui package group (it now lives in acp). It used @agentclientprotocol/sdk's AgentSideConnection over stdin/stdout and programmed only interface services: the agent create/resume factory, session persistence, tool registry, user interaction, and optional approval/bash capabilities. It did not change the agent loop and was not a capability-seam implementation. The bridge implements the following stable session path: initialize negotiates the protocol version, advertises text plus resource_link prompts, and advertises loadSession.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-06-14-acp-agent-client-protocol.md](../02-notes/archived/feature/2026-06-14-acp-agent-client-protocol.md)
- Pinned source: [.agents/notes/archived/feature/2026-06-14-acp-agent-client-protocol.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-06-14-acp-agent-client-protocol.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/acp/acp/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/acp/acp`. | `exact-code-occurrence, named-file, named-package-member` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Core file in the package named by the note: `packages/acp/acp`. Defines `agents`, a construct named by the note. | `exact-code-occurrence, named-package-member, symbol-definition` |
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. Defines `AgentHandle`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/acp/acp/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/acp/acp`. | `named-package-member` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/terminal/terminal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/index.ts) | package entry point | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/terminal/terminal/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/terminal/terminal/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/acp/acp`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/agent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/terminal/terminal`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `agents` | `const` | [`packages/acp/acp/src/index.ts:108`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L108) | `const agents = ctx.agents` |
| `agentLoop` | `const` | [`packages/client/ui-settings-plugins/src/client/index.ts:63`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/client/index.ts#L63) | `const agentLoop = new AgentLoopCardController(ctx.settingsScope.bind({ namespace: AGENT_LOOP_NS }))` |
| `AgentHandle` | `interface` | [`packages/core/agent/src/index.ts:172`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L172) | `export interface AgentHandle {` |
| `SessionHeader` | `interface` | [`packages/core/session/src/types.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L61) | `export interface SessionHeader {` |
| `cwd` | `const` | [`packages/fs/tool-fs/src/session-cwd.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts#L24) | `const cwd = exec.agent?.session.header.cwd` |
| `permission` | `const` | [`packages/hooks/hook-protocol/src/codec.ts:125`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/codec.ts#L125) | `const permission = permissionDecisionOf(str(hso, 'permissionDecision'))` |
| `custom` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:724`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L724) | `const custom = answer.custom?.trim()` |
| `terminal` | `const` | [`packages/jobs/jobs/src/invariant.ts:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/invariant.ts#L30) | `const terminal = TERMINAL_STATUSES.has(snapshot.status)` |
| `generic` | `const` | [`packages/session-query/tool-session-query/src/service-boundary.ts:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/service-boundary.ts#L121) | `const generic = genericFailure()` |
| `model` | `const` | [`packages/typert/loader/src/index.ts:109`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts#L109) | `const model = requireObject(pkgName, manifest.model, 'TYPERT.model')` |
| `presentCall` | `function` | [`packages/workflow/tool-ralph/src/index.ts:394`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts#L394) | `function presentCall(args: RalphCallArgs): ToolCallView {` |
| `presentResult` | `function` | [`packages/workflow/tool-ralph/src/index.ts:398`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts#L398) | `function presentResult(args: RalphCallArgs, result: { content: ContentBlock[]; isError: boolean }): ToolResultView {` |
| `initialize` | `def` | [`python/sdk/src/deepseek_harness/client.py:117`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/src/deepseek_harness/client.py#L117) | `def initialize(` |
| `diff` | `const` | [`vendor/loader/src/config/entry.ts:157`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/entry.ts#L157) | `const diff = Object` |

### Tests and executable evidence

- [`python/sdk/tests/test_client.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_client.py) — A test under the owning area exercises or imports `initialize`. Contains the exact code literal `session/prompt` named by the note.
- [`packages/acp/acp/tests/edges.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/edges.spec.ts) — A test under the owning area exercises or imports `initialize`.
- [`packages/acp/acp/tests/turns.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/turns.spec.ts) — A test under the owning area exercises or imports `initialize`. Contains the exact code literal `session/cancel` named by the note.
- [`packages/acp/acp/tests/bridge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/bridge.spec.ts) — A test under the owning area exercises or imports `initialize`. A test under the owning area exercises or imports `resource_link`.
- [`packages/acp/acp/tests/dispose.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/dispose.spec.ts) — A test under the owning area exercises or imports `initialize`.
- [`packages/acp/acp/tests/approval.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/approval.spec.ts) — A test under the owning area exercises or imports `initialize`.
- [`python/sdk/tests/test_bundled_runtime.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_bundled_runtime.py) — A test under the owning area exercises or imports `initialize`.
- [`packages/acp/acp/tests/multi-session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/multi-session.spec.ts) — A test under the owning area exercises or imports `initialize`.
- Source verification intent: The ACP suites cover the in-memory protocol codec, create/load replay, exact prompt settlement, cancellation races, unsupported content, tool presentation, terminal capability fallback, permission outcome mapping, config-option validation and persistence, multi-session isolation, disconnect/disposal quiescence, and HMR cleanup. Snapshot and built-bin tests exercise the app composition, while the real-API e2e self-skips without a key.

## How to read the implementation

1. Start with [`packages/acp/acp/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/README.md) because it has the strongest evidence link to the note.
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
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `agents`, `agentLoop`, `AgentHandle`, `SessionHeader`, `cwd`, `permission`, `custom`, `terminal`, `generic`, `model`, `presentCall`, `presentResult`, `initialize`, `diff`
- Regex: `(?i)(agents|agentLoop|AgentHandle|SessionHeader|permission|custom|terminal|generic)`

```bash
rg -n --pcre2 "(?i)(agents|agentLoop|AgentHandle|SessionHeader|permission|custom|terminal|generic)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0135. ACP subagent backend (out-of-process delegation)](0135-acp-subagent-backend-out-of-process-delegation.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/acp/acp/src/invariant.ts`.
- **`shares-code-with`** — [0144. The approval seam --- one-shot permission decisions over a waterfall of answerers](0144-the-approval-seam-one-shot-permission-decisions-over-a-waterfall-of-answ.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/acp/acp/src/invariant.ts`.
- **`shares-code-with`** — [0459. ACP as an automation-only protocol](0459-acp-as-an-automation-only-protocol.md): Shares source implementation: `packages/acp/acp`, `packages/acp/acp/README.md`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0265. Settlement delivery belongs to the continuation manager](0265-settlement-delivery-belongs-to-the-continuation-manager.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0159. Fresh-agent Ralph workflow tool](0159-fresh-agent-ralph-workflow-tool.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0035. Agent-scope runtime design and correctness](0035-agent-scope-runtime-design-and-correctness.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0015. The background job runtime (`ctx.jobs`) and generic task control tools](0015-the-background-job-runtime-ctx-jobs-and-generic-task-control-tools.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md`.
