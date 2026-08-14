---
id: "dsh-note-0144"
title: "The approval seam --- one-shot permission decisions over a waterfall of answerers"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-06-approval-seam.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
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
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "signal"
  - "reason"
  - "allow"
  - "deny"
  - "callId"
  - "approval"
  - "decided"
  - "rejected"
  - "cancelled"
  - "ApprovalRequest"
  - "policy"
  - "CallId"
  - "request"
  - "toolName"
search_regex: "(?i)(signal|reason|allow|deny|callId|approval|decided|rejected)"
---

# 0144. The approval seam --- one-shot permission decisions over a waterfall of answerers — implementation context

## Open this when

Two callers need one closed decision --- "may this specific action proceed?": tools/pre-execute's ask decision (including the Claude-Code hook bridge's permissionDecision: ask) and the sandbox Agent Note's post-denial one-shot escalation retry. A shared seam keeps them from inventing separate outcome vocabularies, channel routing, cancellation, and audit trails, while guaranteeing that a deployment with no answerer can never grant an unanswerable request. The answerer may be an interactive host or an automated controller.

## Source decision

One package, dsh-user-approval (packages/interaction/user-approval), owns the vocabulary and the ctx.approval service --- the mechanism. The policy --- who answers, and whether a session is asked at all --- lives outside it: answerers are approval/request waterfall listeners registered by channel-owning plugins (the ACP bridge, host adapters, and test scripts), and a per-session policy tier can decide before a channel is involved. Consumers (dsh-tools' ask routing and the sandbox escalation gate) resolve a question to a closed outcome and derive their own tool results from it.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-06-approval-seam.md](../02-notes/implemented/feature/2026-07-06-approval-seam.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-06-approval-seam.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-06-approval-seam.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`examples/acp-agent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Core file in the package named by the note: `packages/acp/acp`. | `named-package-member` |
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. Defines `approval`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/acp/acp/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/acp/acp`. | `named-package-member` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/tools/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/code-mode.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `signal`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-bash`. Defines `policy`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/examples/acp-demo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/src/index.ts) | package entry point | Core file in the package named by the note: `packages/examples/acp-demo`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `signal` | `const` | [`packages/core/tools/src/code-mode.ts:401`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L401) | `const signal = new Promise<void>((resolve) => { wake = resolve })` |
| `reason` | `const` | [`packages/core/tools/src/index.ts:749`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L749) | `const reason = guard(exec)` |
| `allow` | `const` | [`packages/core/tools/src/index.ts:1076`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1076) | `const allow = filter.allow` |
| `deny` | `const` | [`packages/core/tools/src/index.ts:1077`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1077) | `const deny = filter.deny` |
| `reason` | `const` | [`packages/core/tools/src/index.ts:1124`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1124) | `const reason = layer.guardReason(exec)` |
| `callId` | `const` | [`packages/core/tools/src/index.ts:1367`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1367) | `const callId = exec.callId` |
| `signal` | `const` | [`packages/core/tools/src/index.ts:1372`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1372) | `const signal = exec.signal` |
| `signal` | `const` | [`packages/core/tools/src/index.ts:1538`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1538) | `const signal = fused.signal` |
| `approval` | `const` | [`packages/core/tools/src/index.ts:1693`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1693) | `const approval = this.ctx.get('approval')` |
| `reason` | `const` | [`packages/core/tools/src/index.ts:1901`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1901) | `const reason: unknown = source.reason` |
| `decided` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1437`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1437) | `const decided = new Set<ApprovalRequestId>()` |
| `rejected` | `let` | [`packages/host/apiproxy/src/api-proxy.ts:1768`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1768) | `let rejected = false` |
| `cancelled` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2037`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2037) | `const cancelled = () => err<{ items: SessionSearchItem[]; hasMore: boolean }>(request, {` |
| `ApprovalRequest` | `interface` | [`packages/interaction/user-approval/src/index.ts:153`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-approval/src/index.ts#L153) | `export interface ApprovalRequest {` |
| `policy` | `const` | [`packages/interaction/user-approval/src/index.ts:212`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-approval/src/index.ts#L212) | `const policy = effective(agent)` |
| `signal` | `const` | [`packages/interaction/user-approval/src/index.ts:305`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-approval/src/index.ts#L305) | `const signal = req.signal` |

### Tests and executable evidence

- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `dsh-user-approval`. A test under the owning area exercises or imports `allowed-once`.
- [`packages/acp/acp/tests/approval.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/approval.spec.ts) — A test under the owning area exercises or imports `dsh-user-approval`. A test under the owning area exercises or imports `allowed-once`.
- [`packages/core/tools/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/scoped.spec.ts) — A test under the owning area exercises or imports `prepend`. A test under the owning area exercises or imports `allow`.
- [`packages/core/tools/tests/ts-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/ts-types.spec.ts) — A test under the owning area exercises or imports `toolName`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `prepend`. A test under the owning area exercises or imports `toolName`.
- [`packages/core/tools/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/invariant.spec.ts) — A test under the owning area exercises or imports `allow`.
- [`packages/shell/tool-bash/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/tests/tools.spec.ts) — A test under the owning area exercises or imports `dsh-user-approval`. A test under the owning area exercises or imports `allowed-once`.
- [`packages/core/tools/tests/gen-tool-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/gen-tool-catalog.spec.ts) — A test under the owning area exercises or imports `toolName`. A test under the owning area exercises or imports `dsh-tool-bash`.

## How to read the implementation

1. Start with [`examples/acp-agent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/README.md) because it has the strongest evidence link to the note.
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

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `signal`, `reason`, `allow`, `deny`, `callId`, `approval`, `decided`, `rejected`, `cancelled`, `ApprovalRequest`, `policy`, `CallId`, `request`, `toolName`
- Regex: `(?i)(signal|reason|allow|deny|callId|approval|decided|rejected)`

```bash
rg -n --pcre2 "(?i)(signal|reason|allow|deny|callId|approval|decided|rejected)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0130. Multiplex concurrent ACP sessions over one connection](0130-multiplex-concurrent-acp-sessions-over-one-connection.md): The source note links to this decision directly.
- **`source-link`** — [0138. dsh-hooks-claude-code + dsh-hooks-codex --- the Claude Code / Codex hook bridges](0138-dsh-hooks-claude-code-dsh-hooks-codex-the-claude-code-codex-hook-bridges.md): The source note links to this decision directly.
- **`source-link`** — [0140. Interception extension points --- the typed-Decision surface a hook programs against](0140-interception-extension-points-the-typed-decision-surface-a-hook-programs.md): The source note links to this decision directly.
- **`source-link`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): The source note links to this decision directly.
- **`shares-code-with`** — [0159. Fresh-agent Ralph workflow tool](0159-fresh-agent-ralph-workflow-tool.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0021. Event-domain semantics --- session is the fact log, agent is the live event channel](0021-event-domain-semantics-session-is-the-fact-log-agent-is-the-live-event-c.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/acp/acp/src/invariant.ts`.
- **`shares-code-with`** — [0135. ACP subagent backend (out-of-process delegation)](0135-acp-subagent-backend-out-of-process-delegation.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/acp/acp/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0144-the-approval-seam-one-shot-permission-decisions-over-a-waterfall-of-answ.md`.
