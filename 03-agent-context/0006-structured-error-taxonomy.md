---
id: "dsh-note-0006"
title: "Structured error taxonomy"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-06-11-structured-error-taxonomy.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/security"
  - "domain/session-state"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/registry"
aliases:
  - "ToolExecutionResult"
  - "ToolArgsError"
  - "code"
  - "HarnessError"
  - "cause"
  - "isHarnessError"
  - "LlmError"
  - "toError"
  - "new Error(String(x))"
  - "HarnessError extends Error"
  - "dsh-llm"
  - "ErrorOptions"
  - "tool/result"
  - "code: 'UNKNOWN"
search_regex: "(?i)(ToolExecutionResult|ToolArgsError|code|HarnessError|cause|isHarnessError|LlmError|toError)"
---

# 0006. Structured error taxonomy — implementation context

## Open this when

Failures crossed seams as bare strings. A tool error flattened to a text block --- name, code, and stack lost --- so a future sandbox/retry plugin couldn't tell ENOENT from EACCES, and the model got less actionable feedback than it could. A non-Error throw degraded further: the loop wrapped it in new Error(String(x)), dropping any code. And LlmError was the only typed error in the system, with no shared base, so there was nothing for a consumer to instanceof against generically.

## Source decision

A single HarnessError extends Error base in dsh-llm (the leaf package every other imports --- no new dependency edge): a stable code distinct from message, cause chaining via ErrorOptions, and name defaulting to the subclass. isHarnessError narrows at seams. LlmError and ToolArgsError (dsh-tools) extend it, keeping their existing codes. ToolExecutionResult gains optional error: { name, code }, populated in the registry's catch when the thrown value is a HarnessError.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-06-11-structured-error-taxonomy.md](../02-notes/implemented/architecture/2026-06-11-structured-error-taxonomy.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-06-11-structured-error-taxonomy.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-06-11-structured-error-taxonomy.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. Defines `LlmError`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/error.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm`. Defines `cause`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. Defines `ToolExecutionResult`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `ToolArgsError`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/llm/llm/src/adapter-failure.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/adapter-failure.ts) | provider/backend adapter | Core file in the package named by the note: `packages/llm/llm`. Defines `code`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/tools`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ToolExecutionResult` | `type` | [`packages/core/tools/src/index.ts:580`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L580) | `export type ToolExecutionResult = ToolExecutionSuccess \| ToolExecutionFailure` |
| `ToolArgsError` | `class` | [`packages/core/tools/src/schema.ts:461`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L461) | `export class ToolArgsError extends HarnessError {` |
| `code` | `const` | [`packages/llm/llm/src/adapter-failure.ts:68`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/adapter-failure.ts#L68) | `const code = candidate.code` |
| `HarnessError` | `class` | [`packages/llm/llm/src/error.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts#L13) | `export class HarnessError extends Error {` |
| `cause` | `const` | [`packages/llm/llm/src/error.ts:140`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts#L140) | `const cause = causeText === '' \|\| causeText === message ? '' : \`: ${causeText}\`` |
| `isHarnessError` | `function` | [`packages/llm/llm/src/error.ts:161`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts#L161) | `export function isHarnessError(value: unknown): value is HarnessError {` |
| `LlmError` | `class` | [`packages/llm/llm/src/index.ts:83`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L83) | `export class LlmError extends HarnessError {` |
| `toError` | `function` | [`packages/skill/skill/src/index.ts:850`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L850) | `function toError(error: unknown): Error {` |

### Tests and executable evidence

- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `LlmError`. A test under the owning area exercises or imports `isHarnessError`.
- [`packages/llm/llm/tests/topology.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/topology.spec.ts) — A test under the owning area exercises or imports `LlmError`.
- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `ToolArgsError`. A test under the owning area exercises or imports `ToolExecutionResult`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `ToolExecutionResult`. A test under the owning area exercises or imports `HarnessError`.
- [`packages/core/tools/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/invariant.spec.ts) — A test under the owning area exercises or imports `ToolExecutionResult`.

## How to read the implementation

1. Start with [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/agent-loop`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/security`, `domain/session-state`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/registry`
- Aliases: `ToolExecutionResult`, `ToolArgsError`, `code`, `HarnessError`, `cause`, `isHarnessError`, `LlmError`, `toError`, `new Error(String(x))`, `HarnessError extends Error`, `dsh-llm`, `ErrorOptions`, `tool/result`, `code: 'UNKNOWN`
- Regex: `(?i)(ToolExecutionResult|ToolArgsError|code|HarnessError|cause|isHarnessError|LlmError|toError)`

```bash
rg -n --pcre2 "(?i)(ToolExecutionResult|ToolArgsError|code|HarnessError|cause|isHarnessError|LlmError|toError)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0145. Explicit model-facing tool order](0145-explicit-model-facing-tool-order.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0494. Property-based testing for protocol-shaped code](0494-property-based-testing-for-protocol-shaped-code.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/types.ts`.
- **`shares-code-with`** — [0641. Tutorial-style Cordis docs under docs/cordis-tutorial](0641-tutorial-style-cordis-docs-under-docs-cordis-tutorial.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/schema.ts`.
- **`shares-code-with`** — [0018. Web capability seam - stable tools over multiple providers](0018-web-capability-seam-stable-tools-over-multiple-providers.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/types.ts`.
- **`shares-code-with`** — [0578. The /reload command re-reads loader configs on demand](0578-the-reload-command-re-reads-loader-configs-on-demand.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0302. Render error cause chains at every diagnostic boundary](0302-render-error-cause-chains-at-every-diagnostic-boundary.md): Shares source implementation: `packages/llm/llm/src/adapter-failure.ts`, `packages/llm/llm/src/error.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0006-structured-error-taxonomy.md`.
