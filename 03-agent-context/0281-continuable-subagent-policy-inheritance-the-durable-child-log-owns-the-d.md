---
id: "dsh-note-0281"
title: "Continuable subagent policy inheritance --- the durable child log owns the delegation-time snapshot"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-10-continuable-subagent-policy-inheritance.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/trust"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "get"
  - "captureDelegatedPolicyOverrides"
  - "appendDelegatedPolicyOverrides"
  - "SubagentContinuationManager"
  - "backgroundMode: continuable"
  - "danger-full-access"
  - "workspace-write"
  - "dsh-subagent/src/child-agent.ts"
  - "sandboxPolicy.overrideOf"
  - "ctx.get"
  - "startContinuable"
  - "prepareContinuable"
  - "MaterializeInputs.create"
  - "registerContinuableSetup"
search_regex: "(?i)(captureDelegatedPolicyOverrides|appendDelegatedPolicyOverrides|SubagentContinuationManager|backgroundMode:[- ]continuable|danger\\-full\\-access|workspace\\-write|dsh\\-subagent/src/child\\-agent\\.ts|sandboxPolicy\\.overrideOf)"
---

# 0281. Continuable subagent policy inheritance --- the durable child log owns the delegation-time snapshot — implementation context

## Open this when

The one-shot in-process driver has seeded parent sandbox/approval overrides into its children since the in-process policy-inheritance decision, but the continuable path never did: SubagentContinuationManager materialization applied only child composition and the activation setup registry. The default bundle wires both delegation tools as backgroundMode: continuable, so in a default deployment every background child silently fell back to deployment defaults --- a parent switched to danger-full-access produced children stuck at workspace-write whose every out-of-workspace operation raised an approval prompt.

## Source decision

The capture/append pair moved from the one-shot driver into the seam's shared child-agent module (dsh-subagent/src/child-agent.ts), the declared one home for shared child composition: captureDelegatedPolicyOverrides(parent) snapshots sandboxPolicy.overrideOf(parent.session) through optional ctx.get and pins the child approval policy to 'never' (approvals-pinned decision), and appendDelegatedPolicyOverrides(childSession, overrides) appends the source: 'delegation' events. The one-shot driver and the continuation manager both call them, so the two paths cannot drift.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-10-continuable-subagent-policy-inheritance.md](../02-notes/implemented/feature/2026-08-10-continuable-subagent-policy-inheritance.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-10-continuable-subagent-policy-inheritance.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-10-continuable-subagent-policy-inheritance.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Core file in the package named by the note: `packages/acp/acp`. | `named-package-member` |
| [`packages/acp/acp/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/acp/acp`. | `named-package-member` |
| [`packages/subagent/subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/sandbox/sandbox-policy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-policy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/sandbox/sandbox-policy`. | `named-package-member` |
| [`packages/subagent/subagent/src/child-agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/child-agent.ts) | runtime implementation | Core file in the package named by the note: `packages/subagent/subagent`. Defines `captureDelegatedPolicyOverrides`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/subagent/src/continuation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/continuation.ts) | runtime implementation | Core file in the package named by the note: `packages/subagent/subagent`. Defines `SubagentContinuationManager`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/interaction/user-approval/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-approval/src/index.ts) | package entry point | Core file in the package named by the note: `packages/interaction/user-approval`. | `named-package-member` |
| [`packages/interaction/user-approval/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-approval/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/interaction/user-approval`. | `named-package-member` |
| [`packages/sandbox/sandbox-policy/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-policy/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/sandbox/sandbox-policy`. | `named-package-member` |
| [`packages/interaction/user-approval/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-approval/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/interaction/user-approval`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `get` | `const` | [`packages/shell/tool-bash-persistent/src/index.ts:227`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash-persistent/src/index.ts#L227) | `const get = (owner: Agent, signal: AbortSignal): Promise<TerminalSessionId> => {` |
| `captureDelegatedPolicyOverrides` | `function` | [`packages/subagent/subagent/src/child-agent.ts:199`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/child-agent.ts#L199) | `export function captureDelegatedPolicyOverrides(parent: Agent): DelegatedPolicyOverrides {` |
| `appendDelegatedPolicyOverrides` | `function` | [`packages/subagent/subagent/src/child-agent.ts:215`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/child-agent.ts#L215) | `export function appendDelegatedPolicyOverrides(` |
| `SubagentContinuationManager` | `class` | [`packages/subagent/subagent/src/continuation.ts:349`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/continuation.ts#L349) | `export class SubagentContinuationManager {` |

### Tests and executable evidence

- [`packages/subagent/subagent/tests/continuation-inheritance.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/continuation-inheritance.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `startContinuable`.
- [`packages/acp/acp/tests/approval.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/approval.spec.ts) — A test under the owning area exercises or imports `dsh-user-approval`.
- [`packages/subagent/subagent/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/service.spec.ts) — A test under the owning area exercises or imports `startContinuable`.
- [`packages/subagent/subagent/tests/continuation.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/continuation.spec.ts) — A test under the owning area exercises or imports `startContinuable`. A test under the owning area exercises or imports `prepareContinuable`.
- [`packages/subagent/subagent/tests/list-children.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/list-children.spec.ts) — A test under the owning area exercises or imports `startContinuable`.
- [`packages/subagent/subagent-in-process-driver/tests/inheritance.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-in-process-driver/tests/inheritance.spec.ts) — A test under the owning area exercises or imports `firstLiveSeq`.
- [`apps/web/tests/shipped-composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/shipped-composition.e2e.ts) — Contains the exact code literal `dsh-sandbox-policy` named by the note. Contains the exact code literal `dsh-user-approval` named by the note.
- [`packages/workflow/tool-ralph/tests/integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/tests/integration.spec.ts) — Contains the exact code literal `dsh-subagent-in-process-driver` named by the note.

## How to read the implementation

1. Start with [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/trust`, `domain/build-release`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/storage`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `get`, `captureDelegatedPolicyOverrides`, `appendDelegatedPolicyOverrides`, `SubagentContinuationManager`, `backgroundMode: continuable`, `danger-full-access`, `workspace-write`, `dsh-subagent/src/child-agent.ts`, `sandboxPolicy.overrideOf`, `ctx.get`, `startContinuable`, `prepareContinuable`, `MaterializeInputs.create`, `registerContinuableSetup`
- Regex: `(?i)(captureDelegatedPolicyOverrides|appendDelegatedPolicyOverrides|SubagentContinuationManager|backgroundMode:[- ]continuable|danger\-full\-access|workspace\-write|dsh\-subagent/src/child\-agent\.ts|sandboxPolicy\.overrideOf)`

```bash
rg -n --pcre2 "(?i)(captureDelegatedPolicyOverrides|appendDelegatedPolicyOverrides|SubagentContinuationManager|backgroundMode:[- ]continuable|danger\\-full\\-access|workspace\\-write|dsh\\-subagent/src/child\\-agent\\.ts|sandboxPolicy\\.overrideOf)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0184. In-process subagent policy inheritance --- the child starts under the parent's sandbox override](0184-in-process-subagent-policy-inheritance-the-child-starts-under-the-parent.md): The source note links to this decision directly.
- **`source-link`** — [0285. Delegated subagents run with approvals pinned to `'never'`](0285-delegated-subagents-run-with-approvals-pinned-to-never.md): The source note links to this decision directly.
- **`shares-code-with`** — [0263. The continuable child return channel is an obligation](0263-the-continuable-child-return-channel-is-an-obligation.md): Shares source implementation: `packages/subagent/subagent/src/child-agent.ts`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0148. Background subagent tasks](0148-background-subagent-tasks.md): Shares source implementation: `packages/subagent/subagent/src/continuation.ts`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0106. Subagent list identity via the projection unit](0106-subagent-list-identity-via-the-projection-unit.md): Shares source implementation: `packages/subagent/subagent/src/continuation.ts`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0661. Trim unreachable ACP bridge surface --- the branding knobs and the kind-sniffing fallback](0661-trim-unreachable-acp-bridge-surface-the-branding-knobs-and-the-kind-snif.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/acp/acp/src/invariant.ts`.
- **`shares-code-with`** — [0173. Durable subagent catalog and list_agents](0173-durable-subagent-catalog-and-list-agents.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/invariant.ts`.
- **`shares-code-with`** — [0049. Canonical tool output contract](0049-canonical-tool-output-contract.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0281-continuable-subagent-policy-inheritance-the-durable-child-log-owns-the-d.md`.
