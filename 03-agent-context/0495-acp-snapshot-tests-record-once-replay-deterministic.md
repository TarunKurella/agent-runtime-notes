---
id: "dsh-note-0495"
title: "ACP snapshot tests --- record-once / replay-deterministic"
status: "implemented"
class: "testing"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/testing/2026-06-19-acp-snapshot-tests.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/testing"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "json"
  - "refresh"
  - "rawOutput"
  - "record"
  - "finish"
  - "ReplayEntry"
  - "deriveReplayScript"
  - "jsonl"
  - "session.jsonl"
  - "assistant/chunk"
  - "llm-replay"
  - "llm/stream"
  - "compaction/summary"
  - "llmStreamCall: true"
search_regex: "(?i)(json|refresh|rawOutput|record|finish|ReplayEntry|deriveReplayScript|jsonl)"
---

# 0495. ACP snapshot tests --- record-once / replay-deterministic — implementation context

## Open this when

Unit tests do not exercise the complete assembled-agent subprocess or its ACP automation wire, while real-API tests are nondeterministic and key-gated. Loader wiring, backend behavior, and protocol output can therefore regress despite green unit coverage, as the default-export postmortem demonstrated. The blocker for a full-transcript test is the model: the agent's output is driven by a non-deterministic LLM, and a key-gated test that hits the real API on every run is neither deterministic nor CI-runnable. The tier needs the fidelity of a real run with the determinism of a fixture.

## Source decision

A snapshot test boots the real ACP example, drives its stdio protocol from a deterministic script, and compares normalized output with committed expected outputs. A session log recorded once from the real API supplies all later model streams. The fixture is the product's ordinary persisted JSONL. Each scenario's session.jsonl is harvested from a real run. assistant/chunk events reproduce the model streams; tool, message, and boundary events capture the harness behavior. One ordinary session artifact therefore serves as both replay source and behavioral expected output.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/testing/2026-06-19-acp-snapshot-tests.md](../02-notes/implemented/testing/2026-06-19-acp-snapshot-tests.md)
- Pinned source: [.agents/notes/implemented/testing/2026-06-19-acp-snapshot-tests.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/testing/2026-06-19-acp-snapshot-tests.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/postmortem/0001-acp-default-export-drops-inject.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/postmortem/0001-acp-default-export-drops-inject.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. Defines `record`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/test-support/llm-replay/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/llm-replay`. Defines `deriveReplayScript`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/test-support/llm-replay/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`packages/core/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/session) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/llm-deepseek`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/test-support/llm-replay`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `refresh` | `const` | [`packages/client/ui-agent-preset/src/client/index.ts:77`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-agent-preset/src/client/index.ts#L77) | `const refresh = (): void => {` |
| `rawOutput` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:304`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L304) | `const rawOutput = request.rawOutput ?? request.summary` |
| `record` | `const` | [`packages/core/session/src/index.ts:100`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L100) | `const record = input as Record<string, unknown>` |
| `record` | `const` | [`packages/core/session/src/index.ts:255`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L255) | `const record = typeof data === 'object' && data !== null` |
| `record` | `const` | [`packages/core/session/src/index.ts:306`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L306) | `const record = typeof data === 'object' && data !== null` |
| `finish` | `const` | [`packages/core/tools/src/ts-types.ts:115`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/ts-types.ts#L115) | `const finish = (document: TypeDocument): void => {` |
| `ReplayEntry` | `type` | [`packages/test-support/llm-replay/src/index.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/index.ts#L36) | `export type ReplayEntry =` |
| `deriveReplayScript` | `function` | [`packages/test-support/llm-replay/src/index.ts:206`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/index.ts#L206) | `export function deriveReplayScript(events: SessionEvent[]): ReplayEntry[] {` |
| `jsonl` | `const` | [`scripts/gen-doc-graphs.ts:731`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts#L731) | `const jsonl = nodeId('bundle', 'jsonl')` |

### Tests and executable evidence

- [`packages/test-support/llm-replay/tests/llm-replay.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/tests/llm-replay.spec.ts) — A test under the owning area exercises or imports `llm-replay`. A test under the owning area exercises or imports `deriveReplayScript`.
- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — Contains the exact code literal `compaction/summary` named by the note.
- [`scripts/session-fixture-layout.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/session-fixture-layout.spec.ts) — Contains the exact code literal `assistant/chunk` named by the note.

## How to read the implementation

1. Start with [`docs/postmortem/0001-acp-default-export-drops-inject.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/postmortem/0001-acp-default-export-drops-inject.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

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

- Tags: `class/testing`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `json`, `refresh`, `rawOutput`, `record`, `finish`, `ReplayEntry`, `deriveReplayScript`, `jsonl`, `session.jsonl`, `assistant/chunk`, `llm-replay`, `llm/stream`, `compaction/summary`, `llmStreamCall: true`
- Regex: `(?i)(json|refresh|rawOutput|record|finish|ReplayEntry|deriveReplayScript|jsonl)`

```bash
rg -n --pcre2 "(?i)(json|refresh|rawOutput|record|finish|ReplayEntry|deriveReplayScript|jsonl)" source-deepseek-harness
rg -l --fixed-strings "class/testing" 03-agent-context
```

## Connected notes

- **`source-link`** — [0680. Single-source the acp-agent replay config](0680-single-source-the-acp-agent-replay-config.md): The source note links to this decision directly.
- **`source-link`** — [0681. Pin request-header content in one snapshot scenario](0681-pin-request-header-content-in-one-snapshot-scenario.md): The source note links to this decision directly.
- **`source-link`** — [0007. Capability seams --- Service Definition / Service Provider / Consumer roles](0007-capability-seams-service-definition-service-provider-consumer-roles.md): The source note links to this decision directly.
- **`source-link`** — [0459. ACP as an automation-only protocol](0459-acp-as-an-automation-only-protocol.md): The source note links to this decision directly.
- **`shares-code-with`** — [0497. Persist the seed boundary so fork-child replay routes correctly](0497-persist-the-seed-boundary-so-fork-child-replay-routes-correctly.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0466. Remove synthetic turns for log-only events](0466-remove-synthetic-turns-for-log-only-events.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0398. GUI testing system --- the three-tier structure](0398-gui-testing-system-the-three-tier-structure.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0495-acp-snapshot-tests-record-once-replay-deterministic.md`.
