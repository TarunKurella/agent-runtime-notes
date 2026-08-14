---
id: "dsh-note-0677"
title: "Use `session.jsonl` as the only snapshot session-log artifact"
status: "archived"
class: "testing"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/testing/2026-06-20-remove-redundant-snapshot-log-expected-output.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/testing"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/evidence"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
aliases:
  - "json"
  - "cancel"
  - "normalizeSessionLog"
  - "fixtureContext"
  - "ReplayEntry"
  - "jsonl"
  - "session.jsonl"
  - "session.expected.jsonl"
  - "error-finish"
  - "replay.override.json"
  - "{ \"kind\": \"chunks\", \"chunks\": StreamChunk[] }"
  - "llm-replay"
  - "Use `session.jsonl` as the only snapshot session-log artifact"
  - "testing"
search_regex: "(?i)(json|cancel|normalizeSessionLog|fixtureContext|ReplayEntry|jsonl|session\\.jsonl|session\\.expected\\.jsonl)"
---

# 0677. Use `session.jsonl` as the only snapshot session-log artifact — implementation context

## Open this when

Model-driving ACP snapshot scenarios ship both session.jsonl and session.expected.jsonl. For normal recorded scenarios, session.jsonl is the replay fixture harvested from a real run, and the replay test normalizes the newly persisted log and compares it to session.expected.jsonl. In the current fixtures, the two normalized logs are identical for ordinary recorded scenarios. Authored override scenarios (error-finish, cancel) currently use replay.override.json to drive model behavior and keep session.jsonl as a minimal dummy fixture, while session.expected.jsonl holds the expected persisted log.

## Source decision

The session.expected.jsonl concept is removed entirely. Every scenario has at most one committed session-log artifact, session.jsonl: For recorded scenarios, session.jsonl remains the raw harvested log. Replay still derives model chunks from it, and the snapshot test compares the replay run's normalized persisted log against normalized session.jsonl. For authored override scenarios, replay.override.json drives model behavior and session.jsonl holds the expected produced session log.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/testing/2026-06-20-remove-redundant-snapshot-log-expected-output.md](../02-notes/archived/testing/2026-06-20-remove-redundant-snapshot-log-expected-output.md)
- Pinned source: [.agents/notes/archived/testing/2026-06-20-remove-redundant-snapshot-log-expected-output.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/testing/2026-06-20-remove-redundant-snapshot-log-expected-output.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/test-support/llm-replay/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/llm-replay`. Defines `ReplayEntry`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/test-support/llm-replay/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`packages/test-support/llm-replay`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay) | package or module directory | The note names this package or capability. | `named-package` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Defines `jsonl`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/acp-snapshot/src/suite.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts) | runtime implementation | Defines `fixtureContext`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/pointer-grace.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/pointer-grace.ts) | runtime implementation | Defines `cancel`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/acp-snapshot/src/normalize.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/normalize.ts) | runtime implementation | Defines `normalizeSessionLog`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/llm-replay/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/README.md) | package contract and examples | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`packages/test-support/llm-replay/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/package.json) | composition and configuration | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `cancel` | `const` | [`packages/client/ui-primitives/src/pointer-grace.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/pointer-grace.ts#L36) | `const cancel = useCallback(() => {` |
| `normalizeSessionLog` | `function` | [`packages/test-support/acp-snapshot/src/normalize.ts:297`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/normalize.ts#L297) | `export function normalizeSessionLog(` |
| `fixtureContext` | `function` | [`packages/test-support/acp-snapshot/src/suite.ts:368`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts#L368) | `export function fixtureContext(fixture: string): NormalizeContext {` |
| `ReplayEntry` | `type` | [`packages/test-support/llm-replay/src/index.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/index.ts#L36) | `export type ReplayEntry =` |
| `jsonl` | `const` | [`scripts/gen-doc-graphs.ts:731`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts#L731) | `const jsonl = nodeId('bundle', 'jsonl')` |

### Tests and executable evidence

- [`packages/test-support/acp-snapshot/tests/suite.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/tests/suite.spec.ts) — A test under the owning area exercises or imports `fixtureContext`.
- [`packages/test-support/llm-replay/tests/llm-replay.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/tests/llm-replay.spec.ts) — A test under the owning area exercises or imports `ReplayEntry`. A test under the owning area exercises or imports `llm-replay`.
- [`packages/test-support/acp-snapshot/tests/normalize.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/tests/normalize.spec.ts) — A test under the owning area exercises or imports `normalizeSessionLog`.
- Source verification intent: session.expected.jsonl appears nowhere in the snapshot harness, fixtures, orphan guards, or docs; the snapshot test derives the expected session log from session.jsonl for every model scenario; authored sidecar scenarios commit their expected produced log as session.jsonl with replay.override.json as the model-behavior override; and the orphan-fixture guards know which files each scenario kind requires. The ACP snapshot tests Agent Note describes the reduced fixture set.

## How to read the implementation

1. Start with [`packages/test-support/llm-replay/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/testing`, `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`, `concern/ownership`, `domain/build-release`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/archived`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/projection`
- Aliases: `json`, `cancel`, `normalizeSessionLog`, `fixtureContext`, `ReplayEntry`, `jsonl`, `session.jsonl`, `session.expected.jsonl`, `error-finish`, `replay.override.json`, `{ "kind": "chunks", "chunks": StreamChunk[] }`, `llm-replay`, `Use `session.jsonl` as the only snapshot session-log artifact`, `testing`
- Regex: `(?i)(json|cancel|normalizeSessionLog|fixtureContext|ReplayEntry|jsonl|session\.jsonl|session\.expected\.jsonl)`

```bash
rg -n --pcre2 "(?i)(json|cancel|normalizeSessionLog|fixtureContext|ReplayEntry|jsonl|session\\.jsonl|session\\.expected\\.jsonl)" source-deepseek-harness
rg -l --fixed-strings "class/testing" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0533. Persist assembled assistant messages, not stream chunks](0533-persist-assembled-assistant-messages-not-stream-chunks.md): Shares source implementation: `packages/test-support/llm-replay`, `packages/test-support/llm-replay/README.md`.
- **`shares-code-with`** — [0683. Snapshot semantic terminal state for the TUI](0683-snapshot-semantic-terminal-state-for-the-tui.md): Shares source implementation: `packages/test-support/llm-replay`, `packages/test-support/llm-replay/src/index.ts`.
- **`shares-code-with`** — [0682. Extract the ACP snapshot suite into a support package](0682-extract-the-acp-snapshot-suite-into-a-support-package.md): Shares source implementation: `packages/test-support/acp-snapshot/src/normalize.ts`, `packages/test-support/acp-snapshot/src/suite.ts`.
- **`shares-code-with`** — [0497. Persist the seed boundary so fork-child replay routes correctly](0497-persist-the-seed-boundary-so-fork-child-replay-routes-correctly.md): Shares source implementation: `packages/test-support/llm-replay/README.md`, `packages/test-support/llm-replay/package.json`.
- **`shares-code-with`** — [0678. Record fork and mixed spawn+fork snapshot scenarios](0678-record-fork-and-mixed-spawn-fork-snapshot-scenarios.md): Shares source implementation: `packages/test-support/llm-replay`, `packages/test-support/llm-replay/src/index.ts`.
- **`shares-code-with`** — [0681. Pin request-header content in one snapshot scenario](0681-pin-request-header-content-in-one-snapshot-scenario.md): Shares source implementation: `packages/test-support/acp-snapshot/src/normalize.ts`, `packages/test-support/acp-snapshot/src/suite.ts`.
- **`shares-code-with`** — [0571. Auto-titled terminal from the first message](0571-auto-titled-terminal-from-the-first-message.md): Shares source implementation: `packages/test-support/llm-replay`, `packages/test-support/llm-replay/src/index.ts`.
- **`shares-code-with`** — [0335. Goal-round wrap-up message](0335-goal-round-wrap-up-message.md): Shares source implementation: `packages/test-support/llm-replay`, `packages/test-support/llm-replay/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0677-use-session-jsonl-as-the-only-snapshot-session-log-artifact.md`.
