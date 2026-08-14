---
id: "dsh-note-0476"
title: "Buffer-free feedback telemetry"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-08-06-buffer-free-feedback-telemetry.md"
implementation_evidence: "medium"
target_anchor: "append-only event and session store"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/performance"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "shutdown"
  - "live"
  - "on-demand"
  - "captureSession"
  - "session-telemetry/record"
  - "FEEDBACK_ONLY"
  - "feedback/record"
  - "session/event"
  - "agent-error"
  - "Buffer-free feedback telemetry"
  - "simplification"
  - "boundary"
  - "concurrency"
  - "evidence"
search_regex: "(?i)(shutdown|live|on\\-demand|captureSession|session\\-telemetry/record|FEEDBACK_ONLY|feedback/record|session/event)"
---

# 0476. Buffer-free feedback telemetry — implementation context

## Open this when

Feedback-only telemetry must upload the session-log prefix only after recorded feedback. Retaining a deep-copied, redacted record for every projected event until that trigger duplicates the canonical session log and grows without a bound for a long-lived session that never records feedback.

## Source decision

The telemetry coordinator provides live and on-demand capture. On-demand capture registers no session, flush, or operational-event listeners and retains no projected records. captureSession(session, throughSeq?) reads the canonical session log after the handoff cursor through an optional inclusive sequence boundary, applies the fixed projection, deep-copies each accepted event, runs the current session-telemetry/record waterfall, and hands the result to the backend. FEEDBACK_ONLY invokes that method with the feedback/record event's sequence.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-08-06-buffer-free-feedback-telemetry.md](../02-notes/implemented/simplification/2026-08-06-buffer-free-feedback-telemetry.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-08-06-buffer-free-feedback-telemetry.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-08-06-buffer-free-feedback-telemetry.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/cli/src/profile-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts) | runtime implementation | Defines `shutdown`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Defines `live`, a construct named by the note. | `symbol-definition` |
| [`packages/lsp/lsp-stdio/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/index.ts) | package entry point | Defines `live`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `live`, a construct named by the note. | `symbol-definition` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `session/event` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `session/event` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `session/event` named by the note. | `exact-code-occurrence` |
| [`docs/subsystems/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/README.md) | package contract and examples | Contains the exact code literal `session-telemetry/record` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Contains the exact code literal `session/event` named by the note. | `exact-code-occurrence` |
| [`docs/persistence-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/persistence-catalog.md) | package contract and examples | Contains the exact code literal `feedback/record` named by the note. Contains the exact code literal `session/event` named by the note. | `exact-code-occurrence` |
| [`docs/subsystems/feedback.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/feedback.md) | package contract and examples | Contains the exact code literal `feedback/record` named by the note. | `exact-code-occurrence` |
| [`packages/feedback/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/README.md) | package contract and examples | Contains the exact code literal `feedback/record` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `shutdown` | `const` | [`apps/cli/src/profile-boot.ts:210`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L210) | `const shutdown = createProcessShutdown(async () => { await app.current?.fiber.dispose() })` |
| `live` | `const` | [`packages/core/session/src/index.ts:1147`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L1147) | `const live = this.get(source.id)` |
| `live` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1600`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1600) | `const live = ctx.get('agents')?.get(sessionId)` |
| `live` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1628`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1628) | `const live = ctx.agents.get(sessionId)` |
| `live` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1682`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1682) | `const live = ctx.agents.get(sessionId)` |
| `live` | `const` | [`packages/lsp/lsp-stdio/src/index.ts:356`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/index.ts#L356) | `const live = [...this.instances.values()]` |

### Tests and executable evidence

- [`apps/cli/tests/process-shutdown.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/process-shutdown.spec.ts) — A test under the owning area exercises or imports `shutdown`.
- [`apps/cli/tests/headless-shutdown.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/headless-shutdown.e2e.ts) — A test under the owning area exercises or imports `shutdown`.
- [`apps/cli/tests/fixtures/never-dispose.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/never-dispose.mjs) — A test under the owning area exercises or imports `shutdown`.
- [`packages/host/apiproxy/tests/rpc-schemas.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/rpc-schemas.spec.ts) — A test under the owning area exercises or imports `agent-error`.
- [`packages/client/runtime/tests/manager.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/manager.client.spec.ts) — A test under the owning area exercises or imports `agent-error`.
- [`packages/host/apiproxy/tests/api-proxy-approval.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-approval.spec.ts) — A test under the owning area exercises or imports `on-demand`.
- [`packages/session/session-telemetry/tests/telemetry.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry/tests/telemetry.spec.ts) — A test under the owning area exercises or imports `on-demand`. A test under the owning area exercises or imports `captureSession`.
- [`packages/session/session-telemetry-otel/tests/otel.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/tests/otel.spec.ts) — A test under the owning area exercises or imports `FEEDBACK_ONLY`.

## How to read the implementation

1. Start with [`apps/cli/src/profile-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** append-only event and session store.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/performance`, `concern/recovery`, `domain/build-release`, `domain/observability`, `domain/session-state`, `domain/storage`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`
- Aliases: `shutdown`, `live`, `on-demand`, `captureSession`, `session-telemetry/record`, `FEEDBACK_ONLY`, `feedback/record`, `session/event`, `agent-error`, `Buffer-free feedback telemetry`, `simplification`, `boundary`, `concurrency`, `evidence`
- Regex: `(?i)(shutdown|live|on\-demand|captureSession|session\-telemetry/record|FEEDBACK_ONLY|feedback/record|session/event)`

```bash
rg -n --pcre2 "(?i)(shutdown|live|on\\-demand|captureSession|session\\-telemetry/record|FEEDBACK_ONLY|feedback/record|session/event)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0258. Feedback-gated session telemetry](0258-feedback-gated-session-telemetry.md): The source note links to this decision directly.
- **`shares-code-with`** — [0338. Bounded, escalating signal shutdown for Web and headless](0338-bounded-escalating-signal-shutdown-for-web-and-headless.md): Shares source implementation: `apps/cli/src/profile-boot.ts`, `apps/cli/tests/headless-shutdown.e2e.ts`.
- **`shares-code-with`** — [0085. the end-seed log boundary](0085-the-end-seed-log-boundary.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0324. Multi-select custom answer composition](0324-multi-select-custom-answer-composition.md): Shares source implementation: `packages/client/runtime/tests/manager.client.spec.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0329. fail-loud releases the terminal before exiting](0329-fail-loud-releases-the-terminal-before-exiting.md): Shares source implementation: `apps/cli/src/profile-boot.ts`, `packages/lsp/lsp-stdio/src/index.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0518. Recallable compaction --- index checkpoints, a state checkpoint, and in-session history recall](0518-recallable-compaction-index-checkpoints-a-state-checkpoint-and-in-sessio.md): Shares source implementation: `packages/core/session/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0476-buffer-free-feedback-telemetry.md`.
