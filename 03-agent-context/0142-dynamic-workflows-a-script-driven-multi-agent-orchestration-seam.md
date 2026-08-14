---
id: "dsh-note-0142"
title: "Dynamic workflows --- a script-driven multi-agent orchestration seam"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-05-dynamic-workflows.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
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
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "code"
  - "script"
  - "phases"
  - "phase"
  - "args"
  - "dispose"
  - "signal"
  - "ctx"
  - "ObjectJsonSchema"
  - "description"
  - "ValueSchemaSpec"
  - "outputSchema"
  - "effort"
  - "completed"
search_regex: "(?i)(code|script|phases|phase|args|dispose|signal|ObjectJsonSchema)"
---

# 0142. Dynamic workflows --- a script-driven multi-agent orchestration seam — implementation context

## Open this when

The harness can delegate ONE task to ONE child (dsh-tool-subagent), but work that fans out across many independent pieces --- an audit over many files, a migration, multi-angle research, adversarial verification of findings --- forces the model to orchestrate turn by turn: every intermediate result lands in the parent context, the plan lives nowhere durable, and coordination costs a model round-trip per step.

## Source decision

A workflow capability family at packages/workflow/ in the bash seam shape (Service Definition / Service Provider / Consumer), plus the structured-output foundation it needs on the subagent seam. A workflow call contains JSON meta (name, description, and optional whenToUse/phases) and a JavaScript script body with top-level await that returns a JSON value. Metadata is validated as data and never evaluated. The body receives agent(prompt, options), parallel(thunks), pipeline(items, ...stages), phase(title), log(message), and args.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-05-dynamic-workflows.md](../02-notes/implemented/feature/2026-07-05-dynamic-workflows.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-05-dynamic-workflows.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-05-dynamic-workflows.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/subsystems/workflow.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/workflow.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `workflow/end` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/client/ui-workflow-run/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workflow-run/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/client/ui-workflow-run`. | `named-file, named-package-member` |
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. Defines `args`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. Defines `signal`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `outputSchema`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/util/timeout/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/index.ts) | package entry point | Core file in the package named by the note: `packages/util/timeout`. Defines `signal`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/dispatch.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/dispatch.ts) | runtime implementation | Core file in the package named by the note: `packages/core/agent`. Defines `args`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `description`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/tools/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `code` | `const` | [`apps/cli/src/plugin.ts:135`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts#L135) | `const code = (result.error as NodeJS.ErrnoException).code` |
| `script` | `const` | [`packages/client/modules/src/index.ts:170`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L170) | `const script = \`<script>window.__DSH_BOOT__ = ${json}</script>\`` |
| `phases` | `const` | [`packages/client/ui-workflow-run/src/client/workflow-definition.ts:97`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workflow-run/src/client/workflow-definition.ts#L97) | `const phases = new Map<string, { phase: string \| null; members: WorkflowRunMemberData[] }>()` |
| `phase` | `const` | [`packages/client/ui-workflow-run/src/client/workflow-definition.ts:99`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workflow-run/src/client/workflow-definition.ts#L99) | `const phase = member.phase === undefined ? null : member.phase` |
| `args` | `const` | [`packages/core/agent/src/dispatch.ts:125`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/dispatch.ts#L125) | `const args: unknown[] = [carrier, name, fused(payload)]` |
| `dispose` | `const` | [`packages/core/agent/src/index.ts:373`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L373) | `const dispose = this.ctx.effect(() => {` |
| `dispose` | `const` | [`packages/core/agent/src/index.ts:451`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L451) | `const dispose = this.ctx.effect(function* (this: AgentRegistry) {` |
| `args` | `const` | [`packages/core/agent/src/index.ts:529`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L529) | `const args: unknown[] = [entry.carrier, 'agent/disposed', { agent: entry.agent }]` |
| `args` | `const` | [`packages/core/agent/src/index.ts:561`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L561) | `const args: unknown[] = [entry.carrier, 'agent/created', { agent: entry.agent }]` |
| `signal` | `const` | [`packages/core/tools/src/code-mode.ts:401`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L401) | `const signal = new Promise<void>((resolve) => { wake = resolve })` |
| `ctx` | `const` | [`packages/core/tools/src/index.ts:947`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L947) | `const ctx = this.ctx` |
| `dispose` | `const` | [`packages/core/tools/src/index.ts:951`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L951) | `const dispose = ctx.effect(function* (this: ToolRuntime) {` |
| `signal` | `const` | [`packages/core/tools/src/index.ts:1372`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1372) | `const signal = exec.signal` |
| `signal` | `const` | [`packages/core/tools/src/index.ts:1538`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1538) | `const signal = fused.signal` |
| `dispose` | `const` | [`packages/core/tools/src/index.ts:1894`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1894) | `const dispose = (): void => {` |
| `ObjectJsonSchema` | `type` | [`packages/core/tools/src/json-schema.ts:59`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L59) | `export type ObjectJsonSchema = JsonSchemaNode & { type: 'object' }` |

### Tests and executable evidence

- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `phases`. A test under the owning area exercises or imports `budget`.
- [`packages/skill/skill/tests/skill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/tests/skill.spec.ts) — A test under the owning area exercises or imports `whenToUse`.
- [`packages/core/tools/tests/schema.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/schema.spec.ts) — A test under the owning area exercises or imports `__proto__`. A test under the owning area exercises or imports `ValueSchemaSpec`.
- [`packages/core/tools/tests/py-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/py-types.spec.ts) — A test under the owning area exercises or imports `phases`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `isolation`. A test under the owning area exercises or imports `__proto__`.
- [`packages/core/tools/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/invariant.spec.ts) — A test under the owning area exercises or imports `run_code`.
- [`packages/core/tools/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/properties.spec.ts) — A test under the owning area exercises or imports `ValueSchemaSpec`.
- [`packages/core/tools/tests/json-schema.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/json-schema.spec.ts) — A test under the owning area exercises or imports `ObjectJsonSchema`.
- Source verification intent: Worker-side logic runs through an in-process MessageChannel so V8 coverage measures it. Unit tests cover script helpers, fatal and nullable failures, JSON boundaries, caps, cancellation, child ownership, and structured output through real loops. A built-bin smoke runs the separately bundled lib/worker.cjs under plain Node, a with-key e2e drives real child agents, and model-facing workflow behavior is snapshot-covered through its owning example.

## How to read the implementation

1. Start with [`docs/subsystems/workflow.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/workflow.md) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `code`, `script`, `phases`, `phase`, `args`, `dispose`, `signal`, `ctx`, `ObjectJsonSchema`, `description`, `ValueSchemaSpec`, `outputSchema`, `effort`, `completed`
- Regex: `(?i)(code|script|phases|phase|args|dispose|signal|ObjectJsonSchema)`

```bash
rg -n --pcre2 "(?i)(code|script|phases|phase|args|dispose|signal|ObjectJsonSchema)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0035. Agent-scope runtime design and correctness](0035-agent-scope-runtime-design-and-correctness.md): The source note links to this decision directly.
- **`source-link`** — [0051. Unified JSON-value schema DSL](0051-unified-json-value-schema-dsl.md): The source note links to this decision directly.
- **`source-link`** — [0283. Durable workflow runs in Chat](0283-durable-workflow-runs-in-chat.md): The source note links to this decision directly.
- **`shares-code-with`** — [0561. Plan mode --- a logged per-agent session mode](0561-plan-mode-a-logged-per-agent-session-mode.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0158. persistent PTY sessions](0158-persistent-pty-sessions.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0144. The approval seam --- one-shot permission decisions over a waterfall of answerers](0144-the-approval-seam-one-shot-permission-decisions-over-a-waterfall-of-answ.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/types.ts`.
- **`shares-code-with`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/py-types.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/py-types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0142-dynamic-workflows-a-script-driven-multi-agent-orchestration-seam.md`.
