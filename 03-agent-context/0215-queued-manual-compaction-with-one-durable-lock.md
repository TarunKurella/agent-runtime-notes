---
id: "dsh-note-0215"
title: "Queued manual compaction with one durable lock"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-30-queued-manual-compaction.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "changed"
  - "inject"
  - "summary"
  - "ManualCompactionError"
  - "ManualCompactAgentContext"
  - "CompactionEngine"
  - "commands"
  - "MessageId"
  - "/compact"
  - "@deepseek-ai/dsh-command-compact"
  - "ctx.commands"
  - "compactNow"
  - "command/run"
  - "command/done"
search_regex: "(?i)(changed|inject|summary|ManualCompactionError|ManualCompactAgentContext|CompactionEngine|commands|MessageId)"
---

# 0215. Queued manual compaction with one durable lock — implementation context

## Open this when

Automatic compaction protects the context window, but an interactive user also needs a deterministic way to condense accumulated history before pressure policy fires. Sending /compact as prompt text would spend a model turn and let the conversation model reinterpret a direct control action. Implementing it inside one UI would duplicate command discovery, lifecycle logging, cancellation, and backend policy. The human command arrives between turns and must summarize asynchronously.

## Source decision

The command plugin tracks each real handler promise independently of the command executor's abort-aware wait. Its composite lifecycle effect unregisters /compact before asynchronously draining handlers that already started, so root teardown reaches quiescence only after backend close and flush work settles. The seam's ManualCompactAgentContext adds only runMaintenance() to the session and routing facts compaction already needs. Retention, balancing, summarization, marker ordering, replacement, and durability remain backend responsibilities.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-30-queued-manual-compaction.md](../02-notes/implemented/feature/2026-07-30-queued-manual-compaction.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-30-queued-manual-compaction.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-30-queued-manual-compaction.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/interaction/commands/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/index.ts) | package entry point | Core file in the package named by the note: `packages/interaction/commands`. | `named-package-member` |
| [`packages/interaction/commands/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/interaction/commands`. | `named-package-member` |
| [`packages/interaction/commands/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/interaction/commands`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/compaction/command-compact/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/command-compact/src/index.ts) | package entry point | Core file in the package named by the note: `packages/compaction/command-compact`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/compaction/compaction-basic/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/index.ts) | package entry point | Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-package-member` |
| [`packages/compaction/compaction-basic/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-package-member` |
| [`packages/compaction/command-compact/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/command-compact/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/compaction/command-compact`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/compaction/compaction-basic/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/compaction/compaction-basic`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/compaction/compaction-basic/src/summarizer.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/summarizer.ts) | runtime implementation | Core file in the package named by the note: `packages/compaction/compaction-basic`. Defines `summary`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/interaction/commands`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/compaction/command-compact`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/command-compact) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/compaction/compaction-basic`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `changed` | `let` | [`apps/cli/src/plugin.ts:64`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts#L64) | `let changed = false` |
| `inject` | `const` | [`packages/compaction/command-compact/src/index.ts:11`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/command-compact/src/index.ts#L11) | `export const inject = ['commands', 'compaction']` |
| `inject` | `const` | [`packages/compaction/command-compact/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/command-compact/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `inject` | `const` | [`packages/compaction/compaction-basic/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `summary` | `const` | [`packages/compaction/compaction-basic/src/summarizer.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/summarizer.ts#L169) | `const summary = summaryText(rawOutput)` |
| `ManualCompactionError` | `class` | [`packages/compaction/compaction/src/index.ts:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/index.ts#L41) | `export class ManualCompactionError extends Error {` |
| `ManualCompactAgentContext` | `interface` | [`packages/compaction/compaction/src/index.ts:70`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/index.ts#L70) | `export interface ManualCompactAgentContext extends CompactionAgentContext {` |
| `CompactionEngine` | `class` | [`packages/compaction/compaction/src/index.ts:96`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/index.ts#L96) | `export abstract class CompactionEngine extends Service {` |
| `commands` | `const` | [`packages/hooks/hooks-codex/src/config.ts:60`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/config.ts#L60) | `const commands: MatcherGroup['hooks'] = []` |
| `inject` | `const` | [`packages/interaction/commands/src/invariant.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/invariant.ts#L16) | `export const inject = ['invariants']` |
| `MessageId` | `type` | [`packages/llm/llm/src/brand.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts#L16) | `export type MessageId = Branded<'MessageId'>` |

### Tests and executable evidence

- [`packages/compaction/compaction/tests/compaction.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/tests/compaction.spec.ts) — A test under the owning area exercises or imports `CompactionEngine`. A test under the owning area exercises or imports `ManualCompactAgentContext`.
- [`packages/compaction/command-compact/tests/command-compact.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/command-compact/tests/command-compact.spec.ts) — A test under the owning area exercises or imports `CompactionEngine`. A test under the owning area exercises or imports `compactNow`.
- [`packages/compaction/compaction-basic/tests/compaction-basic.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/compaction-basic.spec.ts) — A test under the owning area exercises or imports `dsh-compaction-basic`.
- [`packages/compaction/command-compact/tests/loader-composition.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/command-compact/tests/loader-composition.spec.ts) — A test under the owning area exercises or imports `CompactionEngine`. A test under the owning area exercises or imports `compactNow`.
- [`packages/compaction/compaction-basic/tests/manual-compaction.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/manual-compaction.spec.ts) — A test under the owning area exercises or imports `compactNow`. A test under the owning area exercises or imports `ManualCompactionError`.
- [`packages/compaction/compaction-basic/tests/loader-composition.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/loader-composition.spec.ts) — A test under the owning area exercises or imports `dsh-compaction-basic`.
- [`packages/compaction/compaction-basic/tests/compaction-loop-repro.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/compaction-loop-repro.spec.ts) — A test under the owning area exercises or imports `whenIdle`. A test under the owning area exercises or imports `dsh-compaction-basic`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — Contains the exact code literal `session/end-seed` named by the note.
- Source verification intent: Agent-loop tests cover same-tick right of way, preserved IDs and FIFO lifecycle, waking and quiet queued work, idempotent release, whenIdle(), cancellation, and teardown. Compact tests cover standalone and numbered invariant ownership, end-seed replay, live versus stale orphans, re-entrant listeners, selected-span drift, commit and close failures, flush ordering, exact cancellation causes, raw output and usage preservation, and automatic/manual mutual exclusion.

## How to read the implementation

1. Start with [`packages/interaction/commands/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `changed`, `inject`, `summary`, `ManualCompactionError`, `ManualCompactAgentContext`, `CompactionEngine`, `commands`, `MessageId`, `/compact`, `@deepseek-ai/dsh-command-compact`, `ctx.commands`, `compactNow`, `command/run`, `command/done`
- Regex: `(?i)(changed|inject|summary|ManualCompactionError|ManualCompactAgentContext|CompactionEngine|commands|MessageId)`

```bash
rg -n --pcre2 "(?i)(changed|inject|summary|ManualCompactionError|ManualCompactAgentContext|CompactionEngine|commands|MessageId)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0085. the end-seed log boundary](0085-the-end-seed-log-boundary.md): The source note links to this decision directly.
- **`source-link`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): The source note links to this decision directly.
- **`source-link`** — [0466. Remove synthetic turns for log-only events](0466-remove-synthetic-turns-for-log-only-events.md): The source note links to this decision directly.
- **`shares-code-with`** — [0033. After-call compaction pressure and context-overflow recovery](0033-after-call-compaction-pressure-and-context-overflow-recovery.md): Shares source implementation: `packages/compaction/compaction-basic/src/index.ts`, `packages/compaction/compaction-basic/src/invariant.ts`.
- **`shares-code-with`** — [0551. Effect-owned TUI interactive extensions](0551-effect-owned-tui-interactive-extensions.md): Shares source implementation: `packages/interaction/commands`, `packages/interaction/commands/src/index.ts`.
- **`shares-code-with`** — [0163. Plugin-owned human command registration](0163-plugin-owned-human-command-registration.md): Shares source implementation: `packages/interaction/commands`, `packages/interaction/commands/src/index.ts`.
- **`shares-code-with`** — [0062. Web command business surfaces and assembly (ui-commands / ui-skill / ui-subagent)](0062-web-command-business-surfaces-and-assembly-ui-commands-ui-skill-ui-subag.md): Shares source implementation: `packages/interaction/commands`, `packages/interaction/commands/src/index.ts`.
- **`shares-code-with`** — [0475. Remove the TUI package](0475-remove-the-tui-package.md): Shares source implementation: `packages/interaction/commands`, `packages/interaction/commands/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0215-queued-manual-compaction-with-one-durable-lock.md`.
