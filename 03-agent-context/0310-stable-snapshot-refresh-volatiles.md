---
id: "dsh-note-0310"
title: "Stable snapshot refresh volatiles"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-27-stable-snapshot-refresh-volatiles.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/trust"
  - "domain/build-release"
  - "domain/context"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/streaming"
aliases:
  - "normalizeSessionLog"
  - "agent/inbox/spliced"
  - "Stable snapshot refresh volatiles"
  - "bug fix"
  - "boundary"
  - "compatibility"
  - "evidence"
  - "human control"
  - "lifecycle"
  - "ownership"
  - "trust"
  - "build release"
  - "context"
  - "protocols"
search_regex: "(?i)(normalizeSessionLog|agent/inbox/spliced|Stable[- ]snapshot[- ]refresh[- ]volatiles|bug[- ]fix|boundary|compatibility|evidence|human[- ]control)"
---

# 0310. Stable snapshot refresh volatiles — implementation context

## Open this when

ACP snapshot comparison normalizes generated UUIDs, cwd aliases, spill locators, embedded event times, and omitted-byte counts, but refresh write-back persisted the fresh raw values. A behaviorally unchanged refresh therefore rewrote fixtures with new randomness or host-specific path spellings even though the comparison contract considered both logs equal. Message identity needs a weaker structural precondition than aligned records: an unrelated log event can break record alignment while an inherited message's identity-free value remains unchanged across parent and child logs.

## Source decision

Before record or refresh writes session fixtures, the shared snapshot support passes fixture-ready logs to one structural message-ID owner. It recognizes surface carriers through the session package's authoritative surface-type predicate and the correlated queued copies in agent/inbox/spliced, fingerprints every complete message with its top-level id removed, and records every ID-to-fingerprint edge across all parent/child logs. It reuses an existing UUID only when both its ID and fingerprint have degree one in the fresh and existing graphs, then rewrites only validated message id fields in those carriers.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-27-stable-snapshot-refresh-volatiles.md](../02-notes/implemented/bug-fix/2026-07-27-stable-snapshot-refresh-volatiles.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-27-stable-snapshot-refresh-volatiles.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-27-stable-snapshot-refresh-volatiles.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/test-support/acp-snapshot/src/normalize.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/normalize.ts) | runtime implementation | Defines `normalizeSessionLog`, a construct named by the note. | `symbol-definition` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `agent/inbox/spliced` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Contains the exact code literal `agent/inbox/spliced` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.zh.md) | package contract and examples | Contains the exact code literal `agent/inbox/spliced` named by the note. | `exact-code-occurrence` |
| [`docs/persistence-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/persistence-catalog.md) | package contract and examples | Contains the exact code literal `agent/inbox/spliced` named by the note. | `exact-code-occurrence` |
| [`docs/persistence-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/persistence-catalog.zh.md) | package contract and examples | Contains the exact code literal `agent/inbox/spliced` named by the note. | `exact-code-occurrence` |
| [`scripts/snapshots/python-sdk-single-exe/advanced/result.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/snapshots/python-sdk-single-exe/advanced/result.json) | repository automation | Contains the exact code literal `agent/inbox/spliced` named by the note. | `exact-code-occurrence` |
| [`scripts/snapshots/python-sdk-single-exe/advanced/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/snapshots/python-sdk-single-exe/advanced/session.jsonl) | repository automation | Contains the exact code literal `agent/inbox/spliced` named by the note. | `exact-code-occurrence` |
| [`scripts/snapshots/python-sdk-single-exe/advanced/session.1.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/snapshots/python-sdk-single-exe/advanced/session.1.jsonl) | repository automation | Contains the exact code literal `agent/inbox/spliced` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `normalizeSessionLog` | `function` | [`packages/test-support/acp-snapshot/src/normalize.ts:297`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/normalize.ts#L297) | `export function normalizeSessionLog(` |

### Tests and executable evidence

- [`packages/test-support/acp-snapshot/tests/normalize.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/tests/normalize.spec.ts) — A test under the owning area exercises or imports `normalizeSessionLog`.

## How to read the implementation

1. Start with [`packages/test-support/acp-snapshot/src/normalize.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/normalize.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/trust`, `domain/build-release`, `domain/context`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/streaming`
- Aliases: `normalizeSessionLog`, `agent/inbox/spliced`, `Stable snapshot refresh volatiles`, `bug fix`, `boundary`, `compatibility`, `evidence`, `human control`, `lifecycle`, `ownership`, `trust`, `build release`, `context`, `protocols`
- Regex: `(?i)(normalizeSessionLog|agent/inbox/spliced|Stable[- ]snapshot[- ]refresh[- ]volatiles|bug[- ]fix|boundary|compatibility|evidence|human[- ]control)`

```bash
rg -n --pcre2 "(?i)(normalizeSessionLog|agent/inbox/spliced|Stable[- ]snapshot[- ]refresh[- ]volatiles|bug[- ]fix|boundary|compatibility|evidence|human[- ]control)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0601. Live standalone compaction progress in the terminal](0601-live-standalone-compaction-progress-in-the-terminal.md): Shares source implementation: `docs/agent-lifecycle.md`, `docs/persistence-catalog.md`.
- **`shares-code-with`** — [0358. The minimal preset owns the complete RL agent composition](0358-the-minimal-preset-owns-the-complete-rl-agent-composition.md): Shares source implementation: `docs/agent-lifecycle.md`, `scripts/gen-doc-graphs.ts`.
- **`shares-code-with`** — [0328. Compaction checkpoints use an English engineering register](0328-compaction-checkpoints-use-an-english-engineering-register.md): Shares source implementation: `docs/agent-lifecycle.md`, `docs/persistence-catalog.md`.
- **`shares-code-with`** — [0677. Use `session.jsonl` as the only snapshot session-log artifact](0677-use-session-jsonl-as-the-only-snapshot-session-log-artifact.md): Shares source implementation: `packages/test-support/acp-snapshot/src/normalize.ts`, `scripts/gen-doc-graphs.ts`.
- **`shares-code-with`** — [0681. Pin request-header content in one snapshot scenario](0681-pin-request-header-content-in-one-snapshot-scenario.md): Shares source implementation: `packages/test-support/acp-snapshot/src/normalize.ts`, `scripts/gen-doc-graphs.ts`.
- **`shares-code-with`** — [0633. Documentation graph index for maintainers and SDK users](0633-documentation-graph-index-for-maintainers-and-sdk-users.md): Shares source implementation: `docs/agent-lifecycle.md`, `scripts/gen-doc-graphs.ts`.
- **`shares-code-with`** — [0179. Web todo display --- snapshot side-effect channel + two render surfaces](0179-web-todo-display-snapshot-side-effect-channel-two-render-surfaces.md): Shares source implementation: `scripts/gen-doc-graphs.ts`.
- **`shares-code-with`** — [0682. Extract the ACP snapshot suite into a support package](0682-extract-the-acp-snapshot-suite-into-a-support-package.md): Shares source implementation: `packages/test-support/acp-snapshot/src/normalize.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0310-stable-snapshot-refresh-volatiles.md`.
