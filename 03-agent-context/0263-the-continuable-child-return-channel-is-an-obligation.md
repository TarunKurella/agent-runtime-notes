---
id: "dsh-note-0263"
title: "The continuable child return channel is an obligation"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-06-continuable-child-report-obligation.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "systemPrompt"
  - "subagent"
  - "wakeup"
  - "inject"
  - "installReportTool"
  - "report"
  - "list_agents"
  - "reportDelivery"
  - "subagent-report"
  - "pinsChildSystemPrompts"
  - "pinsChildToolSchemas"
  - "system-prompt.<n>.expected.md"
  - "ctx.systemPrompt"
  - "The continuable child return channel is an obligation"
search_regex: "(?i)(systemPrompt|subagent|wakeup|inject|installReportTool|report|list_agents|reportDelivery)"
---

# 0263. The continuable child return channel is an obligation — implementation context

## Open this when

A continuable background child owns its own Session, so nothing it writes there reaches the agent that started it. The report tool gave that child a return channel and then presented it as one option among several: the schema said "call this zero or more times", nothing in the child's prompt asked it to call the tool at all, and the accepted default scheduling (quiet) added the report to a parked parent's next request without waking it. Each of those choices is defensible alone. Together they made the return channel unusable as a delegation contract.

## Source decision

The return channel is an instruction the child receives, not a capability it may discover. The report package installs two scope-local registrations into every continuable in-process child, and one disposer revokes both: the report tool, whose description now states that the child calls it once before finishing with a self-contained final result, and earlier for progress that changes what the parent should do next; a tool:report system-prompt section at order 117 carrying the same obligation in the child's own voice, so a child that never reads tool descriptions closely still receives it.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-06-continuable-child-report-obligation.md](../02-notes/implemented/feature/2026-08-06-continuable-child-report-obligation.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-06-continuable-child-report-obligation.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-06-continuable-child-report-obligation.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/subagent/subagent/src/child-agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/child-agent.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/subagent/subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/workflow/workflow/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow/src/index.ts) | package entry point | Core file in the package named by the note: `packages/workflow/workflow`. | `named-package-member` |
| [`packages/workflow/workflow/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/workflow/workflow`. | `named-package-member` |
| [`packages/subagent/subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/subagent`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/workflow/workflow/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/workflow/workflow`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/subagent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/workflow/workflow`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Defines `systemPrompt`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/code-mode.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts) | runtime implementation | Defines `wakeup`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/registry/src/service.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/src/service.ts) | runtime implementation | Defines `report`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `systemPrompt` | `const` | [`packages/boot/app-boot/src/index.ts:822`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L822) | `const systemPrompt = ctx.get('systemPrompt')` |
| `subagent` | `const` | [`packages/client/ui-subagent/src/client/index.ts:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-subagent/src/client/index.ts#L45) | `const subagent = owner.session?.subagent` |
| `wakeup` | `const` | [`packages/core/tools/src/code-mode.ts:381`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L381) | `const wakeup = (): void => {` |
| `inject` | `const` | [`packages/subagent/subagent/src/invariant.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/invariant.ts#L12) | `export const inject = ['invariants']` |
| `installReportTool` | `function` | [`packages/subagent/tool-subagent-report/src/index.ts:49`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-report/src/index.ts#L49) | `export function installReportTool(` |
| `report` | `const` | [`packages/typert/registry/src/service.ts:456`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/src/service.ts#L456) | `const report: ReportObserverError = (change, error) => {` |
| `inject` | `const` | [`packages/workflow/workflow/src/invariant.ts:17`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow/src/invariant.ts#L17) | `export const inject = ['invariants']` |

### Tests and executable evidence

- [`packages/subagent/tool-subagent-report/tests/tool-subagent-report.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-report/tests/tool-subagent-report.spec.ts) — A test under the owning area exercises or imports `installReportTool`.

## How to read the implementation

1. Start with [`packages/subagent/subagent/src/child-agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/child-agent.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/jobs-tasks`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `systemPrompt`, `subagent`, `wakeup`, `inject`, `installReportTool`, `report`, `list_agents`, `reportDelivery`, `subagent-report`, `pinsChildSystemPrompts`, `pinsChildToolSchemas`, `system-prompt.<n>.expected.md`, `ctx.systemPrompt`, `The continuable child return channel is an obligation`
- Regex: `(?i)(systemPrompt|subagent|wakeup|inject|installReportTool|report|list_agents|reportDelivery)`

```bash
rg -n --pcre2 "(?i)(systemPrompt|subagent|wakeup|inject|installReportTool|report|list_agents|reportDelivery)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0211. Continuable subagent report tool](0211-continuable-subagent-report-tool.md): The source note links to this decision directly.
- **`source-link`** — [0265. Settlement delivery belongs to the continuation manager](0265-settlement-delivery-belongs-to-the-continuation-manager.md): The source note links to this decision directly.
- **`shares-code-with`** — [0354. Code Mode collapses the executor, not just the wire](0354-code-mode-collapses-the-executor-not-just-the-wire.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/invariant.ts`.
- **`shares-code-with`** — [0049. Canonical tool output contract](0049-canonical-tool-output-contract.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/invariant.ts`.
- **`shares-code-with`** — [0285. Delegated subagents run with approvals pinned to `'never'`](0285-delegated-subagents-run-with-approvals-pinned-to-never.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/child-agent.ts`.
- **`shares-code-with`** — [0184. In-process subagent policy inheritance --- the child starts under the parent's sandbox override](0184-in-process-subagent-policy-inheritance-the-child-starts-under-the-parent.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/child-agent.ts`.
- **`shares-code-with`** — [0148. Background subagent tasks](0148-background-subagent-tasks.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0281. Continuable subagent policy inheritance --- the durable child log owns the delegation-time snapshot](0281-continuable-subagent-policy-inheritance-the-durable-child-log-owns-the-d.md): Shares source implementation: `packages/subagent/subagent/src/child-agent.ts`, `packages/subagent/subagent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0263-the-continuable-child-return-channel-is-an-obligation.md`.
