---
id: "dsh-note-0678"
title: "Record fork and mixed spawn+fork snapshot scenarios"
status: "archived"
class: "testing"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/testing/2026-06-22-fork-snapshot-scenarios.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/testing"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
aliases:
  - "subagent"
  - "createdAt"
  - "slice"
  - "dsh-llm-replay"
  - "seedLength"
  - "llm-replay"
  - "acp-agent"
  - "subagent-spawn"
  - "subagent-multi"
  - "cordis.yml"
  - "cordis.snapshot.yml"
  - "subagent_fork"
  - "subagent-fork"
  - "subagent-mixed"
search_regex: "(?i)(subagent|createdAt|slice|dsh\\-llm\\-replay|seedLength|llm\\-replay|acp\\-agent|subagent\\-spawn)"
---

# 0678. Record fork and mixed spawn+fork snapshot scenarios — implementation context

## Open this when

The seed-boundary Agent Note made fork-child replay route correctly: dsh-llm-replay derives a child's script from the events at or after its persisted seedLength boundary, so a fork child's inherited parent prefix is not replayed as the child's own model calls. But it shipped with no recorded fork scenario --- the slice was exercised only by llm-replay's unit tests (a synthetic child fixture) and a persistence round-trip test. The full-transcript snapshot tier, the one net that boots the real acp-agent and replays an end-to-end nested transcript, had only spawn children (subagent-spawn, subagent-multi).

## Source decision

Record two scenarios against the real API, both replayed keyless in the default gate: subagent-fork --- the parent completes a turn that establishes a fact, then delegates one subtask via subagent_fork. The fork child inherits the conversation (its log carries a non-zero seedLength), so it can answer from the parent's context. This is the focused regression: the child fixture's seedLength is the boundary the replay slice depends on, recorded from a real fork rather than hand-synthesized.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/testing/2026-06-22-fork-snapshot-scenarios.md](../02-notes/archived/testing/2026-06-22-fork-snapshot-scenarios.md)
- Pinned source: [.agents/notes/archived/testing/2026-06-22-fork-snapshot-scenarios.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/testing/2026-06-22-fork-snapshot-scenarios.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/subagent/subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/test-support/llm-replay/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`packages/test-support/llm-replay/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`examples/acp-agent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/subagent/subagent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/test-support/llm-replay`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `createdAt`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-subagent/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-subagent/src/client/index.ts) | package entry point | Defines `subagent`, a construct named by the note. | `symbol-definition` |
| [`packages/subprocess/subprocess-local/src/spawn.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts) | runtime implementation | Defines `slice`, a construct named by the note. | `symbol-definition` |
| [`examples/acp-agent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/README.md) | package contract and examples | Core file in the package named by the note: `examples/acp-agent`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `subagent` | `const` | [`packages/client/ui-subagent/src/client/index.ts:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-subagent/src/client/index.ts#L45) | `const subagent = owner.session?.subagent` |
| `createdAt` | `const` | [`packages/goal/goal/src/fold.ts:160`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L160) | `const createdAt = nonNegativeInteger(value['createdAt'], 'createdAt')` |
| `slice` | `const` | [`packages/subprocess/subprocess-local/src/spawn.ts:211`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts#L211) | `const slice = lossy ? buffer : buffer.subarray(fromByte - windowStart)` |

### Tests and executable evidence

- [`examples/acp-agent/tests/acp.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/acp.snapshot.ts) — A test under the owning area exercises or imports `subagent-multi`. A test under the owning area exercises or imports `subagent-mixed`.
- [`packages/test-support/llm-replay/tests/llm-replay.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/tests/llm-replay.spec.ts) — A test under the owning area exercises or imports `seedLength`. A test under the owning area exercises or imports `llm-replay`.
- [`examples/acp-agent/tests/snapshots/subagent-mixed/input.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/snapshots/subagent-mixed/input.json) — A test under the owning area exercises or imports `subagent_fork`.
- [`examples/acp-agent/tests/snapshots/subagent-mixed/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/snapshots/subagent-mixed/session.jsonl) — A test under the owning area exercises or imports `subagent_fork`.
- [`examples/acp-agent/tests/snapshots/subagent-fork-in-process/input.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/snapshots/subagent-fork-in-process/input.json) — A test under the owning area exercises or imports `subagent_fork`.
- [`examples/acp-agent/tests/snapshots/pty-tools/tool-schemas.expected.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/snapshots/pty-tools/tool-schemas.expected.json) — A test under the owning area exercises or imports `subagent_fork`.
- [`examples/acp-agent/tests/snapshots/web-fetch/tool-schemas.expected.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/snapshots/web-fetch/tool-schemas.expected.json) — A test under the owning area exercises or imports `subagent_fork`.
- [`examples/acp-agent/tests/snapshots/text-turn/tool-schemas.expected.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/snapshots/text-turn/tool-schemas.expected.json) — A test under the owning area exercises or imports `subagent_fork`.

## How to read the implementation

1. Start with [`packages/subagent/subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/testing`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/policy`
- Aliases: `subagent`, `createdAt`, `slice`, `dsh-llm-replay`, `seedLength`, `llm-replay`, `acp-agent`, `subagent-spawn`, `subagent-multi`, `cordis.yml`, `cordis.snapshot.yml`, `subagent_fork`, `subagent-fork`, `subagent-mixed`
- Regex: `(?i)(subagent|createdAt|slice|dsh\-llm\-replay|seedLength|llm\-replay|acp\-agent|subagent\-spawn)`

```bash
rg -n --pcre2 "(?i)(subagent|createdAt|slice|dsh\\-llm\\-replay|seedLength|llm\\-replay|acp\\-agent|subagent\\-spawn)" source-deepseek-harness
rg -l --fixed-strings "class/testing" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0280. Parallel subagent delegations](0280-parallel-subagent-delegations.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/invariant.ts`.
- **`shares-code-with`** — [0285. Delegated subagents run with approvals pinned to `'never'`](0285-delegated-subagents-run-with-approvals-pinned-to-never.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0362. One selection rule keeps subagent output past an empty terminal message](0362-one-selection-rule-keeps-subagent-output-past-an-empty-terminal-message.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0263. The continuable child return channel is an obligation](0263-the-continuable-child-return-channel-is-an-obligation.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0148. Background subagent tasks](0148-background-subagent-tasks.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0661. Trim unreachable ACP bridge surface --- the branding knobs and the kind-sniffing fallback](0661-trim-unreachable-acp-bridge-surface-the-branding-knobs-and-the-kind-snif.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0462. Merge subagent control into the subagent service](0462-merge-subagent-control-into-the-subagent-service.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0442. Documentation-site navigation and repository chrome](0442-documentation-site-navigation-and-repository-chrome.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0678-record-fork-and-mixed-spawn-fork-snapshot-scenarios.md`.
