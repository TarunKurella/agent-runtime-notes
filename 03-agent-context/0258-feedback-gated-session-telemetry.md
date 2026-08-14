---
id: "dsh-note-0258"
title: "Feedback-gated session telemetry"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-05-feedback-gated-session-telemetry.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/performance"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
aliases:
  - "shutdown"
  - "live"
  - "SessionTelemetryMode"
  - "mode"
  - "url"
  - "emit"
  - "append"
  - "@deepseek-ai/dsh-session-telemetry-otel"
  - "FULL"
  - "FEEDBACK_ONLY"
  - "feedback/record"
  - "DISABLED"
  - "on-demand"
  - "captureSession"
search_regex: "(?i)(shutdown|live|SessionTelemetryMode|mode|emit|append|@deepseek\\-ai/dsh\\-session\\-telemetry\\-otel|FULL)"
---

# 0258. Feedback-gated session telemetry — implementation context

## Open this when

Session telemetry originally has one mounted behavior: every accepted record enters the reporting backend immediately. Deployments need two stricter policies without replacing the plugin: hold a session's telemetry unless its user records feedback, or disable reporting while still explaining what happens to feedback. The policy must preserve the telemetry seam's redaction-before-backend boundary.

## Source decision

@deepseek-ai/dsh-session-telemetry-otel exposes the string-valued SessionTelemetryMode enum to TypeScript callers and accepts the same three uppercase mode values in serialized configuration: FULL explicitly selects immediate delivery to the configured OTel pipeline. FEEDBACK_ONLY reads the canonical session log when feedback/record is appended and hands over the unreleased prefix through that exact event. Records appended after that boundary remain local until another feedback event.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-05-feedback-gated-session-telemetry.md](../02-notes/implemented/feature/2026-08-05-feedback-gated-session-telemetry.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-05-feedback-gated-session-telemetry.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-05-feedback-gated-session-telemetry.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/session/session-telemetry-otel/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session/session-telemetry-otel`. Defines `SessionTelemetryMode`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session/session-telemetry-otel/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-telemetry-otel`. | `named-package-member` |
| [`packages/session/session-telemetry-otel`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel) | package or module directory | The note names this package or capability. | `named-package` |
| [`apps/cli/src/profile-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts) | runtime implementation | Defines `shutdown`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Defines `live`, a construct named by the note. | `symbol-definition` |
| [`packages/workflow/tool-workflow/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/index.ts) | package entry point | Defines `append`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/llm-mock-server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-mock-server/src/index.ts) | package entry point | Defines `emit`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-telemetry-otel/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/README.md) | package contract and examples | Core file in the package named by the note: `packages/session/session-telemetry-otel`. | `named-package-member` |
| [`packages/session/session-telemetry-otel/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/package.json) | composition and configuration | Core file in the package named by the note: `packages/session/session-telemetry-otel`. | `named-package-member` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `session/event` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `session/event` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `session/event` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `shutdown` | `const` | [`apps/cli/src/profile-boot.ts:210`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L210) | `const shutdown = createProcessShutdown(async () => { await app.current?.fiber.dispose() })` |
| `live` | `const` | [`packages/core/session/src/index.ts:1147`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L1147) | `const live = this.get(source.id)` |
| `SessionTelemetryMode` | `enum` | [`packages/session/session-telemetry-otel/src/index.ts:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/index.ts#L44) | `export enum SessionTelemetryMode {` |
| `mode` | `const` | [`packages/session/session-telemetry-otel/src/index.ts:157`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/index.ts#L157) | `const mode = resolveMode(config.mode)` |
| `url` | `const` | [`packages/session/session-telemetry-otel/src/index.ts:170`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/index.ts#L170) | `const url = config.exporter?.url` |
| `emit` | `function` | [`packages/test-support/llm-mock-server/src/index.ts:291`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-mock-server/src/index.ts#L291) | `function emit(options: ResolvedOptions, event: MockLlmServerEvent): void {` |
| `append` | `const` | [`packages/workflow/tool-workflow/src/index.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-workflow/src/index.ts#L75) | `const append = <Type extends keyof ToolWorkflowRecordEventMap>(` |

### Tests and executable evidence

- [`apps/cli/tests/process-shutdown.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/process-shutdown.spec.ts) — A test under the owning area exercises or imports `shutdown`.
- [`apps/cli/tests/headless-shutdown.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/headless-shutdown.e2e.ts) — A test under the owning area exercises or imports `shutdown`.
- [`apps/cli/tests/fixtures/never-dispose.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/never-dispose.mjs) — A test under the owning area exercises or imports `shutdown`.
- [`packages/session/session-telemetry-otel/tests/otel.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/tests/otel.spec.ts) — A test under the owning area exercises or imports `SessionTelemetryMode`. A test under the owning area exercises or imports `FULL`.
- [`packages/session/session-telemetry-otel/tests/loader-composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/tests/loader-composition.e2e.ts) — A test under the owning area exercises or imports `FEEDBACK_ONLY`. A test under the owning area exercises or imports `DISABLED`.
- [`apps/web/tests/feedback-command.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/feedback-command.e2e.ts) — Contains the exact code literal `feedback/record` named by the note.

## How to read the implementation

1. Start with [`packages/session/session-telemetry-otel/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/performance`, `concern/recovery`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/session-state`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`
- Aliases: `shutdown`, `live`, `SessionTelemetryMode`, `mode`, `url`, `emit`, `append`, `@deepseek-ai/dsh-session-telemetry-otel`, `FULL`, `FEEDBACK_ONLY`, `feedback/record`, `DISABLED`, `on-demand`, `captureSession`
- Regex: `(?i)(shutdown|live|SessionTelemetryMode|mode|emit|append|@deepseek\-ai/dsh\-session\-telemetry\-otel|FULL)`

```bash
rg -n --pcre2 "(?i)(shutdown|live|SessionTelemetryMode|mode|emit|append|@deepseek\\-ai/dsh\\-session\\-telemetry\\-otel|FULL)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0286. SessionTelemetryBackend requires explicit opt-in](0286-sessiontelemetrybackend-requires-explicit-opt-in.md): The source note links to this decision directly.
- **`source-link`** — [0476. Buffer-free feedback telemetry](0476-buffer-free-feedback-telemetry.md): The source note links to this decision directly.
- **`shares-code-with`** — [0236. Default session-telemetry mount (OTel reporting) in the dsh web composition](0236-default-session-telemetry-mount-otel-reporting-in-the-dsh-web-compositio.md): Shares source implementation: `packages/session/session-telemetry-otel`, `packages/session/session-telemetry-otel/README.md`.
- **`shares-code-with`** — [0109. Shared anonymous user id across telemetry, feedback, and DeepSeek requests](0109-shared-anonymous-user-id-across-telemetry-feedback-and-deepseek-requests.md): Shares source implementation: `packages/session/session-telemetry-otel`, `packages/session/session-telemetry-otel/README.md`.
- **`shares-code-with`** — [0338. Bounded, escalating signal shutdown for Web and headless](0338-bounded-escalating-signal-shutdown-for-web-and-headless.md): Shares source implementation: `apps/cli/src/profile-boot.ts`, `apps/cli/tests/process-shutdown.spec.ts`.
- **`shares-code-with`** — [0233. SessionTelemetryBackend anonymous user id ($DSH_HOME/.anonymous-user-id) and the OTel Resource user.id](0233-sessiontelemetrybackend-anonymous-user-id-dsh-home-anonymous-user-id-and.md): Shares source implementation: `packages/session/session-telemetry-otel`, `packages/session/session-telemetry-otel/src/index.ts`.
- **`shares-code-with`** — [0176. Session telemetry seam with mandatory redaction and the OTel backend](0176-session-telemetry-seam-with-mandatory-redaction-and-the-otel-backend.md): Shares source implementation: `packages/session/session-telemetry-otel/src/index.ts`, `packages/session/session-telemetry-otel/src/invariant.ts`.
- **`shares-code-with`** — [0273. Feedback acknowledgement sharing disclosure](0273-feedback-acknowledgement-sharing-disclosure.md): Shares source implementation: `packages/session/session-telemetry-otel`, `packages/session/session-telemetry-otel/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0258-feedback-gated-session-telemetry.md`.
