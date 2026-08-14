---
id: "dsh-note-0100"
title: "Large-session JSONL restore pipeline"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-05-large-session-jsonl-restore-pipeline.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/streaming"
aliases:
  - "assertSessionEventEnvelope"
  - "events"
  - "pending"
  - "SessionLogScanner"
  - "ZstdFrameDecoder"
  - "Session.fromRestore"
  - "fromRestore"
  - "zstdDecompressSync"
  - "Buffer.indexOf"
  - "JSON.parse"
  - "turn/end"
  - "for...in"
  - "WeakSet"
  - "Session.events"
search_regex: "(?i)(assertSessionEventEnvelope|events|pending|SessionLogScanner|ZstdFrameDecoder|Session\\.fromRestore|fromRestore|zstdDecompressSync)"
---

# 0100. Large-session JSONL restore pipeline — implementation context

## Open this when

Restoring a stored session activates it and materializes its complete authoritative event log before the agent can run. Large JSONL artifacts made that one-time operation pay several avoidable costs: each independent Zstandard frame created and closed a decoder context, decoded plaintext was accumulated and rescanned as whole-log buffers and strings, and freshly parsed events went through generic snapshot and deep-freeze paths designed for borrowed or cyclic values. A representative profile contained 61.8 MiB of Zstandard data, 97.1 MiB of plaintext, and 1,307,073 events.

## Source decision

Restoration is one ownership-transfer pipeline from the persistence artifact into Session.fromRestore. The compressed artifact remains the source buffer, while each decoding and scanning stage consumes the previous stage's output incrementally without retaining a whole-log plaintext or parsed copy; the resulting event array is the only complete decoded representation. The structural Zstandard scanner identifies complete frame ranges before decoding. The dedicated first frame is decoded and parsed separately as the session header; subsequent plaintext frames are yielded in order into the JSONL scanner.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-05-large-session-jsonl-restore-pipeline.md](../02-notes/implemented/architecture/2026-08-05-large-session-jsonl-restore-pipeline.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-05-large-session-jsonl-restore-pipeline.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-05-large-session-jsonl-restore-pipeline.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `pending`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/fiber.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts) | runtime implementation | Defines `pending`, a construct named by the note. | `symbol-definition` |
| [`packages/lsp/lsp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp/src/index.ts) | package entry point | Defines `pending`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/client/src/api.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/api.ts) | runtime implementation | Defines `events`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/server/src/server.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/server.ts) | runtime implementation | Defines `pending`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Defines `events`, a construct named by the note. Defines `assertSessionEventEnvelope`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts) | package entry point | Defines `events`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-deepseek/src/sse.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/sse.ts) | runtime implementation | Defines `events`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-pi-ai/src/adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts) | provider/backend adapter | Defines `events`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-persistence-jsonl/src/zstd.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/zstd.ts) | runtime implementation | Defines `ZstdFrameDecoder`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-persistence-jsonl/src/format.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/format.ts) | runtime implementation | Defines `SessionLogScanner`, a construct named by the note. | `symbol-definition` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `turn/end` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `assertSessionEventEnvelope` | `function` | [`packages/core/session/src/index.ts:213`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L213) | `function assertSessionEventEnvelope(value: Record<string, unknown>, index: number): asserts value is SessionEvent {` |
| `events` | `const` | [`packages/core/session/src/index.ts:1098`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L1098) | `const events = session.events` |
| `events` | `const` | [`packages/llm/llm-deepseek/src/sse.ts:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/sse.ts#L32) | `const events = stream` |
| `events` | `const` | [`packages/llm/llm-pi-ai/src/adapter.ts:313`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts#L313) | `const events = snapshot.models.streamSimple(model, context, {` |
| `pending` | `const` | [`packages/lsp/lsp/src/index.ts:108`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp/src/index.ts#L108) | `const pending = new Map<string, Route>()` |
| `events` | `const` | [`packages/sdk/client/src/api.ts:150`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/api.ts#L150) | `const events: SessionEvent[] = []` |
| `pending` | `const` | [`packages/sdk/server/src/server.ts:207`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/server.ts#L207) | `const pending = this.sessionCreations.get(sessionId)` |
| `SessionLogScanner` | `class` | [`packages/session/session-persistence-jsonl/src/format.ts:272`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/format.ts#L272) | `export class SessionLogScanner {` |
| `ZstdFrameDecoder` | `interface` | [`packages/session/session-persistence-jsonl/src/zstd.ts:125`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/zstd.ts#L125) | `export interface ZstdFrameDecoder {` |
| `events` | `const` | [`packages/typert/loader/src/index.ts:111`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts#L111) | `const events = requireArray(pkgName, model.events, 'TYPERT.model.events')` |
| `pending` | `const` | [`vendor/cordis/src/fiber.ts:494`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts#L494) | `const pending = Promise.resolve(result).finally(() => {` |
| `pending` | `const` | [`vendor/hmr/src/index.ts:346`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L346) | `const pending: string[] = []` |
| `pending` | `const` | [`vendor/hmr/src/index.ts:403`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L403) | `const pending = new Map<ModuleJob, Plugin>()` |

### Tests and executable evidence

- [`scripts/test-invariants.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-invariants.ts) — A test under the owning area exercises or imports `WeakSet`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `switch`.
- [`scripts/change-scope.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/change-scope.spec.ts) — A test under the owning area exercises or imports `switch`.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `switch`.
- [`apps/web/tests/default-model.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/default-model.e2e.ts) — A test under the owning area exercises or imports `switch`.
- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — A test under the owning area exercises or imports `switch`.
- [`apps/web/tests/settings-chrome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/settings-chrome.e2e.ts) — A test under the owning area exercises or imports `switch`.
- [`apps/web/tests/models-settings.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/models-settings.e2e.ts) — A test under the owning area exercises or imports `switch`.

## How to read the implementation

1. Start with [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/agent-loop`, `domain/build-release`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/storage`, `lifecycle/implemented`, `mechanism/streaming`
- Aliases: `assertSessionEventEnvelope`, `events`, `pending`, `SessionLogScanner`, `ZstdFrameDecoder`, `Session.fromRestore`, `fromRestore`, `zstdDecompressSync`, `Buffer.indexOf`, `JSON.parse`, `turn/end`, `for...in`, `WeakSet`, `Session.events`
- Regex: `(?i)(assertSessionEventEnvelope|events|pending|SessionLogScanner|ZstdFrameDecoder|Session\.fromRestore|fromRestore|zstdDecompressSync)`

```bash
rg -n --pcre2 "(?i)(assertSessionEventEnvelope|events|pending|SessionLogScanner|ZstdFrameDecoder|Session\\.fromRestore|fromRestore|zstdDecompressSync)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0308. Recursive Python SDK session notifications](0308-recursive-python-sdk-session-notifications.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/llm/llm-deepseek/src/sse.ts`.
- **`shares-code-with`** — [0364. Owned-run finish reason reporting](0364-owned-run-finish-reason-reporting.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/sdk/client/src/api.ts`.
- **`shares-code-with`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): Shares source implementation: `packages/sdk/server/src/server.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0587. TUI prompt themes compose mutable plugin values](0587-tui-prompt-themes-compose-mutable-plugin-values.md): Shares source implementation: `packages/llm/llm-pi-ai/src/adapter.ts`, `packages/typert/loader/src/index.ts`.
- **`shares-code-with`** — [0061. Web client Agent-scope parity model and the provisioning channel (agents/scope / blank reuse / provide)](0061-web-client-agent-scope-parity-model-and-the-provisioning-channel-agents.md): Shares source implementation: `packages/core/session/src/index.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0052. The slot system standard --- single register, four props shares, and the framework store seat](0052-the-slot-system-standard-single-register-four-props-shares-and-the-frame.md): Shares source implementation: `vendor/cordis/src/fiber.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0189. Code Mode live dispatch lifecycle and native-contract parallelism](0189-code-mode-live-dispatch-lifecycle-and-native-contract-parallelism.md): Shares source implementation: `packages/core/session/src/index.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0141. SessionStore fork API](0141-sessionstore-fork-api.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/sdk/server/src/server.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0100-large-session-jsonl-restore-pipeline.md`.
