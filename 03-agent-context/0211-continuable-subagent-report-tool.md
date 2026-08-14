---
id: "dsh-note-0211"
title: "Continuable subagent report tool"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-30-continuable-subagent-report-tool.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "AgentSetupCommit"
  - "wakeup"
  - "MessageId"
  - "parentSession"
  - "SubagentActivationSetupRegistry"
  - "toolFilter"
  - "SubagentRun"
  - "messageId"
  - "report"
  - "jsonl"
  - "output"
  - "@deepseek-ai/dsh-tool-subagent-report"
  - "{ messageId: string }"
  - "exec.agent"
search_regex: "(?i)(AgentSetupCommit|wakeup|MessageId|parentSession|SubagentActivationSetupRegistry|toolFilter|SubagentRun|report)"
---

# 0211. Continuable subagent report tool — implementation context

## Open this when

Continuable in-process subagents can receive later parent messages, retain descendants, settle, and cold-resume, but the base lifecycle gives them no way to send selected content back to their direct parent. Their complete output already remains reconstructable from the durable child Session, so the missing capability is explicit delivery rather than result storage. Treating every final assistant message as an implicit result would conflate turn completion with reporting.

## Source decision

Add the independently installed @deepseek-ai/dsh-tool-subagent-report package. It contributes an ordinary model-facing report tool to each continuable in-process child Activation. The mechanism accepts zero or multiple calls in a turn; the child is separately instructed to call it once before finishing (the report obligation). Success neither concludes the turn, settles the Activation, nor prevents later parent follow-ups, and finishing a turn never reports automatically. The feature is a collaboration control, not a result-bearing execution wrapper.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-30-continuable-subagent-report-tool.md](../02-notes/implemented/feature/2026-07-30-continuable-subagent-report-tool.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-30-continuable-subagent-report-tool.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-30-continuable-subagent-report-tool.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. Defines `AgentSetupCommit`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/subagent/tool-subagent-report/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-report/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/tool-subagent-report`. Defines `messageId`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/tool-subagent-control/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/tool-subagent-control`. Defines `messageId`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/tool-subagent-report/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-report/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/tool-subagent-report`. | `named-package-member` |
| [`packages/subagent/tool-subagent-control/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/tool-subagent-control`. | `named-package-member` |
| [`packages/subagent/tool-subagent-control/src/list-agents.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control/src/list-agents.ts) | runtime implementation | Core file in the package named by the note: `packages/subagent/tool-subagent-control`. | `named-package-member` |
| [`packages/core/agent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/subagent/tool-subagent-report`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-report) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/subagent/tool-subagent-control`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control) | package or module directory | The note names this package or capability. | `named-package` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Defines `jsonl`, a construct named by the note. Contains the exact code literal `tools/post-execute` named by the note. | `exact-code-occurrence, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `AgentSetupCommit` | `interface` | [`packages/core/agent/src/index.ts:56`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L56) | `export interface AgentSetupCommit {` |
| `wakeup` | `const` | [`packages/core/tools/src/code-mode.ts:381`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L381) | `const wakeup = (): void => {` |
| `MessageId` | `type` | [`packages/llm/llm/src/brand.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts#L16) | `export type MessageId = Branded<'MessageId'>` |
| `parentSession` | `const` | [`packages/sdk/server/src/server.ts:79`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/server.ts#L79) | `const parentSession = session.header.parentSession` |
| `SubagentActivationSetupRegistry` | `class` | [`packages/subagent/subagent/src/activation-setup-registry.ts:60`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/activation-setup-registry.ts#L60) | `export class SubagentActivationSetupRegistry {` |
| `toolFilter` | `const` | [`packages/subagent/subagent/src/descriptor.ts:235`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/descriptor.ts#L235) | `const toolFilter = Object.hasOwn(value, 'toolFilter')` |
| `SubagentRun` | `interface` | [`packages/subagent/subagent/src/types.ts:249`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts#L249) | `export interface SubagentRun {` |
| `messageId` | `const` | [`packages/subagent/tool-subagent-control/src/index.ts:66`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control/src/index.ts#L66) | `const messageId = await ctx.subagents.followup(` |
| `messageId` | `const` | [`packages/subagent/tool-subagent-report/src/index.ts:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-report/src/index.ts#L98) | `const messageId = await ctx.subagents.reportFrom(exec.agent as Agent, content, {` |
| `report` | `const` | [`packages/typert/registry/src/service.ts:456`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/src/service.ts#L456) | `const report: ReportObserverError = (change, error) => {` |
| `jsonl` | `const` | [`scripts/gen-doc-graphs.ts:731`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts#L731) | `const jsonl = nodeId('bundle', 'jsonl')` |
| `output` | `const` | [`vendor/cosmokit/src/string.ts:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cosmokit/src/string.ts#L23) | `const output: number[] = []` |

### Tests and executable evidence

- [`packages/subagent/subagent/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/service.spec.ts) — A test under the owning area exercises or imports `SubagentRun`.
- [`packages/subagent/tool-subagent-control/tests/list-agents.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control/tests/list-agents.spec.ts) — A test under the owning area exercises or imports `send_message`. A test under the owning area exercises or imports `list_agents`.
- [`packages/subagent/subagent/tests/activation-setup-registry.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/activation-setup-registry.spec.ts) — A test under the owning area exercises or imports `SubagentActivationSetupRegistry`.
- [`packages/subagent/tool-subagent-report/tests/tool-subagent-report.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-report/tests/tool-subagent-report.spec.ts) — A test under the owning area exercises or imports `toolFilter`. A test under the owning area exercises or imports `UNAUTHORIZED`.
- [`packages/subagent/tool-subagent-control/tests/tool-subagent-control.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control/tests/tool-subagent-control.spec.ts) — A test under the owning area exercises or imports `send_message`.

## How to read the implementation

1. Start with [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/jobs-tasks`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/tools`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`
- Aliases: `AgentSetupCommit`, `wakeup`, `MessageId`, `parentSession`, `SubagentActivationSetupRegistry`, `toolFilter`, `SubagentRun`, `messageId`, `report`, `jsonl`, `output`, `@deepseek-ai/dsh-tool-subagent-report`, `{ messageId: string }`, `exec.agent`
- Regex: `(?i)(AgentSetupCommit|wakeup|MessageId|parentSession|SubagentActivationSetupRegistry|toolFilter|SubagentRun|report)`

```bash
rg -n --pcre2 "(?i)(AgentSetupCommit|wakeup|MessageId|parentSession|SubagentActivationSetupRegistry|toolFilter|SubagentRun|report)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0263. The continuable child return channel is an obligation](0263-the-continuable-child-return-channel-is-an-obligation.md): The source note links to this decision directly.
- **`shares-code-with`** — [0264. Continuable subagent current-turn interrupt](0264-continuable-subagent-current-turn-interrupt.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0363. Bounded background job admission](0363-bounded-background-job-admission.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0150. Expose agent session identity and JSONL location to tools and hooks](0150-expose-agent-session-identity-and-jsonl-location-to-tools-and-hooks.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0180. Model-facing session query tools](0180-model-facing-session-query-tools.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0042. Explicit turn cancellation capability](0042-explicit-turn-cancellation-capability.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0211-continuable-subagent-report-tool.md`.
