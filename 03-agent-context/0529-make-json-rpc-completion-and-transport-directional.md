---
id: "dsh-note-0529"
title: "Make JSON-RPC completion and transport directional"
status: "proposed"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/simplification/2026-07-19-make-jsonrpc-directional.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/proposed"
  - "mechanism/policy"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "notify"
  - "request"
  - "aborted"
  - "started"
  - "TurnEndReason"
  - "reason"
  - "completed"
  - "finished"
  - "event"
  - "accepted"
  - "session_prompt"
  - "next_request"
  - "respond"
  - "respond_error"
search_regex: "(?i)(notify|request|aborted|started|TurnEndReason|reason|completed|finished)"
---

# 0529. Make JSON-RPC completion and transport directional — implementation context

## Open this when

The JSON-RPC bridge models both endpoints as symmetric peers although the shipped protocol is directional. The shared transport (now dsh-sdk-protocol, used by the server and by the TypeScript SDK client, which exercises the outbound-request/inbound-notification direction) still implements two halves no endpoint uses: server-originated requests and client-originated notifications. The Python SDK sends requests and receives responses or notifications, but it also queues unused inbound server requests and exposes response helpers. session/prompt also reports one settled turn through two protocol shapes.

## Source decision

Specialize each endpoint to its actual role. The server keeps inbound requests, outbound responses, and outbound notifications; the TypeScript and Python clients keep outbound requests and inbound responses or notifications. Delete the direction no endpoint uses --- server-originated requests and client-originated notifications. Return the settled outcome directly from session/prompt as { status, reason } after agent.whenIdle(). Delete session.finished, the constant acceptance response, and the Python post-response completion loop.

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/simplification/2026-07-19-make-jsonrpc-directional.md](../02-notes/proposed/simplification/2026-07-19-make-jsonrpc-directional.md)
- Pinned source: [.agents/notes/proposed/simplification/2026-07-19-make-jsonrpc-directional.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/simplification/2026-07-19-make-jsonrpc-directional.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/smoke-python-runtime.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/smoke-python-runtime.py) | repository automation | The source note names this file directly. Contains the exact code literal `session/prompt` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/sdk/server/src/server.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/server.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/sdk/protocol/src/transport.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/protocol/src/transport.ts) | runtime implementation | The source note names this file directly. Core file in the package named by the note: `packages/sdk/protocol`. | `named-file, named-package-member` |
| [`python/sdk/src/deepseek_harness/api.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/src/deepseek_harness/api.py) | runtime implementation | The source note names this file directly. | `named-file` |
| [`python/sdk/src/deepseek_harness/client.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/src/deepseek_harness/client.py) | runtime implementation | The source note names this file directly. Defines `next_request`, a construct named by the note. | `exact-code-occurrence, named-file, symbol-definition` |
| [`packages/sdk/protocol/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/protocol/src/index.ts) | package entry point | Core file in the package named by the note: `packages/sdk/protocol`. | `named-package-member` |
| [`packages/sdk/protocol/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/protocol/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/sdk/protocol`. | `named-package-member` |
| [`packages/sdk/protocol/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/protocol/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/sdk/protocol`. | `named-package-member` |
| [`packages/sdk/client/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/sdk/client`. | `named-directory-member` |
| [`packages/sdk/client/src/api.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/api.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/sdk/client`. Defines `event`, a construct named by the note. | `named-directory-member, symbol-definition` |
| [`packages/sdk/client/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/sdk/client`. | `named-directory-member` |
| [`packages/sdk/client/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/sdk/client`. | `named-directory-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `notify` | `const` | [`packages/acp/acp/src/index.ts:131`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L131) | `const notify = (notification: SessionNotification): void => {` |
| `request` | `const` | [`packages/core/agent-loop/src/agent.ts:486`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L486) | `const request = markAgentLoopRequest(deepFreeze({` |
| `aborted` | `const` | [`packages/core/agent-loop/src/index.ts:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L98) | `const aborted = Promise.withResolvers<never>()` |
| `started` | `const` | [`packages/core/session/src/repair.ts:92`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts#L92) | `const started = callSeq !== undefined` |
| `TurnEndReason` | `type` | [`packages/core/session/src/types.ts:177`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L177) | `export type TurnEndReason = TurnEndReasonMap[keyof TurnEndReasonMap]` |
| `reason` | `const` | [`packages/core/tools/src/index.ts:749`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L749) | `const reason = guard(exec)` |
| `completed` | `let` | [`packages/llm/llm/src/index.ts:872`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L872) | `let completed = false` |
| `finished` | `let` | [`packages/llm/llm/src/invariant.ts:42`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts#L42) | `let finished = false` |
| `event` | `const` | [`packages/sdk/client/src/api.ts:159`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/api.ts#L159) | `const event = validatedSessionEvent(notification.params.event)` |
| `accepted` | `let` | [`packages/sdk/client/src/dispose.ts:38`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/dispose.ts#L38) | `let accepted = false` |
| `session_prompt` | `def` | [`python/sdk/src/deepseek_harness/client.py:138`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/src/deepseek_harness/client.py#L138) | `def session_prompt(` |
| `next_request` | `def` | [`python/sdk/src/deepseek_harness/client.py:206`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/src/deepseek_harness/client.py#L206) | `def next_request(self) -> IncomingRequest:` |
| `respond` | `def` | [`python/sdk/src/deepseek_harness/client.py:212`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/src/deepseek_harness/client.py#L212) | `def respond(self, request_id: str \| int, result: JsonValue) -> None:` |
| `respond_error` | `def` | [`python/sdk/src/deepseek_harness/client.py:215`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/src/deepseek_harness/client.py#L215) | `def respond_error(` |
| `_request_raw` | `def` | [`python/sdk/src/deepseek_harness/client.py:228`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/src/deepseek_harness/client.py#L228) | `def _request_raw(` |
| `IncomingRequest` | `class` | [`python/sdk/src/deepseek_harness/models.py:20`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/src/deepseek_harness/models.py#L20) | `class IncomingRequest:` |

### Tests and executable evidence

- [`python/sdk/tests/test_client.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_client.py) — The source note names this file directly. A test under the owning area exercises or imports `next_request`.
- [`packages/sdk/protocol/tests/transport.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/protocol/tests/transport.spec.ts) — The source note names this file directly.
- [`packages/core/session/tests/fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/fork.spec.ts) — A test under the owning area exercises or imports `TurnEndReason`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `TurnEndReason`.
- Source verification intent: The TypeScript endpoint cannot originate requests or consume notifications. The Python endpoint cannot originate notifications or consume server requests. session/prompt returns the authoritative ok, error, or aborted outcome and reason after turn settlement. Session events and subagent lifecycle notifications emitted during the turn arrive before the response. Same-session overlap rejection, framing, multibyte input, handler errors, flush, shutdown ordering, and final-response reconstruction retain their behavior.

## How to read the implementation

1. Start with [`scripts/smoke-python-runtime.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/smoke-python-runtime.py) because it has the strongest evidence link to the note.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/agent-loop`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/proposed`, `mechanism/policy`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `notify`, `request`, `aborted`, `started`, `TurnEndReason`, `reason`, `completed`, `finished`, `event`, `accepted`, `session_prompt`, `next_request`, `respond`, `respond_error`
- Regex: `(?i)(notify|request|aborted|started|TurnEndReason|reason|completed|finished)`

```bash
rg -n --pcre2 "(?i)(notify|request|aborted|started|TurnEndReason|reason|completed|finished)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0075. Regroup packages/ by measured clustering](0075-regroup-packages-by-measured-clustering.md): Shares source implementation: `packages/sdk/protocol/src/index.ts`, `packages/sdk/protocol/src/types.ts`.
- **`shares-code-with`** — [0490. Remove the SDK project toolchain](0490-remove-the-sdk-project-toolchain.md): Shares source implementation: `packages/sdk/protocol/src/index.ts`, `packages/sdk/protocol/src/invariant.ts`.
- **`shares-code-with`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): Shares source implementation: `packages/sdk/server/src/server.ts`.
- **`same-design-pressure`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares design concerns: `concern/boundary`, `concern/concurrency`, `concern/evidence`.
- **`same-design-pressure`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares design concerns: `concern/boundary`, `concern/concurrency`, `concern/evidence`.
- **`same-design-pressure`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares design concerns: `concern/boundary`, `concern/concurrency`, `concern/evidence`.
- **`shares-code-with`** — [0203. SDK max output tokens](0203-sdk-max-output-tokens.md): Shares source implementation: `python/sdk/src/deepseek_harness/client.py`.
- **`shares-code-with`** — [0141. SessionStore fork API](0141-sessionstore-fork-api.md): Shares source implementation: `packages/sdk/server/src/server.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0529-make-json-rpc-completion-and-transport-directional.md`.
