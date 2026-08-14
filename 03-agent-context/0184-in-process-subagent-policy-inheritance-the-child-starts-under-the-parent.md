---
id: "dsh-note-0184"
title: "In-process subagent policy inheritance --- the child starts under the parent's sandbox override"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-25-subagent-policy-inheritance.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "SessionHeader"
  - "callId"
  - "get"
  - "captureDelegatedPolicyOverrides"
  - "appendDelegatedPolicyOverrides"
  - "parentSession"
  - "read-only"
  - "sandboxPolicy.overrideOf"
  - "dsh-subagent"
  - "sandbox/mode"
  - "approval/policy"
  - "Session.firstLiveSeq"
  - "firstLiveSeq"
  - "SessionHeader.seedLength"
search_regex: "(?i)(SessionHeader|callId|captureDelegatedPolicyOverrides|appendDelegatedPolicyOverrides|parentSession|read\\-only|sandboxPolicy\\.overrideOf|dsh\\-subagent)"
---

# 0184. In-process subagent policy inheritance --- the child starts under the parent's sandbox override — implementation context

## Open this when

Sandbox and approval overrides are per-session log folds. An in-process subagent gets a new session, so a spawn child once fell back to deployment defaults and a fork child saw only switches inside its completed-turn prefix. Delegation could therefore widen a parent that had switched to read-only.

## Source decision

The delegation boundary snapshots sandboxPolicy.overrideOf(parent.session) before its first await, through the shared child-agent helpers (captureDelegatedPolicyOverrides/appendDelegatedPolicyOverrides in dsh-subagent), which the one-shot driver and the continuable start both call. A later parent switch belongs to the parent's future; cancel-and-redelegate takes a new snapshot. The sandbox-policy service is optional, and only the explicit session override is copied, never deployment defaults or one-shot grants.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-25-subagent-policy-inheritance.md](../02-notes/implemented/feature/2026-07-25-subagent-policy-inheritance.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-25-subagent-policy-inheritance.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-25-subagent-policy-inheritance.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/subagent/subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/src/child-agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/child-agent.ts) | runtime implementation | Core file in the package named by the note: `packages/subagent/subagent`. Defines `captureDelegatedPolicyOverrides`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/subagent/src/continuation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/continuation.ts) | runtime implementation | Core file in the package named by the note: `packages/subagent/subagent`. Defines `parentSession`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/subagent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `callId`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Defines `SessionHeader`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/tool-bash-persistent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash-persistent/src/index.ts) | package entry point | Defines `get`, a construct named by the note. | `symbol-definition` |
| [`packages/subagent/subagent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/README.md) | package contract and examples | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/package.json) | composition and configuration | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-subagent` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `SessionHeader` | `interface` | [`packages/core/session/src/types.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L61) | `export interface SessionHeader {` |
| `callId` | `const` | [`packages/core/tools/src/index.ts:1367`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1367) | `const callId = exec.callId` |
| `get` | `const` | [`packages/shell/tool-bash-persistent/src/index.ts:227`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash-persistent/src/index.ts#L227) | `const get = (owner: Agent, signal: AbortSignal): Promise<TerminalSessionId> => {` |
| `captureDelegatedPolicyOverrides` | `function` | [`packages/subagent/subagent/src/child-agent.ts:199`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/child-agent.ts#L199) | `export function captureDelegatedPolicyOverrides(parent: Agent): DelegatedPolicyOverrides {` |
| `appendDelegatedPolicyOverrides` | `function` | [`packages/subagent/subagent/src/child-agent.ts:215`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/child-agent.ts#L215) | `export function appendDelegatedPolicyOverrides(` |
| `parentSession` | `let` | [`packages/subagent/subagent/src/continuation.ts:822`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/continuation.ts#L822) | `let parentSession = agent.session.header.parentSession` |

### Tests and executable evidence

- [`apps/web/tests/settings-chrome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/settings-chrome.e2e.ts) — Contains the exact code literal `sandbox/mode` named by the note. Contains the exact code literal `approval/policy` named by the note.
- [`apps/web/tests/snapshots/approval-composer/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/approval-composer/session.jsonl) — Contains the exact code literal `approval/policy` named by the note.
- [`apps/web/tests/snapshots/goal-multi-turn-actions/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/goal-multi-turn-actions/session.jsonl) — Contains the exact code literal `approval/policy` named by the note.
- [`apps/web/tests/snapshots/permission-policy-context/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/permission-policy-context/session.jsonl) — Contains the exact code literal `approval/policy` named by the note.

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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/security`, `domain/session-state`, `domain/storage`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`
- Aliases: `SessionHeader`, `callId`, `get`, `captureDelegatedPolicyOverrides`, `appendDelegatedPolicyOverrides`, `parentSession`, `read-only`, `sandboxPolicy.overrideOf`, `dsh-subagent`, `sandbox/mode`, `approval/policy`, `Session.firstLiveSeq`, `firstLiveSeq`, `SessionHeader.seedLength`
- Regex: `(?i)(SessionHeader|callId|captureDelegatedPolicyOverrides|appendDelegatedPolicyOverrides|parentSession|read\-only|sandboxPolicy\.overrideOf|dsh\-subagent)`

```bash
rg -n --pcre2 "(?i)(SessionHeader|callId|captureDelegatedPolicyOverrides|appendDelegatedPolicyOverrides|parentSession|read\\-only|sandboxPolicy\\.overrideOf|dsh\\-subagent)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0144. The approval seam --- one-shot permission decisions over a waterfall of answerers](0144-the-approval-seam-one-shot-permission-decisions-over-a-waterfall-of-answ.md): The source note links to this decision directly.
- **`source-link`** — [0281. Continuable subagent policy inheritance --- the durable child log owns the delegation-time snapshot](0281-continuable-subagent-policy-inheritance-the-durable-child-log-owns-the-d.md): The source note links to this decision directly.
- **`source-link`** — [0285. Delegated subagents run with approvals pinned to `'never'`](0285-delegated-subagents-run-with-approvals-pinned-to-never.md): The source note links to this decision directly.
- **`shares-code-with`** — [0148. Background subagent tasks](0148-background-subagent-tasks.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/continuation.ts`.
- **`shares-code-with`** — [0263. The continuable child return channel is an obligation](0263-the-continuable-child-return-channel-is-an-obligation.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/child-agent.ts`.
- **`shares-code-with`** — [0106. Subagent list identity via the projection unit](0106-subagent-list-identity-via-the-projection-unit.md): Shares source implementation: `packages/subagent/subagent/src/continuation.ts`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0354. Code Mode collapses the executor, not just the wire](0354-code-mode-collapses-the-executor-not-just-the-wire.md): Shares source implementation: `packages/subagent/subagent/src/continuation.ts`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0362. One selection rule keeps subagent output past an empty terminal message](0362-one-selection-rule-keeps-subagent-output-past-an-empty-terminal-message.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0184-in-process-subagent-policy-inheritance-the-child-starts-under-the-parent.md`.
