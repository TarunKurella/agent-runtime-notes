---
id: "dsh-note-0195"
title: "TypeScript SDK client and the SDK subagent backend"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-27-typescript-sdk-and-sdk-subagent-backend.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/configuration"
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
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "code"
  - "notify"
  - "aborted"
  - "args"
  - "event"
  - "run"
  - "started"
  - "TurnEndReason"
  - "command"
  - "events"
  - "reason"
  - "provider"
  - "completed"
  - "finished"
search_regex: "(?i)(code|notify|aborted|args|event|started|TurnEndReason|command)"
---

# 0195. TypeScript SDK client and the SDK subagent backend — implementation context

## Open this when

The stdio JSON-RPC serving surface (@deepseek-ai/dsh-sdk-jsonrpc-server, the single-exe Agent Note) had exactly one client: the Python SDK. TypeScript consumers wanting the same drive-a-harness-as-a-subprocess capability --- repo tests, automation, and above all a subagent backend whose child is a complete harness runtime rather than a generic ACP agent --- had nothing to import: the request/notification payload shapes existed only as anonymous object literals inside the server, and the transport class lived inside the server plugin package.

## Source decision

Three packages, layered exactly like the existing Python stack, plus one Service Provider registration: @deepseek-ai/dsh-sdk-protocol (packages/sdk/protocol/) --- the wire made shared and nominal. JsonRpcLineTransport moves here verbatim from dsh-sdk-jsonrpc-server (which now imports it), and types.ts names every payload the server speaks: InitializeParams/Result, SessionPromptParams/Result, the four notification payloads, and the HarnessSdkRequestMap/HarnessSdkNotificationMap indexes. The package root explicitly exports that complete interface and provides no source-module deep imports.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-27-typescript-sdk-and-sdk-subagent-backend.md](../02-notes/implemented/feature/2026-07-27-typescript-sdk-and-sdk-subagent-backend.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-27-typescript-sdk-and-sdk-subagent-backend.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-27-typescript-sdk-and-sdk-subagent-backend.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `dsh-acp-snapshot` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/sdk/client/src/api.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/api.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/sdk/client`. Core file in the package named by the note: `packages/sdk/client`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/sdk/client/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/sdk/client`. Core file in the package named by the note: `packages/sdk/client`. | `exact-code-occurrence, named-directory-member, named-package-member` |
| [`packages/sdk/client/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/sdk/client`. Core file in the package named by the note: `packages/sdk/client`. | `exact-code-occurrence, named-directory-member, named-package-member` |
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. Defines `run`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/inbox.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/inbox.ts) | runtime implementation | Core file in the package named by the note: `packages/core/agent`. Defines `event`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/sdk/server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts) | package entry point | Core file in the package named by the note: `packages/sdk/server`. | `named-package-member` |
| [`packages/sdk/client/src/client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/client.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/sdk/client`. Core file in the package named by the note: `packages/sdk/client`. | `exact-code-occurrence, named-directory-member, named-package-member, symbol-definition` |
| [`packages/sdk/client/src/dispose.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/dispose.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/sdk/client`. Core file in the package named by the note: `packages/sdk/client`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/sdk/protocol/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/protocol/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/sdk/protocol`. Core file in the package named by the note: `packages/sdk/protocol`. | `named-directory-member, named-package-member` |
| [`packages/sdk/protocol/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/protocol/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/sdk/protocol`. Core file in the package named by the note: `packages/sdk/protocol`. | `named-directory-member, named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `code` | `const` | [`apps/cli/src/plugin.ts:135`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts#L135) | `const code = (result.error as NodeJS.ErrnoException).code` |
| `notify` | `const` | [`packages/acp/acp/src/index.ts:131`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L131) | `const notify = (notification: SessionNotification): void => {` |
| `aborted` | `const` | [`packages/core/agent-loop/src/index.ts:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L98) | `const aborted = Promise.withResolvers<never>()` |
| `args` | `const` | [`packages/core/agent/src/dispatch.ts:125`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/dispatch.ts#L125) | `const args: unknown[] = [carrier, name, fused(payload)]` |
| `event` | `const` | [`packages/core/agent/src/inbox.ts:186`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/inbox.ts#L186) | `const event = this.session.append('agent/inbox/spliced', splice)` |
| `args` | `const` | [`packages/core/agent/src/index.ts:529`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L529) | `const args: unknown[] = [entry.carrier, 'agent/disposed', { agent: entry.agent }]` |
| `args` | `const` | [`packages/core/agent/src/index.ts:561`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L561) | `const args: unknown[] = [entry.carrier, 'agent/created', { agent: entry.agent }]` |
| `run` | `const` | [`packages/core/agent/src/index.ts:642`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L642) | `const run: InitiatorRun = {` |
| `run` | `let` | [`packages/core/agent/src/index.ts:689`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L689) | `let run = this.initiatorRuns.getStore()` |
| `started` | `const` | [`packages/core/session/src/repair.ts:92`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts#L92) | `const started = callSeq !== undefined` |
| `TurnEndReason` | `type` | [`packages/core/session/src/types.ts:177`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L177) | `export type TurnEndReason = TurnEndReasonMap[keyof TurnEndReasonMap]` |
| `command` | `const` | [`packages/goal/command-goal/src/index.ts:111`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/command-goal/src/index.ts#L111) | `const command = parseGoalCommand(invocation.rawInput)` |
| `events` | `const` | [`packages/llm/llm-deepseek/src/sse.ts:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/sse.ts#L32) | `const events = stream` |
| `reason` | `const` | [`packages/llm/llm-deepseek/src/translate.ts:107`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/translate.ts#L107) | `const reason = pendingFinish ?? { kind: 'stop' as const }` |
| `provider` | `const` | [`packages/llm/llm/src/index.ts:632`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L632) | `const provider = registration.provider.id` |
| `completed` | `let` | [`packages/llm/llm/src/index.ts:872`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L872) | `let completed = false` |

### Tests and executable evidence

- [`examples/jsonrpc-agent/tests/sdk.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/jsonrpc-agent/tests/sdk.snapshot.ts) — The source note names this file directly.
- [`examples/jsonrpc-agent/tests/fixtures/subagent/subagent-dsh-sdk`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/jsonrpc-agent/tests/fixtures/subagent/subagent-dsh-sdk) — The source note names this implementation area directly.
- [`packages/core/session/tests/fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/fork.spec.ts) — A test under the owning area exercises or imports `TurnEndReason`.
- [`packages/sdk/server/tests/server.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/tests/server.spec.ts) — A test under the owning area exercises or imports `dsh-sdk-protocol`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `TurnEndReason`.
- [`packages/sdk/client/tests/sdk-client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/tests/sdk-client.spec.ts) — A test under the owning area exercises or imports `JsonRpcResponseError`. A test under the owning area exercises or imports `HarnessClient`.
- [`packages/sdk/protocol/tests/transport.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/protocol/tests/transport.spec.ts) — A test under the owning area exercises or imports `JsonRpcLineTransport`. A test under the owning area exercises or imports `JsonRpcResponseError`.
- [`packages/sdk/server/tests/plugin-apply.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/tests/plugin-apply.spec.ts) — A test under the owning area exercises or imports `dsh-sdk-jsonrpc-server`.
- Source verification intent: Four tiers, per testing policy: Keyless unit --- sdk-client drives a scripted fake runtime (tests/fake-runtime.ts, env-scripted, protocol-only --- the Python test_client.py pattern) over real stdio; subagent-dsh-sdk drives the same fake through the real provider. 100% per-file coverage on all three packages. Keyless Loader composition --- subagent-dsh-sdk/tests/loader-composition.e2e.ts boots a test-only cordis.yml (examples/jsonrpc-agent/tests/fixtures/subagent/subagent-dsh-sdk/) where the child is a REAL second harness runtime with its own cordis.yml; asserts the parent tool result and the child's own.

## How to read the implementation

1. Start with [`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.md) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `code`, `notify`, `aborted`, `args`, `event`, `run`, `started`, `TurnEndReason`, `command`, `events`, `reason`, `provider`, `completed`, `finished`
- Regex: `(?i)(code|notify|aborted|args|event|started|TurnEndReason|command)`

```bash
rg -n --pcre2 "(?i)(code|notify|aborted|args|event|started|TurnEndReason|command)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0034. Single-file executable SDK runtime distribution (single-exe)](0034-single-file-executable-sdk-runtime-distribution-single-exe.md): The source note links to this decision directly.
- **`shares-code-with`** — [0265. Settlement delivery belongs to the continuation manager](0265-settlement-delivery-belongs-to-the-continuation-manager.md): Shares source implementation: `packages/core/agent/src/inbox.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0092. Typert Gateway Targeted Method Calls](0092-typert-gateway-targeted-method-calls.md): Shares source implementation: `packages/core/agent/src/inbox.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0084. Follow-up enqueue and owned run boundaries](0084-follow-up-enqueue-and-owned-run-boundaries.md): Shares source implementation: `packages/core/agent/src/inbox.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0135. ACP subagent backend (out-of-process delegation)](0135-acp-subagent-backend-out-of-process-delegation.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0134. Subagent capability seam](0134-subagent-capability-seam.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0159. Fresh-agent Ralph workflow tool](0159-fresh-agent-ralph-workflow-tool.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0195-typescript-sdk-client-and-the-sdk-subagent-backend.md`.
