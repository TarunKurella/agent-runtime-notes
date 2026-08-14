---
id: "dsh-note-0481"
title: "Conversational Schedule delivery"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-08-09-conversational-schedule-delivery.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
aliases:
  - "flush"
  - "schedule/change"
  - "@deepseek-ai/dsh-schedule"
  - "Conversational Schedule delivery"
  - "simplification"
  - "boundary"
  - "evidence"
  - "human control"
  - "lifecycle"
  - "ownership"
  - "recovery"
  - "build release"
  - "jobs tasks"
  - "llm"
search_regex: "(?i)(flush|schedule/change|@deepseek\\-ai/dsh\\-schedule|Conversational[- ]Schedule[- ]delivery|simplification|boundary|evidence|human[- ]control)"
---

# 0481. Conversational Schedule delivery — implementation context

## Open this when

Schedule already delivers a due reminder by queuing a normal Agent follow-up. A second durable Web receipt represented the same occurrence through a Schedule projection, a persistence-success event, Host history and live sidecars, client same-sequence upgrades, a generic event-view slot, and a dedicated renderer. That path spread one feature's confirmation UI across Session, persistence, Host, client runtime, conversation UI, and an extra package. The receipt also created a second meaning of delivery.

## Source decision

A due reminder waits for the Agent's idle maintenance phase and calls followup(). The follow-up starts a normal later turn and appears through the ordinary conversation transcript; Schedule never calls steer() and never interrupts the current turn. schedule/change remains the only durable Schedule state. Its dispatch operation records that the follow-up was synchronously queued, which prevents ordinary restart replay after the dispatch is durable. Dispatch does not claim model success, user acknowledgement, or an external notification.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-08-09-conversational-schedule-delivery.md](../02-notes/implemented/simplification/2026-08-09-conversational-schedule-delivery.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-08-09-conversational-schedule-delivery.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-08-09-conversational-schedule-delivery.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/schedule/schedule/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/index.ts) | package entry point | Core file in the package named by the note: `packages/schedule/schedule`. | `named-package-member` |
| [`packages/schedule/schedule/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/schedule/schedule`. | `named-package-member` |
| [`packages/schedule/schedule/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/schedule/schedule`. | `named-package-member` |
| [`packages/schedule/schedule`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/typert/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts) | package entry point | Defines `flush`, a construct named by the note. | `symbol-definition` |
| [`packages/schedule/schedule/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/README.md) | package contract and examples | Core file in the package named by the note: `packages/schedule/schedule`. | `named-package-member` |
| [`packages/schedule/schedule/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/package.json) | composition and configuration | Core file in the package named by the note: `packages/schedule/schedule`. | `named-package-member` |
| [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) | package contract and examples | Contains the exact code literal `schedule/change` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.zh.md) | package contract and examples | Contains the exact code literal `schedule/change` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-tool-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-tool-catalog.ts) | repository automation | Contains the exact code literal `schedule/change` named by the note. | `exact-code-occurrence` |
| [`docs/persistence-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/persistence-catalog.md) | package contract and examples | Contains the exact code literal `schedule/change` named by the note. | `exact-code-occurrence` |
| [`docs/subsystems/schedule.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/schedule.md) | package contract and examples | Contains the exact code literal `schedule/change` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `flush` | `const` | [`packages/typert/loader/src/index.ts:392`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts#L392) | `const flush = (onError: (error: Error) => void): Promise<void>[] => {` |

### Tests and executable evidence

- [`apps/web/tests/schedule-after.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/schedule-after.e2e.ts) — Contains the exact code literal `schedule/change` named by the note.
- Source verification intent: Package lifecycle tests pin idle waiting, maintenance ownership, follow-up-before-dispatch ordering, synchronous enqueue failure, model-independent dispatch, and restart replay. The assembled Web scenario snapshots the resulting assistant row and asserts that a persisted Schedule dispatch has no special history view. Source and dependency audits reject the removed presentation symbols, event, sidecar, slot, renderer package, and overlay entry.

## How to read the implementation

1. Start with [`packages/schedule/schedule/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/jobs-tasks`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/projection`
- Aliases: `flush`, `schedule/change`, `@deepseek-ai/dsh-schedule`, `Conversational Schedule delivery`, `simplification`, `boundary`, `evidence`, `human control`, `lifecycle`, `ownership`, `recovery`, `build release`, `jobs tasks`, `llm`
- Regex: `(?i)(flush|schedule/change|@deepseek\-ai/dsh\-schedule|Conversational[- ]Schedule[- ]delivery|simplification|boundary|evidence|human[- ]control)`

```bash
rg -n --pcre2 "(?i)(flush|schedule/change|@deepseek\\-ai/dsh\\-schedule|Conversational[- ]Schedule[- ]delivery|simplification|boundary|evidence|human[- ]control)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0496. Real-API e2e in CI against the external DeepSeek API](0496-real-api-e2e-in-ci-against-the-external-deepseek-api.md): Shares source implementation: `packages/schedule/schedule`, `packages/schedule/schedule/src/index.ts`.
- **`shares-code-with`** — [0257. Durable Session-local reminders](0257-durable-session-local-reminders.md): Shares source implementation: `packages/schedule/schedule/src/index.ts`, `packages/schedule/schedule/src/invariant.ts`.
- **`shares-code-with`** — [0537. Truncate interrupted final turns on load](0537-truncate-interrupted-final-turns-on-load.md): Shares source implementation: `docs/tool-catalog.md`, `docs/tool-catalog.zh.md`.
- **`shares-code-with`** — [0315. Atomic Web image admission](0315-atomic-web-image-admission.md): Shares source implementation: `docs/tool-catalog.md`, `docs/tool-catalog.zh.md`.
- **`shares-code-with`** — [0621. TUI step timing trails the step's last message](0621-tui-step-timing-trails-the-step-s-last-message.md): Shares source implementation: `docs/tool-catalog.md`, `docs/tool-catalog.zh.md`.
- **`shares-code-with`** — [0119. Lifecycle-bound message feedback sidecar](0119-lifecycle-bound-message-feedback-sidecar.md): Shares source implementation: `packages/typert/loader/src/index.ts`.
- **`same-design-pressure`** — [0113. Client Conversation business-node assembly and keyed Chat snapshots](0113-client-conversation-business-node-assembly-and-keyed-chat-snapshots.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/human-control`.
- **`shares-code-with`** — [0587. TUI prompt themes compose mutable plugin values](0587-tui-prompt-themes-compose-mutable-plugin-values.md): Shares source implementation: `packages/typert/loader/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0481-conversational-schedule-delivery.md`.
