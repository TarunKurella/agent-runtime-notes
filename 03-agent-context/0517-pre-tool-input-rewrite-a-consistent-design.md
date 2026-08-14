---
id: "dsh-note-0517"
title: "Pre-tool input rewrite --- a consistent design"
status: "proposed"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/feature/2026-06-30-pre-tool-input-rewrite.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/trust"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/proposed"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "ToolExecution"
  - "PreToolDecision"
  - "presentCall"
  - "presentResult"
  - "tools/pre-execute"
  - "PreToolUse"
  - "updatedInput"
  - "assistant/message"
  - "deriveMessages"
  - "tool/call"
  - "ctx.tools.execute"
  - "tool/call.arguments"
  - "dsh-tool-bash"
  - "exec.arguments"
search_regex: "(?i)(ToolExecution|PreToolDecision|presentCall|presentResult|tools/pre\\-execute|PreToolUse|updatedInput|assistant/message)"
---

# 0517. Pre-tool input rewrite --- a consistent design — implementation context

## Open this when

The interception extension-points Agent Note defines tools/pre-execute as an allow/deny/ask gate over an execution whose identity is already protected and whose arguments are deeply frozen. Claude Code's PreToolUse hook also offers updatedInput, so a faithful bridge needs an explicit rewrite mechanism. A rewrite cannot be a mutation escape hatch on the existing execution object: it must keep the durable history, audit record, presentation, and executed value consistent.

## Source decision

A rewrite is a pre-identity consistency transaction. When a hook supplies updatedInput, the effective value must be chosen before the registry constructs its immutable ToolExecution, and it must be reflected in all three readers atomically: The tool/call audit event records the REWRITTEN arguments (with the original retained in a sidecar field for the audit trail --- a hook changed the call, and both the original and the effective arguments are facts worth keeping).

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/feature/2026-06-30-pre-tool-input-rewrite.md](../02-notes/proposed/feature/2026-06-30-pre-tool-input-rewrite.md)
- Pinned source: [.agents/notes/proposed/feature/2026-06-30-pre-tool-input-rewrite.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/feature/2026-06-30-pre-tool-input-rewrite.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-bash`. | `named-package-member` |
| [`packages/shell/tool-bash/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/tool-bash`. | `named-package-member` |
| [`packages/shell/tool-bash`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `ToolExecution`, a construct named by the note. Defines `PreToolDecision`, a construct named by the note. | `symbol-definition` |
| [`packages/workflow/tool-ralph/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts) | package entry point | Defines `presentResult`, a construct named by the note. Defines `presentCall`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/tool-bash/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/README.md) | package contract and examples | Core file in the package named by the note: `packages/shell/tool-bash`. | `named-package-member` |
| [`packages/shell/tool-bash/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/package.json) | composition and configuration | Core file in the package named by the note: `packages/shell/tool-bash`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-tool-bash` named by the note. | `exact-code-occurrence` |
| [`docs/glossary.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/glossary.md) | package contract and examples | Contains the exact code literal `dsh-tool-bash` named by the note. | `exact-code-occurrence` |
| [`docs/glossary.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/glossary.zh.md) | package contract and examples | Contains the exact code literal `dsh-tool-bash` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `tools/pre-execute` named by the note. Contains the exact code literal `assistant/message` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) | package contract and examples | Contains the exact code literal `tool/call` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ToolExecution` | `interface` | [`packages/core/tools/src/index.ts:379`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L379) | `export interface ToolExecution extends ToolExecutionInput {` |
| `PreToolDecision` | `type` | [`packages/core/tools/src/index.ts:588`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L588) | `export type PreToolDecision =` |
| `presentCall` | `function` | [`packages/workflow/tool-ralph/src/index.ts:394`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts#L394) | `function presentCall(args: RalphCallArgs): ToolCallView {` |
| `presentResult` | `function` | [`packages/workflow/tool-ralph/src/index.ts:398`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts#L398) | `function presentResult(args: RalphCallArgs, result: { content: ContentBlock[]; isError: boolean }): ToolResultView {` |

### Tests and executable evidence

- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `PreToolDecision`.
- [`packages/core/tools/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/scoped.spec.ts) — A test under the owning area exercises or imports `ToolExecution`. A test under the owning area exercises or imports `PreToolDecision`.
- [`packages/core/tools/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/invariant.spec.ts) — A test under the owning area exercises or imports `ToolExecution`.
- [`packages/workflow/tool-ralph/tests/tool-ralph.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/tests/tool-ralph.spec.ts) — A test under the owning area exercises or imports `presentResult`. A test under the owning area exercises or imports `presentCall`.
- [`packages/core/tools/tests/execution-signal-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/execution-signal-types.spec.ts) — A test under the owning area exercises or imports `ToolExecution`.
- [`scripts/verify-cordis-config.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-cordis-config.spec.ts) — Contains the exact code literal `dsh-tool-bash` named by the note.
- Source verification intent: A requested rewrite is resolved before ToolExecution identity is created and reflected in all three readers atomically: the tool/call audit records the rewritten arguments (the original retained in a sidecar field), derived history agrees with what executed, and presentation renders the rewritten arguments. The effective ToolExecution.arguments remains deeply frozen and non-writable throughout pre-policy, guards, dispatch, post-policy, and final observation; no mutation shim is introduced. The CC/Codex bridges honor updatedInput instead of logging the faithful-but-degraded warning.

## How to read the implementation

1. Start with [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
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

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/trust`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/proposed`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`
- Aliases: `ToolExecution`, `PreToolDecision`, `presentCall`, `presentResult`, `tools/pre-execute`, `PreToolUse`, `updatedInput`, `assistant/message`, `deriveMessages`, `tool/call`, `ctx.tools.execute`, `tool/call.arguments`, `dsh-tool-bash`, `exec.arguments`
- Regex: `(?i)(ToolExecution|PreToolDecision|presentCall|presentResult|tools/pre\-execute|PreToolUse|updatedInput|assistant/message)`

```bash
rg -n --pcre2 "(?i)(ToolExecution|PreToolDecision|presentCall|presentResult|tools/pre\\-execute|PreToolUse|updatedInput|assistant/message)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0140. Interception extension points --- the typed-Decision surface a hook programs against](0140-interception-extension-points-the-typed-decision-surface-a-hook-programs.md): The source note links to this decision directly.
- **`shares-code-with`** — [0555. Consolidated TUI presentation and navigation](0555-consolidated-tui-presentation-and-navigation.md): Shares source implementation: `packages/shell/tool-bash`, `packages/shell/tool-bash/README.md`.
- **`shares-code-with`** — [0151. Parallel tool-call execution by per-call safety](0151-parallel-tool-call-execution-by-per-call-safety.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/tests/scoped.spec.ts`.
- **`shares-code-with`** — [0545. Every session event is enclosed in a turn](0545-every-session-event-is-enclosed-in-a-turn.md): Shares source implementation: `packages/shell/tool-bash`, `packages/shell/tool-bash/src/index.ts`.
- **`shares-code-with`** — [0558. Rich ACP bash rendering --- the terminal card via the `_meta` convention](0558-rich-acp-bash-rendering-the-terminal-card-via-the-meta-convention.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/shell/tool-bash/src/index.ts`.
- **`shares-code-with`** — [0611. Keep the Code Mode result card complete](0611-keep-the-code-mode-result-card-complete.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/workflow/tool-ralph/src/index.ts`.
- **`shares-code-with`** — [0242. pwsh tool bash parity](0242-pwsh-tool-bash-parity.md): Shares source implementation: `packages/shell/tool-bash/src/index.ts`, `packages/shell/tool-bash/src/invariant.ts`.
- **`shares-code-with`** — [0543. Custom typed tool-schema DSL instead of schemastery](0543-custom-typed-tool-schema-dsl-instead-of-schemastery.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/tests/scoped.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0517-pre-tool-input-rewrite-a-consistent-design.md`.
