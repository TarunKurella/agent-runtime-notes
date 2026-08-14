---
id: "dsh-note-0328"
title: "Compaction checkpoints use an English engineering register"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-31-english-compaction-checkpoints.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/performance"
  - "domain/build-release"
  - "domain/context"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "COMPACTION_INSTRUCTION"
  - "assistant/chunk"
  - "Compaction checkpoints use an English engineering register"
  - "bug fix"
  - "boundary"
  - "discovery routing"
  - "evidence"
  - "performance"
  - "build release"
  - "context"
  - "llm"
  - "session state"
  - "storage"
  - "ui interaction"
search_regex: "(?i)(COMPACTION_INSTRUCTION|assistant/chunk|Compaction[- ]checkpoints[- ]use[- ]an[- ]English[- ]engineering[- ]register|bug[- ]fix|boundary|discovery[- ]routing|evidence|performance)"
---

# 0328. Compaction checkpoints use an English engineering register — implementation context

## Open this when

A compaction checkpoint becomes part of the next model request's durable prefix. When a multilingual conversation leads the compactor to preserve its narrative material in the conversation language, the checkpoint can introduce a large amount of a language that is absent from the code, tool output, and existing reasoning prefix. That language then persists across later compaction cycles and can influence the conversation model's reasoning register.

## Source decision

COMPACTION_INSTRUCTION requires an English-language internal engineering checkpoint. It asks the model to translate narrative source material as needed while preserving exact literals, including paths, commands, errors, identifiers, signatures, and quoted wording when exactness matters. The checkpoint's headings and terse engineering bullets remain the existing structured format. The requirement is integrated into the first sentence of the trailing compaction instruction.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-31-english-compaction-checkpoints.md](../02-notes/implemented/bug-fix/2026-07-31-english-compaction-checkpoints.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-31-english-compaction-checkpoints.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-31-english-compaction-checkpoints.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/compaction/compaction-basic/src/summarizer.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/summarizer.ts) | runtime implementation | Defines `COMPACTION_INSTRUCTION`, a construct named by the note. | `symbol-definition` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `assistant/chunk` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) | package contract and examples | Contains the exact code literal `assistant/chunk` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `assistant/chunk` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `assistant/chunk` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Contains the exact code literal `assistant/chunk` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.zh.md) | package contract and examples | Contains the exact code literal `assistant/chunk` named by the note. | `exact-code-occurrence` |
| [`docs/persistence-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/persistence-catalog.md) | package contract and examples | Contains the exact code literal `assistant/chunk` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `COMPACTION_INSTRUCTION` | `const` | [`packages/compaction/compaction-basic/src/summarizer.ts:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/summarizer.ts#L31) | `const COMPACTION_INSTRUCTION = [` |

### Tests and executable evidence

- [`scripts/session-fixture-layout.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/session-fixture-layout.spec.ts) — Contains the exact code literal `assistant/chunk` named by the note.

## How to read the implementation

1. Start with [`packages/compaction/compaction-basic/src/summarizer.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/summarizer.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/performance`, `domain/build-release`, `domain/context`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `COMPACTION_INSTRUCTION`, `assistant/chunk`, `Compaction checkpoints use an English engineering register`, `bug fix`, `boundary`, `discovery routing`, `evidence`, `performance`, `build release`, `context`, `llm`, `session state`, `storage`, `ui interaction`
- Regex: `(?i)(COMPACTION_INSTRUCTION|assistant/chunk|Compaction[- ]checkpoints[- ]use[- ]an[- ]English[- ]engineering[- ]register|bug[- ]fix|boundary|discovery[- ]routing|evidence|performance)`

```bash
rg -n --pcre2 "(?i)(COMPACTION_INSTRUCTION|assistant/chunk|Compaction[- ]checkpoints[- ]use[- ]an[- ]English[- ]engineering[- ]register|bug[- ]fix|boundary|discovery[- ]routing|evidence|performance)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0304. The summarization call replays the conversation prefix for KV-cache reuse](0304-the-summarization-call-replays-the-conversation-prefix-for-kv-cache-reus.md): The source note links to this decision directly.
- **`shares-code-with`** — [0601. Live standalone compaction progress in the terminal](0601-live-standalone-compaction-progress-in-the-terminal.md): Shares source implementation: `docs/agent-lifecycle.md`, `docs/architecture.md`.
- **`shares-code-with`** — [0310. Stable snapshot refresh volatiles](0310-stable-snapshot-refresh-volatiles.md): Shares source implementation: `docs/agent-lifecycle.md`, `docs/persistence-catalog.md`.
- **`shares-code-with`** — [0537. Truncate interrupted final turns on load](0537-truncate-interrupted-final-turns-on-load.md): Shares source implementation: `docs/architecture.md`, `docs/architecture.zh.md`.
- **`shares-code-with`** — [0358. The minimal preset owns the complete RL agent composition](0358-the-minimal-preset-owns-the-complete-rl-agent-composition.md): Shares source implementation: `docs/agent-lifecycle.md`, `scripts/gen-doc-graphs.ts`.
- **`shares-code-with`** — [0466. Remove synthetic turns for log-only events](0466-remove-synthetic-turns-for-log-only-events.md): Shares source implementation: `docs/architecture.md`, `docs/config-catalog.md`.
- **`shares-code-with`** — [0633. Documentation graph index for maintainers and SDK users](0633-documentation-graph-index-for-maintainers-and-sdk-users.md): Shares source implementation: `docs/agent-lifecycle.md`, `scripts/gen-doc-graphs.ts`.
- **`shares-code-with`** — [0215. Queued manual compaction with one durable lock](0215-queued-manual-compaction-with-one-durable-lock.md): Shares source implementation: `packages/compaction/compaction-basic/src/summarizer.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0328-compaction-checkpoints-use-an-english-engineering-register.md`.
