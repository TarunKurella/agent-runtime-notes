---
id: "dsh-note-0459"
title: "ACP as an automation-only protocol"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-07-23-acp-automation-only-protocol.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
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
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "@deepseek-ai/dsh-acp"
  - "packages/acp/acp"
  - "assistant/message"
  - "session/request_permission"
  - "dsh-subagent-acp"
  - "stream-json"
  - "session/new"
  - "_meta"
  - "terminal/create"
  - "ACP as an automation-only protocol"
  - "simplification"
  - "boundary"
  - "cancellation timeout"
  - "compatibility"
search_regex: "(?i)(@deepseek\\-ai/dsh\\-acp|packages/acp/acp|assistant/message|session/request_permission|dsh\\-subagent\\-acp|stream\\-json|session/new|_meta)"
---

# 0459. ACP as an automation-only protocol — implementation context

## Open this when

The ACP bridge had become a second interactive product UI. It translated durable events into editor cards, terminal metadata, diffs, plans, titles, reasoning, commands, modes, model and permission pickers, session navigation, and human elicitation. Those responsibilities duplicated the TUI and the Web client while coupling an automation transport to UI services, persistence queries, presentation policy, and editor-specific conventions.

## Source decision

@deepseek-ai/dsh-acp is an automation transport under packages/acp/acp, outside the ui package group. Its public protocol is intentionally small: version negotiation, fresh text sessions with one in-flight prompt each, committed assistant text updates, per-session cancellation, concurrent sessions, and connection-owned teardown. Prompts carry the spec-required baseline only --- text plus resource links flattened to bracketed textual references; the bridge rejects additional directories, MCP servers, beyond-baseline prompt content (image, audio, embedded resources), empty prompts, unknown sessions.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-07-23-acp-automation-only-protocol.md](../02-notes/implemented/simplification/2026-07-23-acp-automation-only-protocol.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-07-23-acp-automation-only-protocol.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-07-23-acp-automation-only-protocol.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/acp/acp/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/acp/acp`. The source note names this file directly. | `exact-code-occurrence, named-directory-member, named-file, named-package-member` |
| [`packages/subagent/subagent-acp/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/subagent/subagent-acp`. | `exact-code-occurrence, named-file, named-package-member` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/acp/acp`. Core file in the package named by the note: `packages/acp/acp`. | `exact-code-occurrence, named-directory-member, named-package-member` |
| [`packages/acp/acp/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/acp/acp`. Core file in the package named by the note: `packages/acp/acp`. | `named-directory-member, named-package-member` |
| [`packages/subagent/subagent-acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/subagent-acp`. | `named-package-member` |
| [`packages/subagent/subagent-acp/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/subagent-acp`. | `named-package-member` |
| [`packages/acp/acp/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/acp/acp`. Core file in the package named by the note: `packages/acp/acp`. | `named-directory-member, named-package-member` |
| [`packages/acp/acp`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp) | package or module directory | The source note names this implementation area directly. The note names this package or capability. | `named-directory, named-package` |
| [`packages/subagent/subagent-acp`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/subagent/subagent-acp/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/package.json) | composition and configuration | Core file in the package named by the note: `packages/subagent/subagent-acp`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `packages/acp/acp` named by the note. Contains the exact code literal `dsh-subagent-acp` named by the note. | `exact-code-occurrence` |
| [`tsconfig.host.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.host.json) | composition and configuration | Contains the exact code literal `packages/acp/acp` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`packages/acp/acp/tests/bridge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/bridge.spec.ts) — A test under the owning area exercises or imports `_meta`.
- [`packages/subagent/subagent-acp/tests/mock-acp-server.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/tests/mock-acp-server.ts) — A test under the owning area exercises or imports `dsh-subagent-acp`.
- [`packages/subagent/subagent-acp/tests/subagent-acp.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/tests/subagent-acp.spec.ts) — A test under the owning area exercises or imports `dsh-subagent-acp`.
- [`packages/shell/tool-bash/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/tests/tools.spec.ts) — Contains the exact code literal `session/new` named by the note.
- [`packages/test-support/acp-snapshot/tests/fixtures/fake-acp-agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/tests/fixtures/fake-acp-agent.ts) — Contains the exact code literal `session/request_permission` named by the note.

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

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/agent-loop`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `@deepseek-ai/dsh-acp`, `packages/acp/acp`, `assistant/message`, `session/request_permission`, `dsh-subagent-acp`, `stream-json`, `session/new`, `_meta`, `terminal/create`, `ACP as an automation-only protocol`, `simplification`, `boundary`, `cancellation timeout`, `compatibility`
- Regex: `(?i)(@deepseek\-ai/dsh\-acp|packages/acp/acp|assistant/message|session/request_permission|dsh\-subagent\-acp|stream\-json|session/new|_meta)`

```bash
rg -n --pcre2 "(?i)(@deepseek\\-ai/dsh\\-acp|packages/acp/acp|assistant/message|session/request_permission|dsh\\-subagent\\-acp|stream\\-json|session/new|_meta)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares source implementation: `packages/acp/acp`, `packages/acp/acp/README.md`.
- **`shares-code-with`** — [0135. ACP subagent backend (out-of-process delegation)](0135-acp-subagent-backend-out-of-process-delegation.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/acp/acp/src/invariant.ts`.
- **`shares-code-with`** — [0285. Delegated subagents run with approvals pinned to `'never'`](0285-delegated-subagents-run-with-approvals-pinned-to-never.md): Shares source implementation: `packages/subagent/subagent-acp`, `packages/subagent/subagent-acp/src/index.ts`.
- **`shares-code-with`** — [0661. Trim unreachable ACP bridge surface --- the branding knobs and the kind-sniffing fallback](0661-trim-unreachable-acp-bridge-surface-the-branding-knobs-and-the-kind-snif.md): Shares source implementation: `packages/acp/acp`, `packages/acp/acp/src/index.ts`.
- **`shares-code-with`** — [0144. The approval seam --- one-shot permission decisions over a waterfall of answerers](0144-the-approval-seam-one-shot-permission-decisions-over-a-waterfall-of-answ.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/acp/acp/src/invariant.ts`.
- **`shares-code-with`** — [0130. Multiplex concurrent ACP sessions over one connection](0130-multiplex-concurrent-acp-sessions-over-one-connection.md): Shares source implementation: `packages/acp/acp/README.md`, `packages/acp/acp/src/index.ts`.
- **`shares-code-with`** — [0068. The dispose ladder belongs to its consumer, not the subprocess seam](0068-the-dispose-ladder-belongs-to-its-consumer-not-the-subprocess-seam.md): Shares source implementation: `packages/subagent/subagent-acp`, `packages/subagent/subagent-acp/src/index.ts`.
- **`shares-code-with`** — [0026. Subagent provider-lifecycle events --- `subagent/provider-added` / `subagent/provider-removed`](0026-subagent-provider-lifecycle-events-subagent-provider-added-subagent-prov.md): Shares source implementation: `packages/acp/acp`, `packages/acp/acp/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0459-acp-as-an-automation-only-protocol.md`.
