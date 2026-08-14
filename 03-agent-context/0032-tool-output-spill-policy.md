---
id: "dsh-note-0032"
title: "Tool output spill policy"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-08-tool-output-spill-files.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "sessionId"
  - "sha256"
  - "subagent"
  - "additionalContexts"
  - "inject"
  - "root"
  - "encodeSegment"
  - "maxInlineBytes"
  - "spillStore"
  - "SpillLocator"
  - "ItemRetainer"
  - "TextRetainer"
  - "next"
  - "dsh-bash-local"
search_regex: "(?i)(sessionId|sha256|subagent|additionalContexts|inject|root|encodeSegment|maxInlineBytes)"
---

# 0032. Tool output spill policy — implementation context

## Open this when

Tool outputs need bounded model-facing previews, but some oversized results are still useful later. A fetched page body or a verbose tool response should not consume the next model request in full, but the model should be able to inspect the complete formatted result later with existing file-reading tools. Before this change the behavior was uneven. dsh-bash-local already writes complete stdout/stderr streams to private temp spill files when its in-memory tail overflows, but ordinary text tool results were returned inline unless the tool hand-rolled its own cap.

## Source decision

A thin spill storage seam plus a default spill policy plugin, in a new packages/spill/ group: There is no dedicated model-facing Consumer package. The Consumer is the existing ctx.tools execution pipeline: dsh-spill-policy consumes final tool results through the tools/post-execute waterfall, and the model follows the backend-supplied retrieval hint for the returned locator. The storage seam is minimal: save text and return a locator plus retrieval hint. SpillLocator is a branded model-facing handle returned by the backend.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-08-tool-output-spill-files.md](../02-notes/implemented/architecture/2026-07-08-tool-output-spill-files.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-08-tool-output-spill-files.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-08-tool-output-spill-files.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. Defines `additionalContexts`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/spill/spill/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/spill`. Core file in the package named by the note: `packages/spill/spill`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/spill/spill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill/src/index.ts) | package entry point | Core file in the package named by the note: `packages/spill/spill`. | `named-package-member` |
| [`packages/core/tools/src/testing.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/testing.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/web/tool-web/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/index.ts) | package entry point | Core file in the package named by the note: `packages/web/tool-web`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/web/tool-web/src/fetch.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts) | runtime implementation | Core file in the package named by the note: `packages/web/tool-web`. | `named-package-member` |
| [`packages/core/tools/src/ts-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/ts-types.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/tools`. Defines `root`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/spill/spill/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/spill`. Core file in the package named by the note: `packages/spill/spill`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/shell/bash-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/bash-local`. | `named-package-member` |
| [`packages/web/tool-web/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/web/tool-web`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `sessionId` | `const` | [`packages/acp/acp/src/index.ts:254`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L254) | `const sessionId = SessionId(randomUUID())` |
| `sha256` | `const` | [`packages/attachment/attachment-local/src/store.ts:139`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/attachment/attachment-local/src/store.ts#L139) | `const sha256 = digest(input.data)` |
| `subagent` | `const` | [`packages/client/ui-subagent/src/client/index.ts:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-subagent/src/client/index.ts#L45) | `const subagent = owner.session?.subagent` |
| `additionalContexts` | `const` | [`packages/core/tools/src/index.ts:1760`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1760) | `const additionalContexts = [` |
| `additionalContexts` | `const` | [`packages/core/tools/src/index.ts:1920`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1920) | `const additionalContexts = prior?.additionalContexts ?? []` |
| `additionalContexts` | `const` | [`packages/core/tools/src/index.ts:1934`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1934) | `const additionalContexts = prior?.additionalContexts ?? []` |
| `inject` | `const` | [`packages/core/tools/src/invariant.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts#L13) | `export const inject = ['invariants']` |
| `root` | `const` | [`packages/core/tools/src/invariant.ts:39`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts#L39) | `const root = String(event.data.rootCallId)` |
| `encodeSegment` | `function` | [`packages/spill/spill-local/src/store.ts:48`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-local/src/store.ts#L48) | `export function encodeSegment(raw: string): string {` |
| `maxInlineBytes` | `const` | [`packages/spill/spill-policy/src/index.ts:111`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/src/index.ts#L111) | `const maxInlineBytes = config.maxInlineBytes` |
| `spillStore` | `const` | [`packages/spill/spill-policy/src/index.ts:142`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/src/index.ts#L142) | `const spillStore = ctx.get('spillStore')` |
| `inject` | `const` | [`packages/spill/spill/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `SpillLocator` | `type` | [`packages/spill/spill/src/types.ts:18`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill/src/types.ts#L18) | `export type SpillLocator = Branded<'SpillLocator'>` |
| `SpillLocator` | `function` | [`packages/spill/spill/src/types.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill/src/types.ts#L26) | `export function SpillLocator(locator: string): SpillLocator {` |
| `ItemRetainer` | `class` | [`packages/util/output-retention/src/index.ts:146`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/output-retention/src/index.ts#L146) | `export class ItemRetainer<T> {` |
| `TextRetainer` | `class` | [`packages/util/output-retention/src/index.ts:247`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/output-retention/src/index.ts#L247) | `export class TextRetainer {` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `additionalContexts`.
- [`packages/web/tool-web/tests/spill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/tests/spill.spec.ts) — A test under the owning area exercises or imports `dsh-spill-policy`. A test under the owning area exercises or imports `dsh-spill-local`.
- [`packages/spill/spill/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill/tests/service.spec.ts) — A test under the owning area exercises or imports `spillStore`. A test under the owning area exercises or imports `SpillLocator`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `additionalContexts`.
- [`packages/web/tool-web/tests/tool-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/tests/tool-web.spec.ts) — A test under the owning area exercises or imports `web_fetch`. A test under the owning area exercises or imports `dsh-tool-web`.
- Source verification intent: dsh-spill unit tests pin the seam contract: registration as ctx.spillStore, one-implementation-per-context, and disposal release. dsh-spill-local unit tests cover saveText, encodeSegment sanitization (separators/tilde/whole-segment dots/empty), the session-hash directory, owner-only permissions, distinct paths per save, the configured/private root, and a storage-failure rejection.

## How to read the implementation

1. Start with [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`
- Aliases: `sessionId`, `sha256`, `subagent`, `additionalContexts`, `inject`, `root`, `encodeSegment`, `maxInlineBytes`, `spillStore`, `SpillLocator`, `ItemRetainer`, `TextRetainer`, `next`, `dsh-bash-local`
- Regex: `(?i)(sessionId|sha256|subagent|additionalContexts|inject|root|encodeSegment|maxInlineBytes)`

```bash
rg -n --pcre2 "(?i)(sessionId|sha256|subagent|additionalContexts|inject|root|encodeSegment|maxInlineBytes)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0029. Tool result retention library](0029-tool-result-retention-library.md): The source note links to this decision directly.
- **`source-link`** — [0049. Canonical tool output contract](0049-canonical-tool-output-contract.md): The source note links to this decision directly.
- **`shares-code-with`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0165. Typed tool returns in Code Mode](0165-typed-tool-returns-in-code-mode.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0137. The `todo_write` tool --- model task list as event-sourced session state](0137-the-todo-write-tool-model-task-list-as-event-sourced-session-state.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0228. Code Mode language dispatch and the Python SDK renderer](0228-code-mode-language-dispatch-and-the-python-sdk-renderer.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0030. Tool-call timeout policy as a plugin](0030-tool-call-timeout-policy-as-a-plugin.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/testing.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0032-tool-output-spill-policy.md`.
