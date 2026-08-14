---
id: "dsh-note-0542"
title: "Dependency swaps rejected by the 2026-07 NIH audit"
status: "rejected"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/rejected/simplification/2026-07-26-dependency-swaps-rejected-by-nih-audit.md"
implementation_evidence: "high"
target_anchor: "repository tests and release policy"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/rejected"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "raceAbort"
  - "snapshotJsonValue"
  - "isJsonValue"
  - "isDeepEqualJson"
  - "ReplaceFileW"
  - "timeout"
  - "exitsWithin"
  - "scanZstdFrames"
  - "yaml"
  - "files"
  - "TimeoutReason"
  - "idleWatchdog"
  - "vscode-jsonrpc"
  - "lsp-stdio"
search_regex: "(?i)(raceAbort|snapshotJsonValue|isJsonValue|isDeepEqualJson|ReplaceFileW|timeout|exitsWithin|scanZstdFrames)"
---

# 0542. Dependency swaps rejected by the 2026-07 NIH audit — implementation context

## Open this when

A repository-wide "Not Invented Here" audit (2026-07-26, ten parallel surveys covering every package group, scripts/, native/, vendor/ edges, python/, test infrastructure, and CI) asked of each hand-rolled surface: would a maintained external package or Node builtin delete it with a net win under the dependency policy? The positive findings became their own proposed notes. The negative verdicts carry equal value --- each names a plausible-looking swap whose hand-rolled shape is load-bearing --- but would otherwise live only in a PR body. This note freezes them.

## Source decision

Adopt the following dependency swaps. Rejected --- per-item evidence below; a future proposal for any item must beat its recorded reason, not just re-cite the policy. Protocol and parsing: vscode-jsonrpc for LSP base-protocol framing/correlation (lsp-stdio): the swappable core is ~255 of ~1,800 src lines; the package cannot express the configured maxMessageBytes incoming-size bound (restoring it means rebuilding the deleted framing), inverts the cancel-grace teardown semantics (raceAbort rejects immediately then tears down; vscode-jsonrpc keeps the promise pending), errors on pre-header stdout banners real.

## Decision status

Rejected design. Do not implement it as written. Reuse its failure analysis, constraints, and reversal condition.

- Raw note: [rejected/simplification/2026-07-26-dependency-swaps-rejected-by-nih-audit.md](../02-notes/rejected/simplification/2026-07-26-dependency-swaps-rejected-by-nih-audit.md)
- Pinned source: [.agents/notes/rejected/simplification/2026-07-26-dependency-swaps-rejected-by-nih-audit.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/rejected/simplification/2026-07-26-dependency-swaps-rejected-by-nih-audit.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/verify-cordis-config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-cordis-config.ts) | repository automation | The source note names this file directly. Contains the exact code literal `scripts/verify-cordis-config.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/sdk/server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts) | package entry point | Core file in the package named by the note: `packages/sdk/server`. | `named-package-member` |
| [`packages/fs/fs-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/fs/fs-local/src/win32.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/win32.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/fs-local`. Defines `ReplaceFileW`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/util/timeout/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/index.ts) | package entry point | Core file in the package named by the note: `packages/util/timeout`. Defines `timeout`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-retry/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-retry`. | `named-package-member` |
| [`packages/llm/llm-retry/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm-retry`. | `named-package-member` |
| [`packages/lsp/lsp-stdio/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/index.ts) | package entry point | Core file in the package named by the note: `packages/lsp/lsp-stdio`. | `named-package-member` |
| [`packages/lsp/lsp-stdio/src/abort.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/abort.ts) | runtime implementation | Core file in the package named by the note: `packages/lsp/lsp-stdio`. Defines `timeout`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/sdk/server/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/sdk/server`. | `named-package-member` |
| [`packages/fs/fs-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/util/timeout/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/util/timeout`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `raceAbort` | `function` | [`packages/core/agent-loop/src/index.ts:93`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L93) | `async function raceAbort<T>(operation: PromiseLike<T> \| T, signal: AbortSignal, id: SessionId): Promise<T> {` |
| `snapshotJsonValue` | `function` | [`packages/core/session/src/json.ts:177`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/json.ts#L177) | `export function snapshotJsonValue<T>(value: T): T \| undefined {` |
| `isJsonValue` | `function` | [`packages/core/session/src/json.ts:188`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/json.ts#L188) | `export function isJsonValue(value: unknown): boolean {` |
| `isDeepEqualJson` | `function` | [`packages/core/session/src/surface.ts:273`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L273) | `function isDeepEqualJson(a: unknown, b: unknown): boolean {` |
| `ReplaceFileW` | `type` | [`packages/fs/fs-local/src/win32.ts:17`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/win32.ts#L17) | `type ReplaceFileW = (` |
| `timeout` | `const` | [`packages/lsp/lsp-stdio/src/abort.ts:14`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/abort.ts#L14) | `const timeout = timeoutOf(signal)` |
| `exitsWithin` | `function` | [`packages/sdk/client/src/dispose.ts:18`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/dispose.ts#L18) | `function exitsWithin(child: ChildProcess, ms: number): Promise<boolean> {` |
| `scanZstdFrames` | `function` | [`packages/session/session-persistence-jsonl/src/zstd.ts:48`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/zstd.ts#L48) | `export function scanZstdFrames(buffer: Buffer, maxFrames = Number.POSITIVE_INFINITY): ZstdFrameScan {` |
| `yaml` | `const` | [`packages/skill/skill-filesystem/src/index.ts:917`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts#L917) | `const yaml = raw.slice(start, closing.start)` |
| `files` | `const` | [`packages/typert/generator/src/analyzer.ts:220`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L220) | `const files = new Map<string, ts.SourceFile \| undefined>()` |
| `TimeoutReason` | `class` | [`packages/util/timeout/src/index.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/index.ts#L12) | `export class TimeoutReason extends Error {` |
| `idleWatchdog` | `function` | [`packages/util/timeout/src/index.ts:126`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/index.ts#L126) | `export function idleWatchdog(` |
| `timeout` | `const` | [`packages/util/timeout/src/index.ts:132`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/index.ts#L132) | `const timeout = new AbortController()` |

### Tests and executable evidence

- [`packages/fs/fs-local/tests/fsio.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/tests/fsio.spec.ts) — A test under the owning area exercises or imports `FS_NOT_TEXT`.
- [`packages/core/session/tests/json.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/json.spec.ts) — A test under the owning area exercises or imports `snapshotJsonValue`. A test under the owning area exercises or imports `isJsonValue`.
- [`packages/fs/fs-local/tests/win32.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/tests/win32.spec.ts) — A test under the owning area exercises or imports `ReplaceFileW`.
- [`packages/lsp/lsp-stdio/tests/host.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/host.spec.ts) — A test under the owning area exercises or imports `dsh-lsp-stdio`. A test under the owning area exercises or imports `dsh-timeout`.
- [`packages/llm/llm-retry/tests/retry.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/tests/retry.spec.ts) — A test under the owning area exercises or imports `Retry-After`.
- [`packages/util/timeout/tests/timeout.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/tests/timeout.spec.ts) — A test under the owning area exercises or imports `TimeoutReason`. A test under the owning area exercises or imports `idleWatchdog`.
- [`packages/lsp/lsp-stdio/tests/framing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/framing.spec.ts) — A test under the owning area exercises or imports `dsh-lsp-stdio`.
- [`packages/lsp/lsp-stdio/tests/provider.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/provider.spec.ts) — A test under the owning area exercises or imports `lsp-stdio`. A test under the owning area exercises or imports `dsh-lsp-stdio`.

## How to read the implementation

1. Start with [`scripts/verify-cordis-config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-cordis-config.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and release policy.

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
- **Status gate** — Rejected design. Do not implement it as written. Reuse its failure analysis, constraints, and reversal condition.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/jobs-tasks`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/rejected`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `raceAbort`, `snapshotJsonValue`, `isJsonValue`, `isDeepEqualJson`, `ReplaceFileW`, `timeout`, `exitsWithin`, `scanZstdFrames`, `yaml`, `files`, `TimeoutReason`, `idleWatchdog`, `vscode-jsonrpc`, `lsp-stdio`
- Regex: `(?i)(raceAbort|snapshotJsonValue|isJsonValue|isDeepEqualJson|ReplaceFileW|timeout|exitsWithin|scanZstdFrames)`

```bash
rg -n --pcre2 "(?i)(raceAbort|snapshotJsonValue|isJsonValue|isDeepEqualJson|ReplaceFileW|timeout|exitsWithin|scanZstdFrames)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): The source note links to this decision directly.
- **`source-link`** — [0569. Support the TUI on Windows](0569-support-the-tui-on-windows.md): The source note links to this decision directly.
- **`source-link`** — [0671. Replace the hand-rolled SSE parser in llm-deepseek with eventsource-parser](0671-replace-the-hand-rolled-sse-parser-in-llm-deepseek-with-eventsource-pars.md): The source note links to this decision directly.
- **`source-link`** — [0684. Adopt execa for hand-rolled test subprocess plumbing](0684-adopt-execa-for-hand-rolled-test-subprocess-plumbing.md): The source note links to this decision directly.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/types.ts`.
- **`shares-code-with`** — [0306. Classify pi-ai transport truncations from flattened message text](0306-classify-pi-ai-transport-truncations-from-flattened-message-text.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/types.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/types.ts`.
- **`shares-code-with`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): Shares source implementation: `packages/sdk/server/src/index.ts`, `packages/util/timeout/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md`.
