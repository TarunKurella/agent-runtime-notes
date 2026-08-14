---
id: "dsh-note-0497"
title: "Persist the seed boundary so fork-child replay routes correctly"
status: "implemented"
class: "testing"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/testing/2026-06-22-fork-child-replay-seed-boundary.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/testing"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "sessions"
  - "meta"
  - "length"
  - "SessionHeader"
  - "CreateSessionOptions"
  - "version"
  - "createdAt"
  - "stream"
  - "toHeaderLine"
  - "fromHeaderLine"
  - "SCHEMA_VERSION"
  - "log"
  - "parseSessionLog"
  - "parseSessionHeader"
search_regex: "(?i)(sessions|meta|length|SessionHeader|CreateSessionOptions|version|createdAt|stream)"
---

# 0497. Persist the seed boundary so fork-child replay routes correctly — implementation context

## Open this when

The per-session snapshot replay Agent Note made the snapshot tier express a nested-agent shape: a parent plus one recorded log per in-process subagent, each replayed as its own script keyed by calling session. It noted (§ Scope, final bullet) that a fork snapshot was "a trivial future addition, not a gap in the keying." That was wrong about a fork child specifically --- not the keying, but the script derivation. A subagent script is derived from a recorded session log by deriveReplayScript: it groups the log's assistant/chunk events by (turn, step) into one replay entry per stream() call.

## Source decision

Record where a session's inherited prefix ends, persist it, and have the replay harness derive a child's script from its own events only. SessionHeader gains an optional seedLength: number --- how many leading events were inherited via a seed rather than produced by this session. The fork backend stamps it (= the seeded-prefix length) when it creates the child; a fresh spawn leaves it absent (≡ 0). It is threaded through CreateSessionOptions.meta (and CreateAgentOptions.meta), set in SessionStore.prepare. seedLength is explicit, never inferred from seed.length.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/testing/2026-06-22-fork-child-replay-seed-boundary.md](../02-notes/implemented/testing/2026-06-22-fork-child-replay-seed-boundary.md)
- Pinned source: [.agents/notes/implemented/testing/2026-06-22-fork-child-replay-seed-boundary.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/testing/2026-06-22-fork-child-replay-seed-boundary.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/json.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/json.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `length`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. Defines `meta`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. Defines `SessionHeader`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/test-support/llm-replay/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/test-support/llm-replay`. Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/test-support/llm-replay/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/test-support/llm-replay`. Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-directory-member, named-package-member` |
| [`packages/subagent/subagent-in-process-driver/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-in-process-driver/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/subagent/subagent-in-process-driver`. Core file in the package named by the note: `packages/subagent/subagent-in-process-driver`. | `named-directory-member, named-package-member` |
| [`packages/subagent/subagent-in-process-driver/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-in-process-driver/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/subagent/subagent-in-process-driver`. Core file in the package named by the note: `packages/subagent/subagent-in-process-driver`. | `named-directory-member, named-package-member` |
| [`packages/test-support/llm-replay/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/test-support/llm-replay`. Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-directory-member, named-package-member` |
| [`packages/test-support/llm-replay/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/test-support/llm-replay`. Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-directory-member, named-package-member` |
| [`packages/subagent/subagent-in-process-driver/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-in-process-driver/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/subagent/subagent-in-process-driver`. Core file in the package named by the note: `packages/subagent/subagent-in-process-driver`. | `named-directory-member, named-package-member` |
| [`packages/subagent/subagent-in-process-driver/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-in-process-driver/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/subagent/subagent-in-process-driver`. Core file in the package named by the note: `packages/subagent/subagent-in-process-driver`. | `named-directory-member, named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `sessions` | `const` | [`packages/acp/acp/src/index.ts:110`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L110) | `const sessions = new Map<SessionId, SessionRecord>()` |
| `meta` | `const` | [`packages/core/session/src/index.ts:876`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L876) | `const meta = options?.meta` |
| `length` | `const` | [`packages/core/session/src/json.ts:136`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/json.ts#L136) | `const length = current.length` |
| `SessionHeader` | `interface` | [`packages/core/session/src/types.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L61) | `export interface SessionHeader {` |
| `CreateSessionOptions` | `interface` | [`packages/core/session/src/types.ts:106`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L106) | `export interface CreateSessionOptions {` |
| `version` | `const` | [`packages/e2b/fs-e2b/src/index.ts:389`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L389) | `const version = await this.writeAtomic(` |
| `createdAt` | `const` | [`packages/goal/goal/src/fold.ts:160`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L160) | `const createdAt = nonNegativeInteger(value['createdAt'], 'createdAt')` |
| `stream` | `const` | [`packages/llm/llm/src/index.ts:865`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L865) | `const stream = adapter.stream(this.forAdapter(resolvedOptions, adapter))` |
| `toHeaderLine` | `function` | [`packages/session/session-persistence-jsonl/src/format.ts:51`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/format.ts#L51) | `export function toHeaderLine(header: SessionHeader): HeaderLine {` |
| `fromHeaderLine` | `function` | [`packages/session/session-persistence-jsonl/src/format.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/format.ts#L71) | `export function fromHeaderLine(line: HeaderLine): SessionHeader {` |
| `SCHEMA_VERSION` | `const` | [`packages/session/session-persistence-sqlite/src/schema.ts:20`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-sqlite/src/schema.ts#L20) | `export const SCHEMA_VERSION = 15` |
| `log` | `const` | [`packages/test-support/acp-snapshot/src/suite.ts:1298`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts#L1298) | `const log = result.sessionLogs[index]` |
| `parseSessionLog` | `function` | [`packages/test-support/llm-replay/src/index.ts:167`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/index.ts#L167) | `export function parseSessionLog(text: string): SessionEvent[] {` |
| `parseSessionHeader` | `function` | [`packages/test-support/llm-replay/src/index.ts:183`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/index.ts#L183) | `export function parseSessionHeader(text: string): { id: string; createdAt: number; seedLength: number } {` |
| `deriveReplayScript` | `function` | [`packages/test-support/llm-replay/src/index.ts:206`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/index.ts#L206) | `export function deriveReplayScript(events: SessionEvent[]): ReplayEntry[] {` |
| `loadSessionScripts` | `function` | [`packages/test-support/llm-replay/src/index.ts:516`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/index.ts#L516) | `export function loadSessionScripts(config: ReplayConfig): SessionScript[] {` |

### Tests and executable evidence

- [`packages/core/session/tests/fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/fork.spec.ts) — A test under the owning area exercises or imports `seedLength`.
- [`packages/core/session/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/scoped.spec.ts) — A test under the owning area exercises or imports `prepare`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `seedLength`. A test under the owning area exercises or imports `prepare`.
- [`packages/test-support/llm-replay/tests/llm-replay.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/tests/llm-replay.spec.ts) — A test under the owning area exercises or imports `deriveReplayScript`. A test under the owning area exercises or imports `seedLength`.
- [`packages/session/session-persistence-jsonl/tests/zstd.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/tests/zstd.spec.ts) — A test under the owning area exercises or imports `toHeaderLine`.
- [`packages/session/session-persistence-jsonl/tests/jsonl.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/tests/jsonl.spec.ts) — A test under the owning area exercises or imports `toHeaderLine`.
- [`packages/session/session-persistence-sqlite/tests/sqlite.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-sqlite/tests/sqlite.spec.ts) — A test under the owning area exercises or imports `SCHEMA_VERSION`.
- [`apps/web/tests/queue-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/queue-actions.e2e.ts) — Contains the exact code literal `dsh-llm-replay` named by the note.

## How to read the implementation

1. Start with [`packages/core/session/src/json.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/json.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/testing`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/agent-loop`, `domain/build-release`, `domain/llm`, `domain/multi-agent`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `sessions`, `meta`, `length`, `SessionHeader`, `CreateSessionOptions`, `version`, `createdAt`, `stream`, `toHeaderLine`, `fromHeaderLine`, `SCHEMA_VERSION`, `log`, `parseSessionLog`, `parseSessionHeader`
- Regex: `(?i)(sessions|meta|length|SessionHeader|CreateSessionOptions|version|createdAt|stream)`

```bash
rg -n --pcre2 "(?i)(sessions|meta|length|SessionHeader|CreateSessionOptions|version|createdAt|stream)" source-deepseek-harness
rg -l --fixed-strings "class/testing" 03-agent-context
```

## Connected notes

- **`source-link`** — [0678. Record fork and mixed spawn+fork snapshot scenarios](0678-record-fork-and-mixed-spawn-fork-snapshot-scenarios.md): The source note links to this decision directly.
- **`source-link`** — [0498. Per-session snapshot replay for nested agents](0498-per-session-snapshot-replay-for-nested-agents.md): The source note links to this decision directly.
- **`shares-code-with`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0533. Persist assembled assistant messages, not stream chunks](0533-persist-assembled-assistant-messages-not-stream-chunks.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/test-support/llm-replay/README.md`.
- **`shares-code-with`** — [0398. GUI testing system --- the three-tier structure](0398-gui-testing-system-the-three-tier-structure.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0061. Web client Agent-scope parity model and the provisioning channel (agents/scope / blank reuse / provide)](0061-web-client-agent-scope-parity-model-and-the-provisioning-channel-agents.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0518. Recallable compaction --- index checkpoints, a state checkpoint, and in-session history recall](0518-recallable-compaction-index-checkpoints-a-state-checkpoint-and-in-sessio.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0497-persist-the-seed-boundary-so-fork-child-replay-routes-correctly.md`.
