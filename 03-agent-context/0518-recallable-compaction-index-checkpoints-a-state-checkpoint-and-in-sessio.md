---
id: "dsh-note-0518"
title: "Recallable compaction --- index checkpoints, a state checkpoint, and in-session history recall"
status: "proposed"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/feature/2026-07-06-recallable-compaction.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/proposed"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "scanned"
  - "compaction"
  - "events"
  - "toolPairingBalancedBefore"
  - "toolPairingBalancedAfter"
  - "matched"
  - "truncated"
  - "shadowedRange"
  - "compaction/summary"
  - "chunkTokens"
  - "compactRegion"
  - "stubTokens"
  - "history_read"
  - "@deepseek-ai/dsh-tool-recall"
search_regex: "(?i)(scanned|compaction|events|toolPairingBalancedBefore|toolPairingBalancedAfter|matched|truncated|shadowedRange)"
---

# 0518. Recallable compaction --- index checkpoints, a state checkpoint, and in-session history recall — implementation context

## Open this when

Compaction is irreversible from the model's current context. The summary the model sees carries no reference to what it shadows --- shadowedRange lives only on the log-only compaction/summary event --- and no tool lets the model read a shadowed span back. Whatever the summarizer drops is unavailable to the model, even though the append-only log holds every byte. Repeated compaction compounds this: the head checkpoint is rewritten every pass, so the request prefix takes a full prompt-cache miss each time, and earlier summaries are re-summarized generation after generation.

## Source decision

Split the checkpoint into two classes and make shadowed history reachable. Newly stale history splits into chunks by deterministic policy: accumulate toward chunkTokens, snap edges with toolPairingBalancedBefore / toolPairingBalancedAfter, prefer turn boundaries, and place the final boundary as close to the retain boundary as balance allows, so the trailing slice shrinks to roughly one turn.

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/feature/2026-07-06-recallable-compaction.md](../02-notes/proposed/feature/2026-07-06-recallable-compaction.md)
- Pinned source: [.agents/notes/proposed/feature/2026-07-06-recallable-compaction.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/feature/2026-07-06-recallable-compaction.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. Defines `events`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/chunk-rows.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `events`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/compaction/compaction/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/index.ts) | package entry point | Core file in the package named by the note: `packages/compaction/compaction`. | `named-package-member` |
| [`packages/compaction/compaction/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/compaction/compaction`. | `named-package-member` |
| [`packages/compaction/compaction/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/compaction/compaction`. | `named-package-member` |
| [`packages/compaction/compaction-basic/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/index.ts) | package entry point | Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-package-member` |
| [`packages/compaction/compaction-basic/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-package-member` |
| [`packages/compaction/compaction/src/tool-pairing.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/tool-pairing.ts) | runtime implementation | Core file in the package named by the note: `packages/compaction/compaction`. Defines `toolPairingBalancedBefore`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/compaction/compaction-basic/src/region.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/region.ts) | runtime implementation | Core file in the package named by the note: `packages/compaction/compaction-basic`. Defines `events`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/compaction/compaction-basic/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `scanned` | `const` | [`packages/client/ui-directory-picker-browse/src/client/DirectoryBrowser.tsx:364`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-directory-picker-browse/src/client/DirectoryBrowser.tsx#L364) | `const scanned = useRef<ScannedDirectory \| null>(null)` |
| `compaction` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:339`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L339) | `const compaction: TurnBucket = {` |
| `events` | `const` | [`packages/compaction/compaction-basic/src/region.ts:503`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/region.ts#L503) | `const events = session.events` |
| `events` | `const` | [`packages/compaction/compaction/src/tool-pairing.ts:59`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/tool-pairing.ts#L59) | `const events = session.events` |
| `toolPairingBalancedBefore` | `function` | [`packages/compaction/compaction/src/tool-pairing.ts:117`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/tool-pairing.ts#L117) | `export function toolPairingBalancedBefore(session: Session, seq: number): boolean {` |
| `toolPairingBalancedAfter` | `function` | [`packages/compaction/compaction/src/tool-pairing.ts:129`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/tool-pairing.ts#L129) | `export function toolPairingBalancedAfter(session: Session, seq: number): boolean {` |
| `events` | `const` | [`packages/core/session/src/chunk-rows.ts:295`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts#L295) | `const events: SessionEvent[] = []` |
| `events` | `const` | [`packages/core/session/src/index.ts:1098`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L1098) | `const events = session.events` |
| `matched` | `const` | [`packages/test-support/acp-snapshot/src/harness.ts:608`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/harness.ts#L608) | `const matched = content?.split('\n').filter(Boolean).some((line) => {` |
| `truncated` | `const` | [`packages/web/tool-web/src/fetch.ts:314`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts#L314) | `const truncated = result.truncated \|\| rendered.sourceTruncated \|\| prefix.length > maxOutputChars` |

### Tests and executable evidence

- [`packages/core/session/tests/surface.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/surface.spec.ts) — A test under the owning area exercises or imports `shadowedSeqs`.
- [`packages/compaction/compaction/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/tests/invariant.spec.ts) — A test under the owning area exercises or imports `shadowedRange`. A test under the owning area exercises or imports `dsh-compaction`.
- [`packages/compaction/compaction/tests/compaction.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/tests/compaction.spec.ts) — A test under the owning area exercises or imports `shadowedRange`. A test under the owning area exercises or imports `compactRegion`.
- [`packages/compaction/compaction/tests/tool-pairing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/tests/tool-pairing.spec.ts) — A test under the owning area exercises or imports `toolPairingBalancedBefore`. A test under the owning area exercises or imports `toolPairingBalancedAfter`.
- [`packages/compaction/compaction-basic/tests/compaction-basic.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/compaction-basic.spec.ts) — A test under the owning area exercises or imports `shadowedRange`. A test under the owning area exercises or imports `toolPairingBalancedBefore`.
- [`packages/compaction/compaction-basic/tests/manual-compaction.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/manual-compaction.spec.ts) — A test under the owning area exercises or imports `compactRegion`. A test under the owning area exercises or imports `dsh-compaction`.
- [`packages/compaction/compaction-basic/tests/loader-composition.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/loader-composition.spec.ts) — A test under the owning area exercises or imports `compaction-basic`.
- [`packages/compaction/compaction-basic/tests/compaction-loop-repro.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/compaction-loop-repro.spec.ts) — A test under the owning area exercises or imports `toolPairingBalancedBefore`. A test under the owning area exercises or imports `toolPairingBalancedAfter`.
- Source verification intent: Auto-compaction over a long session yields [stubs…][state][tail] after every completed pass; prior stubs stay byte-identical across passes; committed stubs never fall inside a later region; the superseded state checkpoint folds without a tombstone, renders labeled, and stays reachable and searchable through the two-hop chain. Every checkpoint's surface text ends with the deterministic footer; footers round-trip through replay byte-identically; the state checkpoint's shadowedRange records its wider input range.

## How to read the implementation

1. Start with [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/proposed`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `scanned`, `compaction`, `events`, `toolPairingBalancedBefore`, `toolPairingBalancedAfter`, `matched`, `truncated`, `shadowedRange`, `compaction/summary`, `chunkTokens`, `compactRegion`, `stubTokens`, `history_read`, `@deepseek-ai/dsh-tool-recall`
- Regex: `(?i)(scanned|compaction|events|toolPairingBalancedBefore|toolPairingBalancedAfter|matched|truncated|shadowedRange)`

```bash
rg -n --pcre2 "(?i)(scanned|compaction|events|toolPairingBalancedBefore|toolPairingBalancedAfter|matched|truncated|shadowedRange)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0012. Session surface --- an ordered projection over the event log](0012-session-surface-an-ordered-projection-over-the-event-log.md): Shares source implementation: `packages/compaction/compaction/src/index.ts`, `packages/compaction/compaction/src/invariant.ts`.
- **`shares-code-with`** — [0316. The human transcript projects append-origin events](0316-the-human-transcript-projects-append-origin-events.md): Shares source implementation: `packages/compaction/compaction/src/index.ts`, `packages/compaction/compaction/src/invariant.ts`.
- **`shares-code-with`** — [0168. Cross-session references](0168-cross-session-references.md): Shares source implementation: `packages/compaction/compaction/src/index.ts`, `packages/compaction/compaction/src/invariant.ts`.
- **`shares-code-with`** — [0003. Event-sourced sessions with derived message history](0003-event-sourced-sessions-with-derived-message-history.md): Shares source implementation: `packages/compaction/compaction/src/index.ts`, `packages/compaction/compaction/src/invariant.ts`.
- **`shares-code-with`** — [0466. Remove synthetic turns for log-only events](0466-remove-synthetic-turns-for-log-only-events.md): Shares source implementation: `packages/core/session/src/chunk-rows.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0085. the end-seed log boundary](0085-the-end-seed-log-boundary.md): Shares source implementation: `packages/core/session/src/chunk-rows.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0304. The summarization call replays the conversation prefix for KV-cache reuse](0304-the-summarization-call-replays-the-conversation-prefix-for-kv-cache-reus.md): Shares source implementation: `packages/compaction/compaction-basic/src/index.ts`, `packages/compaction/compaction-basic/src/types.ts`.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0518-recallable-compaction-index-checkpoints-a-state-checkpoint-and-in-sessio.md`.
