---
id: "dsh-note-0305"
title: "Semantic session checkpoints"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-21-semantic-session-checkpoints.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/performance"
  - "concern/recovery"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "TOOL_NOT_STARTED"
  - "TOOL_OUTCOME_UNKNOWN"
  - "callId"
  - "session/event"
  - "dsh-session-checkpoint-policy"
  - "agent/pre-step"
  - "llm/stream"
  - "request/header"
  - "tools/execute"
  - "tool/call"
  - "turn/end"
  - "session/flush"
  - "ABORTED_BEFORE_DISPATCH"
  - "step/end"
search_regex: "(?i)(TOOL_NOT_STARTED|TOOL_OUTCOME_UNKNOWN|callId|session/event|dsh\\-session\\-checkpoint\\-policy|agent/pre\\-step|llm/stream|request/header)"
---

# 0305. Semantic session checkpoints — implementation context

## Open this when

Persistence buffered every synchronous session/event until the loop's final turn checkpoint. A turn is the correct conversational transaction, but it is too coarse as the only crash-recovery point: a hard crash during a long model request or tool call could discard the whole in-flight turn, including the request envelope needed to identify what had been attempted. A tool call with no result was also repaired with one undifferentiated interruption error, so the resumed model could not tell whether execution had started and could retry a side effect blindly.

## Source decision

dsh-session-checkpoint-policy owns semantic durability barriers as a zero-config plugin beside a persistence backend. At agent/pre-step, it flushes pending prompt input or the preceding response/result batch before the next request is derived. It wraps llm/stream lazily and flushes the live session after request/header is logged but before the adapter stream is constructed. It wraps top-level tools/execute after ordered pre-execute policy and flushes the recorded tool/call before the tool body; nested dispatches reuse the outer model-visible call.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-21-semantic-session-checkpoints.md](../02-notes/implemented/bug-fix/2026-07-21-semantic-session-checkpoints.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-21-semantic-session-checkpoints.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-21-semantic-session-checkpoints.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/core/agent-loop/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/session/session-checkpoint-policy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-checkpoint-policy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session/session-checkpoint-policy`. | `named-package-member` |
| [`packages/session/session-checkpoint-policy/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-checkpoint-policy/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-checkpoint-policy`. | `named-package-member` |
| [`packages/core/agent-loop`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session/session-checkpoint-policy`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-checkpoint-policy) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `callId`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/repair.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts) | runtime implementation | Defines `TOOL_NOT_STARTED`, a construct named by the note. Defines `TOOL_OUTCOME_UNKNOWN`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent-loop/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/core/agent-loop/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/session/session-checkpoint-policy/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-checkpoint-policy/README.md) | package contract and examples | Core file in the package named by the note: `packages/session/session-checkpoint-policy`. | `named-package-member` |
| [`packages/session/session-checkpoint-policy/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-checkpoint-policy/package.json) | composition and configuration | Core file in the package named by the note: `packages/session/session-checkpoint-policy`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `TOOL_NOT_STARTED` | `const` | [`packages/core/session/src/repair.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts#L13) | `export const TOOL_NOT_STARTED = 'TOOL_NOT_STARTED'` |
| `TOOL_OUTCOME_UNKNOWN` | `const` | [`packages/core/session/src/repair.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts#L16) | `export const TOOL_OUTCOME_UNKNOWN = 'TOOL_OUTCOME_UNKNOWN'` |
| `callId` | `const` | [`packages/core/tools/src/index.ts:1367`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1367) | `const callId = exec.callId` |

### Tests and executable evidence

- [`packages/core/session/tests/repair.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/repair.spec.ts) — A test under the owning area exercises or imports `TOOL_NOT_STARTED`. A test under the owning area exercises or imports `TOOL_OUTCOME_UNKNOWN`.
- [`packages/core/session/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/invariant.spec.ts) — A test under the owning area exercises or imports `TOOL_NOT_STARTED`.
- [`packages/session/session-checkpoint-policy/tests/crash-recovery.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-checkpoint-policy/tests/crash-recovery.e2e.ts) — A test under the owning area exercises or imports `TOOL_OUTCOME_UNKNOWN`.
- [`apps/web/tests/live-interactions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/live-interactions.e2e.ts) — Contains the exact code literal `request/header` named by the note.
- [`apps/cli/tests/fixtures/dsh-badge/snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/dsh-badge/snapshot.ts) — Contains the exact code literal `agent/pre-step` named by the note.

## How to read the implementation

1. Start with [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/performance`, `concern/recovery`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `TOOL_NOT_STARTED`, `TOOL_OUTCOME_UNKNOWN`, `callId`, `session/event`, `dsh-session-checkpoint-policy`, `agent/pre-step`, `llm/stream`, `request/header`, `tools/execute`, `tool/call`, `turn/end`, `session/flush`, `ABORTED_BEFORE_DISPATCH`, `step/end`
- Regex: `(?i)(TOOL_NOT_STARTED|TOOL_OUTCOME_UNKNOWN|callId|session/event|dsh\-session\-checkpoint\-policy|agent/pre\-step|llm/stream|request/header)`

```bash
rg -n --pcre2 "(?i)(TOOL_NOT_STARTED|TOOL_OUTCOME_UNKNOWN|callId|session/event|dsh\\-session\\-checkpoint\\-policy|agent/pre\\-step|llm/stream|request/header)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0163. Plugin-owned human command registration](0163-plugin-owned-human-command-registration.md): Shares source implementation: `packages/core/agent-loop`, `packages/core/agent-loop/src/index.ts`.
- **`shares-code-with`** — [0457. Project injected content verbatim, dropping the XML envelopes](0457-project-injected-content-verbatim-dropping-the-xml-envelopes.md): Shares source implementation: `packages/core/agent-loop`, `packages/core/agent-loop/src/index.ts`.
- **`shares-code-with`** — [0159. Fresh-agent Ralph workflow tool](0159-fresh-agent-ralph-workflow-tool.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0562. The session prefix --- request-only messages in front of the derived history](0562-the-session-prefix-request-only-messages-in-front-of-the-derived-history.md): Shares source implementation: `packages/core/agent-loop`, `packages/core/agent-loop/src/index.ts`.
- **`shares-code-with`** — [0596. `dsh migrate`/`dsh upgrade` seed the first turn with a skill](0596-dsh-migrate-dsh-upgrade-seed-the-first-turn-with-a-skill.md): Shares source implementation: `packages/core/agent-loop`, `packages/core/agent-loop/src/index.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/session/src/repair.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0335. Goal-round wrap-up message](0335-goal-round-wrap-up-message.md): Shares source implementation: `packages/core/agent-loop`, `packages/core/agent-loop/src/index.ts`.
- **`shares-code-with`** — [0151. Parallel tool-call execution by per-call safety](0151-parallel-tool-call-execution-by-per-call-safety.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/tools/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0305-semantic-session-checkpoints.md`.
