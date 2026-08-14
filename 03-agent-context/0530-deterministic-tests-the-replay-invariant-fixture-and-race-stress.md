---
id: "dsh-note-0530"
title: "Deterministic tests, the replay invariant fixture, and race stress"
status: "proposed"
class: "testing"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/testing/2026-06-11-deterministic-and-stress-testing.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/testing"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/session-state"
  - "domain/testing"
  - "lifecycle/proposed"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
aliases:
  - "setTimeout"
  - "waitForIdle"
  - "waitForStatus"
  - "waitForEvent"
  - "packages/*/tests"
  - "deriveMessages"
  - "vitest --repeat=200"
  - "--shuffle"
  - "--repeat"
  - "Deterministic tests, the replay invariant fixture, and race stress"
  - "testing"
  - "boundary"
  - "concurrency"
  - "evidence"
search_regex: "(?i)(setTimeout|waitForIdle|waitForStatus|waitForEvent|packages/\\*/tests|deriveMessages|vitest[- ]\\-\\-repeat=200|\\-\\-shuffle)"
---

# 0530. Deterministic tests, the replay invariant fixture, and race stress — implementation context

## Open this when

Several loop tests synchronize with setTimeout(30) sleeps --- flakiness debt that wastes agent cycles on retries and can mask ordering bugs. Separately, our core architectural promise (any session log replays to identical derived history) is asserted in two tests but is cheap to assert everywhere. And the inbox wakeup race was verified by hand exactly once; nothing re-verifies it continuously.

## Source decision

Three measures: No wall-clock sleeps in tests. Replace setTimeout(N) waits with event-driven waits (the existing waitForIdle pattern, extended to waitForStatus, waitForEvent(n)) or vitest fake timers where time itself is under test. Enforce with a lint rule banning setTimeout in packages//tests outside an allowlisted helper module. Universal replay fixture. A shared test helper wraps the loop harness so that after every test, the agent's session log is replayed into a fresh Session and deriveMessages() equality is asserted automatically.

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/testing/2026-06-11-deterministic-and-stress-testing.md](../02-notes/proposed/testing/2026-06-11-deterministic-and-stress-testing.md)
- Pinned source: [.agents/notes/proposed/testing/2026-06-11-deterministic-and-stress-testing.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/testing/2026-06-11-deterministic-and-stress-testing.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/session) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/session/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`packages/core/session/tests/fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/fork.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/session/tests/surface.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/surface.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/session/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/properties.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/session/tests/derived-cache.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/derived-cache.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- Source verification intent: No setTimeout remains in packages//tests outside the allowlisted helper module, enforced by the lint rule. The shared harness replays every test's session log into a fresh Session and asserts deriveMessages() equality automatically, across the whole suite. The nightly job runs the agent-loop and inbox suites with --repeat and --shuffle; a flake it finds is triaged as a bug, never retried away.

## How to read the implementation

1. Start with [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/testing`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/recovery`, `domain/build-release`, `domain/session-state`, `domain/testing`, `lifecycle/proposed`, `mechanism/event-sourcing`, `mechanism/policy`
- Aliases: `setTimeout`, `waitForIdle`, `waitForStatus`, `waitForEvent`, `packages/*/tests`, `deriveMessages`, `vitest --repeat=200`, `--shuffle`, `--repeat`, `Deterministic tests, the replay invariant fixture, and race stress`, `testing`, `boundary`, `concurrency`, `evidence`
- Regex: `(?i)(setTimeout|waitForIdle|waitForStatus|waitForEvent|packages/\*/tests|deriveMessages|vitest[- ]\-\-repeat=200|\-\-shuffle)`

```bash
rg -n --pcre2 "(?i)(setTimeout|waitForIdle|waitForStatus|waitForEvent|packages/\\*/tests|deriveMessages|vitest[- ]\\-\\-repeat=200|\\-\\-shuffle)" source-deepseek-harness
rg -l --fixed-strings "class/testing" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0447. Fold trace-only session facts into load-bearing events](0447-fold-trace-only-session-facts-into-load-bearing-events.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0635. Generated persistence log event catalog](0635-generated-persistence-log-event-catalog.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0311. Load sessions persisted before message identity](0311-load-sessions-persisted-before-message-identity.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0665. Use one surface manager per session](0665-use-one-surface-manager-per-session.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0466. Remove synthetic turns for log-only events](0466-remove-synthetic-turns-for-log-only-events.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0102. Reusable Session preparation before publication](0102-reusable-session-preparation-before-publication.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0318. Fixed header, sticky composer inside the transcript scrollport](0318-fixed-header-sticky-composer-inside-the-transcript-scrollport.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0504. Frame-coalesced reasoning-chunk publication and browser stress validation](0504-frame-coalesced-reasoning-chunk-publication-and-browser-stress-validatio.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0530-deterministic-tests-the-replay-invariant-fixture-and-race-stress.md`.
