---
id: "dsh-note-0266"
title: "MCP client auto-reconnect with bounded backoff"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-06-mcp-client-auto-reconnect.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/protocols"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "apply"
  - "initialDelayMs"
  - "maxDelayMs"
  - "resolveReconnectPolicy"
  - "maxAttempts"
  - "isCurrent"
  - "reconnect"
  - "syncTools"
  - "packages/mcp/mcp-client/src/connection.ts"
  - "serverName"
  - "client.onclose"
  - "StreamableHTTPClientTransport"
  - "list_changed"
  - "failOnStartupError"
search_regex: "(?i)(apply|initialDelayMs|maxDelayMs|resolveReconnectPolicy|maxAttempts|isCurrent|reconnect|syncTools)"
---

# 0266. MCP client auto-reconnect with bounded backoff — implementation context

## Open this when

The MCP client connected once at plugin load. When a stdio server crashed or was killed, its registered tools stayed visible but every call failed with Not connected until a human edited the config (HMR) or restarted the Host --- v1 explicitly deferred reconnection. Long-running hosts (ACP automation, web) cannot be bounced because a child process died, and for stdio the harness composition is the only party that can respawn it. External feedback escalated this as a real operational gap (issue #1746).

## Source decision

packages/mcp/mcp-client/src/connection.ts owns a per-instance connection supervisor; apply() shrinks to config resolution plus two effects (the serverName reservation and the supervisor's lifecycle). The supervisor owns the client/transport generations, the live tool registrations, and the reconnect loop. Trigger. The supervisor arms client.onclose per generation. The SDK fires it when the stdio child exits, so a crash is observed without polling.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-06-mcp-client-auto-reconnect.md](../02-notes/implemented/feature/2026-08-06-mcp-client-auto-reconnect.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-06-mcp-client-auto-reconnect.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-06-mcp-client-auto-reconnect.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/mcp/mcp-client/src/connection.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/connection.ts) | runtime implementation | The source note names this file directly. Defines `isCurrent`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `apply`, a construct named by the note. | `symbol-definition` |
| [`packages/mcp/mcp-client/src/tools.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/tools.ts) | runtime implementation | Defines `syncTools`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/retry-policy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/retry-policy.ts) | runtime implementation | Defines `initialDelayMs`, a construct named by the note. Defines `maxDelayMs`, a construct named by the note. | `symbol-definition` |
| [`packages/mcp/mcp-client/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/index.ts) | package entry point | Defines `reconnect`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `apply` | `function` | [`packages/acp/acp/src/index.ts:105`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L105) | `export function apply(ctx: Context, config: AcpConfig): void {` |
| `initialDelayMs` | `const` | [`packages/llm/llm/src/retry-policy.ts:119`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/retry-policy.ts#L119) | `const initialDelayMs = config?.initialDelayMs ?? DEFAULT_INITIAL_DELAY_MS` |
| `maxDelayMs` | `const` | [`packages/llm/llm/src/retry-policy.ts:120`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/retry-policy.ts#L120) | `const maxDelayMs = config?.maxDelayMs ?? DEFAULT_MAX_DELAY_MS` |
| `resolveReconnectPolicy` | `function` | [`packages/mcp/mcp-client/src/connection.ts:65`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/connection.ts#L65) | `export function resolveReconnectPolicy(config: ReconnectConfig \| undefined, path: string): ResolvedReconnectPolicy {` |
| `maxAttempts` | `const` | [`packages/mcp/mcp-client/src/connection.ts:74`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/connection.ts#L74) | `const maxAttempts = config?.maxAttempts ?? RECONNECT_DEFAULTS.maxAttempts` |
| `isCurrent` | `const` | [`packages/mcp/mcp-client/src/connection.ts:153`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/connection.ts#L153) | `const isCurrent = (generation: Client): boolean => !disposed && client === generation` |
| `reconnect` | `const` | [`packages/mcp/mcp-client/src/index.ts:144`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/index.ts#L144) | `const reconnect = resolveReconnectPolicy(config.reconnect, \`mcp-client(${config.serverName}): reconnect\`)` |
| `syncTools` | `function` | [`packages/mcp/mcp-client/src/tools.ts:128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/tools.ts#L128) | `export async function syncTools(` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/client/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/client`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/client`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/client/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/client`.
- [`packages/llm/llm/tests/retry-policy.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/retry-policy.spec.ts) — A test under the owning area exercises or imports `initialDelayMs`. A test under the owning area exercises or imports `maxDelayMs`.
- [`packages/mcp/mcp-client/tests/apply.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/tests/apply.spec.ts) — A test under the owning area exercises or imports `maxAttempts`.
- [`packages/mcp/mcp-client/tests/mcp-client.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/tests/mcp-client.e2e.ts) — A test under the owning area exercises or imports `maxAttempts`.
- [`packages/mcp/mcp-client/tests/reconnect.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/tests/reconnect.spec.ts) — A test under the owning area exercises or imports `maxAttempts`. A test under the owning area exercises or imports `resolveReconnectPolicy`.
- [`packages/mcp/mcp-client/tests/mcp-client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/tests/mcp-client.spec.ts) — A test under the owning area exercises or imports `syncTools`.
- Source verification intent: Unit (tests/reconnect.spec.ts, mocked SDK): recovery swaps generations without duplication or leaks and serves post-recovery calls, diagnostics distinguish initial or retry failure from established connection loss, strict startup registration survives a pre-connect list_changed notification, failed initialization waits for the old generation's close signal and fails closed when that signal never arrives, disposal waits for the same signal with a bounded incomplete-shutdown path, the failure cap unregisters tools and stops, dispose cancels a pending backoff and quiesces an in-flight sync, a close after dispose.

## How to read the implementation

1. Start with [`packages/mcp/mcp-client/src/connection.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/connection.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/configuration`, `domain/extensions`, `domain/jobs-tasks`, `domain/llm`, `domain/protocols`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `apply`, `initialDelayMs`, `maxDelayMs`, `resolveReconnectPolicy`, `maxAttempts`, `isCurrent`, `reconnect`, `syncTools`, `packages/mcp/mcp-client/src/connection.ts`, `serverName`, `client.onclose`, `StreamableHTTPClientTransport`, `list_changed`, `failOnStartupError`
- Regex: `(?i)(apply|initialDelayMs|maxDelayMs|resolveReconnectPolicy|maxAttempts|isCurrent|reconnect|syncTools)`

```bash
rg -n --pcre2 "(?i)(apply|initialDelayMs|maxDelayMs|resolveReconnectPolicy|maxAttempts|isCurrent|reconnect|syncTools)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): The source note links to this decision directly.
- **`shares-code-with`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares source implementation: `packages/acp/acp/src/index.ts`.
- **`shares-code-with`** — [0144. The approval seam --- one-shot permission decisions over a waterfall of answerers](0144-the-approval-seam-one-shot-permission-decisions-over-a-waterfall-of-answ.md): Shares source implementation: `packages/acp/acp/src/index.ts`.
- **`shares-code-with`** — [0037. Provider-routed LLM adapters and a generic pi-ai backend](0037-provider-routed-llm-adapters-and-a-generic-pi-ai-backend.md): Shares source implementation: `packages/llm/llm/src/retry-policy.ts`.
- **`shares-code-with`** — [0234. Third-party memory MCP examples](0234-third-party-memory-mcp-examples.md): Shares source implementation: `packages/mcp/mcp-client/src/index.ts`.
- **`shares-code-with`** — [0200. Continuable subagents](0200-continuable-subagents.md): Shares source implementation: `packages/acp/acp/src/index.ts`.
- **`shares-code-with`** — [0135. ACP subagent backend (out-of-process delegation)](0135-acp-subagent-backend-out-of-process-delegation.md): Shares source implementation: `packages/acp/acp/src/index.ts`.
- **`shares-code-with`** — [0130. Multiplex concurrent ACP sessions over one connection](0130-multiplex-concurrent-acp-sessions-over-one-connection.md): Shares source implementation: `packages/acp/acp/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0266-mcp-client-auto-reconnect-with-bounded-backoff.md`.
