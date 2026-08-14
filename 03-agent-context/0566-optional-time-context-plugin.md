---
id: "dsh-note-0566"
title: "Optional time-context plugin"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-14-time-context-plugin.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
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
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "timeZone"
  - "refreshIntervalMs"
  - "change"
  - "@deepseek-ai/dsh-time-context"
  - "packages/context/time-context/"
  - "context/"
  - "dsh-agent-spine-demo"
  - "turn/start"
  - "user/message"
  - "assistant/message"
  - "tool/result"
  - "context/message"
  - "steering/message"
  - "Intl.DateTimeFormat"
search_regex: "(?i)(timeZone|refreshIntervalMs|change|@deepseek\\-ai/dsh\\-time\\-context|packages/context/time\\-context/|context/|dsh\\-agent\\-spine\\-demo|turn/start)"
---

# 0566. Optional time-context plugin — implementation context

## Open this when

The dynamic system-prompt storage and refresh decision in this record is superseded by Durable per-step time context. The opt-in package, zoned formatting, and validation remain; the follow-up owns the current model-visible and durability contract. An agent request has no live clock unless a deployment puts one in prompt text or gives the model a query tool. Static text becomes stale, while a tool call adds overhead to ordinary reasoning about dates, deadlines, or idle time. Without elapsed time, the model cannot distinguish an immediate follow-up from one sent hours after the preceding message.

## Source decision

@deepseek-ai/dsh-time-context is an opt-in function plugin at packages/context/time-context/. The context/ product group holds bounded request-context enrichments that define neither a tool nor a service. dsh-agent-spine-demo and shipped examples do not load the package; deployments mount it explicitly when its token and disclosure costs are acceptable. The plugin registers the global context:time system-prompt section at order 10, after the deployment persona and before tool guidance.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-14-time-context-plugin.md](../02-notes/archived/feature/2026-07-14-time-context-plugin.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-14-time-context-plugin.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-14-time-context-plugin.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/context/time-context/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/context/time-context`. Core file in the package named by the note: `packages/context/time-context`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/context/time-context/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/context/time-context`. Core file in the package named by the note: `packages/context/time-context`. | `named-directory-member, named-package-member` |
| [`packages/examples/agent-spine-demo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/src/index.ts) | package entry point | Core file in the package named by the note: `packages/examples/agent-spine-demo`. | `named-package-member` |
| [`packages/context/time-context/src/request-zone.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/request-zone.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/context/time-context`. Core file in the package named by the note: `packages/context/time-context`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/examples/agent-spine-demo/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/examples/agent-spine-demo`. | `named-package-member` |
| [`packages/context/time-context/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/context/time-context`. Core file in the package named by the note: `packages/context/time-context`. | `named-directory-member, named-package-member` |
| [`packages/context/time-context/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/context/time-context`. Core file in the package named by the note: `packages/context/time-context`. | `named-directory-member, named-package-member` |
| [`packages/context/time-context`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context) | package or module directory | The source note names this implementation area directly. The note names this package or capability. | `named-directory, named-package` |
| [`packages/examples/agent-spine-demo`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `change`, a construct named by the note. | `symbol-definition` |
| [`packages/examples/agent-spine-demo/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/README.md) | package contract and examples | Core file in the package named by the note: `packages/examples/agent-spine-demo`. | `named-package-member` |
| [`packages/examples/agent-spine-demo/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/package.json) | composition and configuration | Core file in the package named by the note: `packages/examples/agent-spine-demo`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `timeZone` | `const` | [`packages/context/time-context/src/index.ts:146`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/index.ts#L146) | `const timeZone = config.timeZone` |
| `refreshIntervalMs` | `const` | [`packages/context/time-context/src/index.ts:147`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/index.ts#L147) | `const refreshIntervalMs = config.refreshIntervalMs` |
| `timeZone` | `const` | [`packages/context/time-context/src/request-zone.ts:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/request-zone.ts#L52) | `const timeZone = browserTimeZone(message)` |
| `change` | `const` | [`packages/goal/goal/src/fold.ts:315`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L315) | `const change = decodeGoalChange(event.data)` |

### Tests and executable evidence

- [`packages/context/time-context/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/tests/invariant.spec.ts) — A test under the owning area exercises or imports `timeZone`. A test under the owning area exercises or imports `DateTimeFormat`.
- [`packages/context/time-context/tests/time-context.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/tests/time-context.spec.ts) — A test under the owning area exercises or imports `refreshIntervalMs`. A test under the owning area exercises or imports `timeZone`.
- [`packages/context/time-context/tests/request-zone.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/tests/request-zone.spec.ts) — A test under the owning area exercises or imports `timeZone`.
- [`packages/examples/agent-spine-demo/tests/agent-core.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/tests/agent-core.spec.ts) — A test under the owning area exercises or imports `dsh-agent-spine-demo`.
- [`apps/web/tests/live-interactions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/live-interactions.e2e.ts) — Contains the exact code literal `request/header` named by the note.
- [`packages/core/agent-loop/tests/resume.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/resume.spec.ts) — Contains the exact code literal `steering/message` named by the note.
- [`packages/session/session-persistence/tests/coordinator-contract.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/tests/coordinator-contract.ts) — Contains the exact code literal `steering/message` named by the note.
- [`packages/session-query/tool-session-query/tests/tool-session-query.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/tests/tool-session-query.spec.ts) — Contains the exact code literal `context/message` named by the note.
- Source verification intent: Unit tests pin formatting, baselines, refresh policy, validation, per-agent state, disposal, and load-time system-zone capture. A real agent-loop test pins the transmitted prompt and full request/header snapshots. A keyless subprocess e2e boots a test-only cordis.yml through the real Loader and stdio app, omits timeZone under a controlled TZ, drives two turns, and verifies the persisted request headers externally. Default snapshot compositions omit the plugin, so their transcript fixtures contain no temporal block.

## How to read the implementation

1. Start with [`packages/context/time-context/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/archived`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `timeZone`, `refreshIntervalMs`, `change`, `@deepseek-ai/dsh-time-context`, `packages/context/time-context/`, `context/`, `dsh-agent-spine-demo`, `turn/start`, `user/message`, `assistant/message`, `tool/result`, `context/message`, `steering/message`, `Intl.DateTimeFormat`
- Regex: `(?i)(timeZone|refreshIntervalMs|change|@deepseek\-ai/dsh\-time\-context|packages/context/time\-context/|context/|dsh\-agent\-spine\-demo|turn/start)`

```bash
rg -n --pcre2 "(?i)(timeZone|refreshIntervalMs|change|@deepseek\\-ai/dsh\\-time\\-context|packages/context/time\\-context/|context/|dsh\\-agent\\-spine\\-demo|turn/start)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0156. Durable per-step time context](0156-durable-per-step-time-context.md): Shares source implementation: `packages/context/time-context`, `packages/context/time-context/README.md`.
- **`shares-code-with`** — [0363. Bounded background job admission](0363-bounded-background-job-admission.md): Shares source implementation: `packages/examples/agent-spine-demo`, `packages/examples/agent-spine-demo/src/index.ts`.
- **`shares-code-with`** — [0669. TUI titles come from the session-title service](0669-tui-titles-come-from-the-session-title-service.md): Shares source implementation: `packages/examples/agent-spine-demo`, `packages/examples/agent-spine-demo/src/index.ts`.
- **`shares-code-with`** — [0193. tmux-location context](0193-tmux-location-context.md): Shares source implementation: `packages/examples/agent-spine-demo/src/index.ts`, `packages/examples/agent-spine-demo/src/invariant.ts`.
- **`shares-code-with`** — [0257. Durable Session-local reminders](0257-durable-session-local-reminders.md): Shares source implementation: `packages/context/time-context/src/index.ts`, `packages/context/time-context/src/invariant.ts`.
- **`shares-code-with`** — [0210. Persistent Bash and string-replacement editor tools](0210-persistent-bash-and-string-replacement-editor-tools.md): Shares source implementation: `packages/examples/agent-spine-demo/src/index.ts`, `packages/examples/agent-spine-demo/src/invariant.ts`.
- **`shares-code-with`** — [0200. Continuable subagents](0200-continuable-subagents.md): Shares source implementation: `packages/goal/goal/src/fold.ts`.
- **`shares-code-with`** — [0482. Explicit Schedule time-zone boundary](0482-explicit-schedule-time-zone-boundary.md): Shares source implementation: `packages/context/time-context/src/index.ts`, `packages/context/time-context/src/request-zone.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0566-optional-time-context-plugin.md`.
