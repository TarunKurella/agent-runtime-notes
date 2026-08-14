---
id: "dsh-note-0285"
title: "Delegated subagents run with approvals pinned to `'never'`"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-10-subagent-approval-pinned-never.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/trust"
  - "domain/build-release"
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
aliases:
  - "callId"
  - "permission"
  - "rejected"
  - "ApprovalService"
  - "running"
  - "applyChildComposition"
  - "captureDelegatedPolicyOverrides"
  - "appendDelegatedPolicyOverrides"
  - "idle"
  - "list_agents"
  - "dsh-subagent/src/child-agent.ts"
  - "approvalPolicy: 'never"
  - "approval/policy { policy: 'never', source: 'delegation' }"
  - "sandbox_permissions"
search_regex: "(?i)(callId|permission|rejected|ApprovalService|running|applyChildComposition|captureDelegatedPolicyOverrides|appendDelegatedPolicyOverrides)"
---

# 0285. Delegated subagents run with approvals pinned to `'never'` — implementation context

## Open this when

A delegated child that asked for approval had no one to ask. Under an interactive parent ('ask'), a background child's escalation became a pending question no product surface showed --- subagent sessions are omitted from the Web sidebar, the parent's list_agents reports plain running/idle, and the catalog rows show only activity --- so a permission-blocked child was indistinguishable from a working one; headless and unanswered compositions failed the same ask closed as 'unavailable'.

## Source decision

A delegated child acts only within the permission scope fixed at delegation, and approval prompts are removed from its world entirely: captureDelegatedPolicyOverrides(parent) (dsh-subagent/src/child-agent.ts) still snapshots the parent session's explicit sandbox override, but pins approvalPolicy: 'never' whenever the approval capability is composed --- it no longer reads the parent's own approval policy.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-10-subagent-approval-pinned-never.md](../02-notes/implemented/feature/2026-08-10-subagent-approval-pinned-never.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-10-subagent-approval-pinned-never.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-10-subagent-approval-pinned-never.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/subagent/subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent-acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/subagent-acp`. | `named-package-member` |
| [`packages/subagent/subagent/src/child-agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/child-agent.ts) | runtime implementation | Core file in the package named by the note: `packages/subagent/subagent`. Defines `captureDelegatedPolicyOverrides`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/subagent/src/continuation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/continuation.ts) | runtime implementation | Core file in the package named by the note: `packages/subagent/subagent`. Defines `idle`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/subagent-acp/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/subagent-acp`. | `named-package-member` |
| [`packages/subagent/subagent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/subagent/subagent-acp`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `callId`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/bash-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts) | package entry point | Defines `running`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `rejected`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `callId` | `const` | [`packages/core/tools/src/index.ts:1367`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1367) | `const callId = exec.callId` |
| `permission` | `const` | [`packages/hooks/hook-protocol/src/codec.ts:125`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/codec.ts#L125) | `const permission = permissionDecisionOf(str(hso, 'permissionDecision'))` |
| `rejected` | `let` | [`packages/host/apiproxy/src/api-proxy.ts:1768`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1768) | `let rejected = false` |
| `ApprovalService` | `class` | [`packages/interaction/user-approval/src/index.ts:192`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-approval/src/index.ts#L192) | `export class ApprovalService extends Service {` |
| `running` | `const` | [`packages/shell/bash-local/src/index.ts:257`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts#L257) | `const running = this.ctx.subprocess.spawn(this.spawnSpec(spec, argv, this.config.maxOutputBytes, spec.signal))` |
| `applyChildComposition` | `function` | [`packages/subagent/subagent/src/child-agent.ts:163`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/child-agent.ts#L163) | `export function applyChildComposition(` |
| `captureDelegatedPolicyOverrides` | `function` | [`packages/subagent/subagent/src/child-agent.ts:199`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/child-agent.ts#L199) | `export function captureDelegatedPolicyOverrides(parent: Agent): DelegatedPolicyOverrides {` |
| `appendDelegatedPolicyOverrides` | `function` | [`packages/subagent/subagent/src/child-agent.ts:215`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/child-agent.ts#L215) | `export function appendDelegatedPolicyOverrides(` |
| `idle` | `const` | [`packages/subagent/subagent/src/continuation.ts:1303`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/continuation.ts#L1303) | `const idle = activation.handle.agent.whenIdle()` |

### Tests and executable evidence

- [`packages/interaction/user-approval/tests/approval.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-approval/tests/approval.spec.ts) — A test under the owning area exercises or imports `ApprovalService`.
- [`packages/subagent/subagent-acp/tests/subagent-acp.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/tests/subagent-acp.spec.ts) — A test under the owning area exercises or imports `subagent-acp`.
- [`packages/subagent/subagent-acp/tests/loader-composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/tests/loader-composition.e2e.ts) — A test under the owning area exercises or imports `subagent-acp`.
- [`packages/subagent/subagent/tests/continuation-inheritance.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/continuation-inheritance.spec.ts) — A test under the owning area exercises or imports `ApprovalService`.
- [`apps/web/tests/settings-chrome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/settings-chrome.e2e.ts) — Contains the exact code literal `approval/policy` named by the note.
- [`apps/web/tests/approval-composer.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/approval-composer.e2e.ts) — Contains the exact code literal `approval/decided` named by the note.
- [`apps/web/tests/permission-policy-context.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/permission-policy-context.e2e.ts) — Contains the exact code literal `approval/asked` named by the note.
- [`apps/web/tests/snapshots/approval-composer/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/approval-composer/session.jsonl) — Contains the exact code literal `approval/asked` named by the note. Contains the exact code literal `approval/decided` named by the note.

## How to read the implementation

1. Start with [`packages/subagent/subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/trust`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`
- Aliases: `callId`, `permission`, `rejected`, `ApprovalService`, `running`, `applyChildComposition`, `captureDelegatedPolicyOverrides`, `appendDelegatedPolicyOverrides`, `idle`, `list_agents`, `dsh-subagent/src/child-agent.ts`, `approvalPolicy: 'never`, `approval/policy { policy: 'never', source: 'delegation' }`, `sandbox_permissions`
- Regex: `(?i)(callId|permission|rejected|ApprovalService|running|applyChildComposition|captureDelegatedPolicyOverrides|appendDelegatedPolicyOverrides)`

```bash
rg -n --pcre2 "(?i)(callId|permission|rejected|ApprovalService|running|applyChildComposition|captureDelegatedPolicyOverrides|appendDelegatedPolicyOverrides)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0144. The approval seam --- one-shot permission decisions over a waterfall of answerers](0144-the-approval-seam-one-shot-permission-decisions-over-a-waterfall-of-answ.md): The source note links to this decision directly.
- **`source-link`** — [0184. In-process subagent policy inheritance --- the child starts under the parent's sandbox override](0184-in-process-subagent-policy-inheritance-the-child-starts-under-the-parent.md): The source note links to this decision directly.
- **`shares-code-with`** — [0281. Continuable subagent policy inheritance --- the durable child log owns the delegation-time snapshot](0281-continuable-subagent-policy-inheritance-the-durable-child-log-owns-the-d.md): Shares source implementation: `packages/subagent/subagent/src/child-agent.ts`, `packages/subagent/subagent/src/continuation.ts`.
- **`shares-code-with`** — [0263. The continuable child return channel is an obligation](0263-the-continuable-child-return-channel-is-an-obligation.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/child-agent.ts`.
- **`shares-code-with`** — [0148. Background subagent tasks](0148-background-subagent-tasks.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/continuation.ts`.
- **`shares-code-with`** — [0106. Subagent list identity via the projection unit](0106-subagent-list-identity-via-the-projection-unit.md): Shares source implementation: `packages/subagent/subagent/src/continuation.ts`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0049. Canonical tool output contract](0049-canonical-tool-output-contract.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/invariant.ts`.
- **`shares-code-with`** — [0459. ACP as an automation-only protocol](0459-acp-as-an-automation-only-protocol.md): Shares source implementation: `packages/subagent/subagent-acp`, `packages/subagent/subagent-acp/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0285-delegated-subagents-run-with-approvals-pinned-to-never.md`.
