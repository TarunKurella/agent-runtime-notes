---
id: "dsh-note-0029"
title: "Tool result retention library"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-06-tool-result-retention-library.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/tools"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "ctx"
  - "push"
  - "ToolExecutionResult"
  - "unknown"
  - "finish"
  - "tail"
  - "limit"
  - "exact"
  - "maxBytes"
  - "totalLines"
  - "Omitted"
  - "PushDecision"
  - "RetainedItems"
  - "RetainedText"
search_regex: "(?i)(push|ToolExecutionResult|unknown|finish|tail|limit|exact|maxBytes)"
---

# 0029. Tool result retention library — implementation context

## Open this when

Several model-facing tools already bound the amount of context they return, but each one owns a different local mechanism and vocabulary: bash keeps a tail plus spill files, web search caps source lists, web fetch caps body content, and glob / grep discovery needs an inline first page while keeping exact omission metadata for the full result set. A single truncate(text) helper cannot cover those cases: item tools need item counts and grouping outside the primitive, while text tools need byte budgets and UTF-8-safe head/tail cuts. The shared abstraction the tools need is retention, not generic collection.

## Source decision

@deepseek-ai/dsh-output-retention lives under packages/util/ (peer to dsh-brand and dsh-timeout) and owns bounded model-facing output. It is a library of pure classes and functions, not a Cordis service or plugin: it takes no ctx, registers nothing, holds no cross-call state, and emits no events. Tool packages import it directly when they need bounded output. The library has two independent retainers: ItemRetainer handles ordered logical units such as paths, grep matches, or search sources. It supports head retention only in v1, while keeping the retainer shape open to additional retention strategies later.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-06-tool-result-retention-library.md](../02-notes/implemented/architecture/2026-07-06-tool-result-retention-library.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-06-tool-result-retention-library.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-06-tool-result-retention-library.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/util/output-retention/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/output-retention/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/util/output-retention`. | `named-file, named-package-member` |
| [`packages/util/brand/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/brand/src/index.ts) | package entry point | Core file in the package named by the note: `packages/util/brand`. | `named-package-member` |
| [`packages/util/timeout/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/index.ts) | package entry point | Core file in the package named by the note: `packages/util/timeout`. | `named-package-member` |
| [`packages/util/brand/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/brand/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/util/brand`. | `named-package-member` |
| [`packages/util/timeout/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/util/timeout`. | `named-package-member` |
| [`packages/util/output-retention/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/output-retention/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/util`. Core file in the package named by the note: `packages/util/output-retention`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/util/output-retention/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/output-retention/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/util/output-retention`. | `named-package-member` |
| [`packages/util/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/util`. | `named-directory-member` |
| [`packages/util`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/util) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/util/brand`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/util/brand) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/util/timeout`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/util/output-retention`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/util/output-retention) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ctx` | `const` | [`apps/cli/src/profile-boot.ts:248`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L248) | `const ctx = await boot(NAME, rootConfig, structuredClone(allPatches(composed)), (hostCtx) => {` |
| `push` | `const` | [`packages/client/connection/src/client/fixture.ts:361`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L361) | `const push = (e: Record<string, unknown>): number => {` |
| `ToolExecutionResult` | `type` | [`packages/core/tools/src/index.ts:580`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L580) | `export type ToolExecutionResult = ToolExecutionSuccess \| ToolExecutionFailure` |
| `unknown` | `const` | [`packages/core/tools/src/index.ts:1089`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1089) | `const unknown = [...allow ?? [], ...deny ?? []].filter(name => !known.has(name))` |
| `finish` | `const` | [`packages/core/tools/src/ts-types.ts:115`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/ts-types.ts#L115) | `const finish = (document: TypeDocument): void => {` |
| `tail` | `const` | [`packages/e2b/fs-e2b/src/index.ts:434`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L434) | `const tail = run.then(() => undefined, () => undefined)` |
| `limit` | `const` | [`packages/fs/tool-fs/src/read.ts:59`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts#L59) | `const limit = args.limit === undefined ? maxLimit : parsePositiveInteger(args.limit, 'limit')` |
| `exact` | `const` | [`packages/host/webserver/src/index.ts:243`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/webserver/src/index.ts#L243) | `const exact = this.exact.get(pathname)` |
| `maxBytes` | `const` | [`packages/jobs/tool-jobs/src/index.ts:151`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L151) | `const maxBytes = snapshot.outputLimitBytes` |
| `totalLines` | `const` | [`packages/terminal/terminal-bash/src/session.ts:320`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal-bash/src/session.ts#L320) | `const totalLines = snapshot.text.length === 0 ? 0 : lines.length` |
| `Omitted` | `type` | [`packages/util/output-retention/src/index.ts:40`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/output-retention/src/index.ts#L40) | `export type Omitted =` |
| `PushDecision` | `interface` | [`packages/util/output-retention/src/index.ts:48`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/output-retention/src/index.ts#L48) | `export interface PushDecision {` |
| `RetainedItems` | `interface` | [`packages/util/output-retention/src/index.ts:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/output-retention/src/index.ts#L62) | `export interface RetainedItems<T> {` |
| `RetainedText` | `interface` | [`packages/util/output-retention/src/index.ts:80`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/output-retention/src/index.ts#L80) | `export interface RetainedText {` |
| `ItemRetentionStrategy` | `type` | [`packages/util/output-retention/src/index.ts:87`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/output-retention/src/index.ts#L87) | `export type ItemRetentionStrategy = {` |
| `TextRetentionStrategy` | `type` | [`packages/util/output-retention/src/index.ts:94`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/output-retention/src/index.ts#L94) | `export type TextRetentionStrategy =` |

### Tests and executable evidence

- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `ToolExecutionResult`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `ToolExecutionResult`.
- [`packages/core/tools/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/invariant.spec.ts) — A test under the owning area exercises or imports `ToolExecutionResult`.
- [`packages/util/output-retention/tests/output-retention.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/output-retention/tests/output-retention.spec.ts) — A test under the owning area exercises or imports `ItemRetainer`. A test under the owning area exercises or imports `TextRetainer`.
- [`scripts/client-bundle-purity.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-purity.spec.ts) — Contains the exact code literal `dsh-brand` named by the note.

## How to read the implementation

1. Start with [`packages/util/output-retention/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/output-retention/README.md) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/discovery-routing`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/tools`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `ctx`, `push`, `ToolExecutionResult`, `unknown`, `finish`, `tail`, `limit`, `exact`, `maxBytes`, `totalLines`, `Omitted`, `PushDecision`, `RetainedItems`, `RetainedText`
- Regex: `(?i)(push|ToolExecutionResult|unknown|finish|tail|limit|exact|maxBytes)`

```bash
rg -n --pcre2 "(?i)(push|ToolExecutionResult|unknown|finish|tail|limit|exact|maxBytes)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0015. The background job runtime (`ctx.jobs`) and generic task control tools](0015-the-background-job-runtime-ctx-jobs-and-generic-task-control-tools.md): The source note links to this decision directly.
- **`source-link`** — [0049. Canonical tool output contract](0049-canonical-tool-output-contract.md): The source note links to this decision directly.
- **`shares-code-with`** — [0028. A shared timeout/deadline primitive, with hard-kill left to each capability](0028-a-shared-timeout-deadline-primitive-with-hard-kill-left-to-each-capabili.md): Shares source implementation: `packages/util/brand/src/index.ts`, `packages/util/brand/src/invariant.ts`.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/util/brand/src/index.ts`, `packages/util/brand/src/invariant.ts`.
- **`shares-code-with`** — [0175. Web multimodal image input and durable attachments](0175-web-multimodal-image-input-and-durable-attachments.md): Shares source implementation: `packages/util/brand/src/index.ts`, `packages/util/brand/src/invariant.ts`.
- **`shares-code-with`** — [0168. Cross-session references](0168-cross-session-references.md): Shares source implementation: `packages/util/output-retention/src/index.ts`, `packages/util/output-retention/src/invariant.ts`.
- **`shares-code-with`** — [0040. LSP capability seam and model-facing query tool](0040-lsp-capability-seam-and-model-facing-query-tool.md): Shares source implementation: `packages/util/brand/src/index.ts`.
- **`shares-code-with`** — [0142. Dynamic workflows --- a script-driven multi-agent orchestration seam](0142-dynamic-workflows-a-script-driven-multi-agent-orchestration-seam.md): Shares source implementation: `packages/util/timeout/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0029-tool-result-retention-library.md`.
