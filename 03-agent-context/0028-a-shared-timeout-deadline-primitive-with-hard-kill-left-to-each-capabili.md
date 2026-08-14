---
id: "dsh-note-0028"
title: "A shared timeout/deadline primitive, with hard-kill left to each capability"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-06-timeout-deadline-library.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "code"
  - "ctx"
  - "search"
  - "aborted"
  - "timeoutMs"
  - "next"
  - "reason"
  - "timeout"
  - "StreamChunk"
  - "timedOut"
  - "ShellRunResult"
  - "onAbort"
  - "kill"
  - "TimeoutReason"
search_regex: "(?i)(code|search|aborted|timeoutMs|next|reason|timeout|StreamChunk)"
---

# 0028. A shared timeout/deadline primitive, with hard-kill left to each capability — implementation context

## Open this when

Timeout handling was drifting apart across the tool-bearing capabilities, and the divergence was not superficial --- it was the same logic re-implemented three ways, each with its own subtle correctness burden. bash (then in the bash-local implementation's run.ts) had a full, correct timeout inside the process plumbing: a config-clamped timeoutMs, two independent triggers --- a killTimer for the timeout and an onAbort listener for upstream cancellation --- each calling one kill() closure that escalates SIGTERM→grace→SIGKILL on the process group, and two orthogonal outcome booleans (timedOut, aborted) latched.

## Source decision

@deepseek-ai/dsh-timeout lives under packages/util/ (peer to dsh-brand) and owns the timing and classification half of timeout; the termination half --- the hard kill --- stays in each capability's implementation. It is a library of pure functions, not a cordis service or plugin: it takes no ctx, registers nothing, holds no cross-call state, and emits no events. There is deliberately no central "timeout service" that would have to know how to stop every capability's work --- that knowledge is exactly what a microkernel keeps out of shared layers, and what Codex's exec-only ExecExpiration scope demonstrates.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-06-timeout-deadline-library.md](../02-notes/implemented/architecture/2026-07-06-timeout-deadline-library.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-06-timeout-deadline-library.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-06-timeout-deadline-library.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/web/web/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web/src/types.ts) | public types and contract | The source note names this file directly. Defines `WebSearchRequest`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/web/tool-web/src/search.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/search.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/shell/bash-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts) | package entry point | The source note names this file directly. Defines `timedOut`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/web/web-fetch-http/src/provider.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-fetch-http/src/provider.ts) | provider/backend adapter | The source note names this file directly. Defines `translateAbortOrNetwork`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/subprocess/subprocess-local/src/spawn.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts) | runtime implementation | The source note names this file directly. Defines `kill`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/util/brand/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/brand/src/index.ts) | package entry point | Core file in the package named by the note: `packages/util/brand`. | `named-package-member` |
| [`packages/util/timeout/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/util`. Core file in the package named by the note: `packages/util/timeout`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/llm/llm-pi-ai/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-pi-ai`. Defines `next`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/util/brand/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/brand/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/util/brand`. | `named-package-member` |
| [`packages/llm/llm-pi-ai/src/adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts) | provider/backend adapter | Core file in the package named by the note: `packages/llm/llm-pi-ai`. Defines `timeout`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-bash`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-deepseek`. Defines `next`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `code` | `const` | [`apps/cli/src/plugin.ts:135`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts#L135) | `const code = (result.error as NodeJS.ErrnoException).code` |
| `ctx` | `const` | [`apps/cli/src/profile-boot.ts:248`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L248) | `const ctx = await boot(NAME, rootConfig, structuredClone(allPatches(composed)), (hostCtx) => {` |
| `search` | `const` | [`packages/client/ui-tool/src/client/tool/ToolDetails.tsx:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/ToolDetails.tsx#L41) | `const search = searchCardModel(block)` |
| `aborted` | `const` | [`packages/core/agent-loop/src/index.ts:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L98) | `const aborted = Promise.withResolvers<never>()` |
| `timeoutMs` | `const` | [`packages/guard/timeout-policy/src/index.ts:57`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/guard/timeout-policy/src/index.ts#L57) | `const timeoutMs = ctx.tools.get(exec.name, exec.agent)?.timeoutMs` |
| `next` | `const` | [`packages/llm/llm-deepseek/src/index.ts:208`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts#L208) | `const next = resolveAdapterOptions(raw, launchEnvironmentOf(ctx))` |
| `reason` | `const` | [`packages/llm/llm-deepseek/src/translate.ts:107`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/translate.ts#L107) | `const reason = pendingFinish ?? { kind: 'stop' as const }` |
| `timeout` | `const` | [`packages/llm/llm-pi-ai/src/adapter.ts:328`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts#L328) | `const timeout = timeoutOf(watchdog.signal, 'LLM_STREAM_IDLE_TIMEOUT')` |
| `next` | `const` | [`packages/llm/llm-pi-ai/src/index.ts:168`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts#L168) | `const next = resolveProfiles(raw.providers)` |
| `StreamChunk` | `type` | [`packages/llm/llm/src/types.ts:291`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L291) | `export type StreamChunk =` |
| `timedOut` | `const` | [`packages/shell/bash-local/src/index.ts:230`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts#L230) | `const timedOut = timeoutOf(d.signal, 'BASH_TIMEOUT') !== undefined` |
| `ShellRunResult` | `interface` | [`packages/shell/shell/src/types.ts:113`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts#L113) | `export interface ShellRunResult {` |
| `onAbort` | `const` | [`packages/skill/skill/src/index.ts:826`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L826) | `const onAbort = (): void => {` |
| `kill` | `const` | [`packages/subprocess/subprocess-local/src/spawn.ts:432`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts#L432) | `const kill = (sig: NodeJS.Signals): void => {` |
| `TimeoutReason` | `class` | [`packages/util/timeout/src/index.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/index.ts#L12) | `export class TimeoutReason extends Error {` |
| `clampTimeout` | `function` | [`packages/util/timeout/src/index.ts:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/index.ts#L45) | `export function clampTimeout(` |

### Tests and executable evidence

- [`packages/web/web/tests/web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web/tests/web.spec.ts) — A test under the owning area exercises or imports `WebSearchRequest`.
- [`packages/shell/shell/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/tests/service.spec.ts) — A test under the owning area exercises or imports `ShellRunResult`.
- [`packages/util/timeout/tests/timeout.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/tests/timeout.spec.ts) — A test under the owning area exercises or imports `TimeoutReason`. A test under the owning area exercises or imports `clampTimeout`.
- [`packages/llm/llm-pi-ai/tests/adapter.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/adapter.e2e.ts) — A test under the owning area exercises or imports `dsh-llm-deepseek`. A test under the owning area exercises or imports `dsh-llm-pi-ai`.
- [`packages/shell/tool-bash/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/tests/tools.spec.ts) — A test under the owning area exercises or imports `timedOut`. A test under the owning area exercises or imports `AbortError`.
- [`packages/llm/llm-pi-ai/tests/adapter.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/adapter.spec.ts) — A test under the owning area exercises or imports `dsh-timeout`. A test under the owning area exercises or imports `dsh-llm-pi-ai`.
- [`packages/llm/llm-pi-ai/tests/catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/catalog.spec.ts) — A test under the owning area exercises or imports `dsh-llm-pi-ai`.
- [`packages/llm/llm-pi-ai/tests/convert.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/convert.spec.ts) — A test under the owning area exercises or imports `TIMEOUT`. A test under the owning area exercises or imports `ABORTED`.

## How to read the implementation

1. Start with [`packages/web/web/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web/src/types.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/tools`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `code`, `ctx`, `search`, `aborted`, `timeoutMs`, `next`, `reason`, `timeout`, `StreamChunk`, `timedOut`, `ShellRunResult`, `onAbort`, `kill`, `TimeoutReason`
- Regex: `(?i)(code|search|aborted|timeoutMs|next|reason|timeout|StreamChunk)`

```bash
rg -n --pcre2 "(?i)(code|search|aborted|timeoutMs|next|reason|timeout|StreamChunk)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0091. Packaged ripgrep spawn for glob/grep](0091-packaged-ripgrep-spawn-for-glob-grep.md): The source note links to this decision directly.
- **`shares-code-with`** — [0029. Tool result retention library](0029-tool-result-retention-library.md): Shares source implementation: `packages/util/brand/src/index.ts`, `packages/util/brand/src/invariant.ts`.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/util/brand/src/index.ts`, `packages/util/brand/src/invariant.ts`.
- **`shares-code-with`** — [0094. pi-ai routes are declared providers, not catalog lookups](0094-pi-ai-routes-are-declared-providers-not-catalog-lookups.md): Shares source implementation: `packages/llm/llm-pi-ai/src/adapter.ts`, `packages/llm/llm-pi-ai/src/index.ts`.
- **`shares-code-with`** — [0128. A pi-ai model declares its own input modalities, and undeclared means text](0128-a-pi-ai-model-declares-its-own-input-modalities-and-undeclared-means-tex.md): Shares source implementation: `packages/llm/llm-pi-ai/src/adapter.ts`, `packages/llm/llm-pi-ai/src/index.ts`.
- **`shares-code-with`** — [0175. Web multimodal image input and durable attachments](0175-web-multimodal-image-input-and-durable-attachments.md): Shares source implementation: `packages/util/brand/src/index.ts`, `packages/util/brand/src/invariant.ts`.
- **`shares-code-with`** — [0040. LSP capability seam and model-facing query tool](0040-lsp-capability-seam-and-model-facing-query-tool.md): Shares source implementation: `packages/util/brand/src/index.ts`.
- **`shares-code-with`** — [0087. the web configuration plane](0087-the-web-configuration-plane.md): Shares source implementation: `packages/llm/llm-pi-ai/src/adapter.ts`, `packages/llm/llm-pi-ai/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0028-a-shared-timeout-deadline-primitive-with-hard-kill-left-to-each-capabili.md`.
