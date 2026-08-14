---
id: "dsh-note-0176"
title: "Session telemetry seam with mandatory redaction and the OTel backend"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-23-session-telemetry-otel-revival.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/trust"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "shutdown"
  - "url"
  - "SessionTelemetryCoordinator"
  - "SessionTelemetrySink"
  - "SessionTelemetryBackend"
  - "emit"
  - "flush"
  - "next"
  - "session-telemetry-otlp-rfc"
  - "packages/session/"
  - "telemetry/"
  - "@deepseek-ai/dsh-session-telemetry"
  - "structuredClone"
  - "agent/error"
search_regex: "(?i)(shutdown|SessionTelemetryCoordinator|SessionTelemetrySink|SessionTelemetryBackend|emit|flush|next|session\\-telemetry\\-otlp\\-rfc)"
---

# 0176. Session telemetry seam with mandatory redaction and the OTel backend — implementation context

## Open this when

Every deployment that wants harness sessions in an observability stack must hand-roll a session-log consumer: subscription, lifecycle handoff, and --- hardest --- redaction, since the raw log carries file contents and command output that may embed credentials. A telemetry seam and OTel backend shipped once on the session-telemetry-otlp-rfc branch (PR #222/#231) but never reached master: the proposal exported raw session events verbatim, which legal review declined.

## Source decision

packages/session/ (formerly telemetry/) revives the two reviewed packages under the SDK stance --- the harness provides the capability, the deployment configures where records go and owns what leaves in them: @deepseek-ai/dsh-session-telemetry --- the seam. SessionTelemetrySink (emit/flush?/shutdown), the service-registered SessionTelemetryBackend form, and SessionTelemetryCoordinator owning capture: live adoption with cursor read-back and the per-append firehose (project → structuredClone → redact → emit, zero I/O), buffer-free on-demand replay from the canonical log, the fixed first-chunk-per-(turn, step).

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-23-session-telemetry-otel-revival.md](../02-notes/implemented/feature/2026-07-23-session-telemetry-otel-revival.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-23-session-telemetry-otel-revival.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-23-session-telemetry-otel-revival.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/session/session-telemetry/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/session`. Core file in the package named by the note: `packages/session/session-telemetry`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/session/session-telemetry/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-telemetry`. | `named-package-member` |
| [`packages/session/session-telemetry-otel/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/session`. Core file in the package named by the note: `packages/session/session-telemetry-otel`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/session/session-telemetry/src/coordinator.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry/src/coordinator.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/session`. Core file in the package named by the note: `packages/session/session-telemetry`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/session/session-telemetry-otel/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-telemetry-otel`. | `named-package-member` |
| [`packages/session/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/session`. | `named-directory-member` |
| [`packages/session/session-stats/src/projection.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-stats/src/projection.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/session`. | `named-directory-member` |
| [`packages/session/session-projection/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/session`. | `named-directory-member` |
| [`packages/session/session-persistence-jsonl/src/win32.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/win32.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/session`. | `named-directory-member` |
| [`packages/session/session-persistence/src/coordinator.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/coordinator.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/session`. | `named-directory-member` |
| [`packages/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/session/session-telemetry`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `shutdown` | `const` | [`apps/cli/src/profile-boot.ts:210`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L210) | `const shutdown = createProcessShutdown(async () => { await app.current?.fiber.dispose() })` |
| `url` | `const` | [`packages/session/session-telemetry-otel/src/index.ts:170`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/index.ts#L170) | `const url = config.exporter?.url` |
| `SessionTelemetryCoordinator` | `class` | [`packages/session/session-telemetry/src/coordinator.ts:60`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry/src/coordinator.ts#L60) | `export class SessionTelemetryCoordinator {` |
| `SessionTelemetrySink` | `interface` | [`packages/session/session-telemetry/src/index.ts:94`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry/src/index.ts#L94) | `export interface SessionTelemetrySink {` |
| `SessionTelemetryBackend` | `class` | [`packages/session/session-telemetry/src/index.ts:148`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry/src/index.ts#L148) | `export abstract class SessionTelemetryBackend extends Service implements SessionTelemetrySink {` |
| `emit` | `function` | [`packages/test-support/llm-mock-server/src/index.ts:291`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-mock-server/src/index.ts#L291) | `function emit(options: ResolvedOptions, event: MockLlmServerEvent): void {` |
| `flush` | `const` | [`packages/typert/loader/src/index.ts:392`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts#L392) | `const flush = (onError: (error: Error) => void): Promise<void>[] => {` |
| `next` | `const` | [`vendor/loader/src/index.ts:181`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts#L181) | `const next = fiber.parent.fiber` |

### Tests and executable evidence

- [`apps/cli/tests/process-shutdown.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/process-shutdown.spec.ts) — A test under the owning area exercises or imports `shutdown`.
- [`apps/cli/tests/headless-shutdown.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/headless-shutdown.e2e.ts) — A test under the owning area exercises or imports `shutdown`.
- [`apps/cli/tests/fixtures/never-dispose.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/never-dispose.mjs) — A test under the owning area exercises or imports `shutdown`.
- [`packages/session/session-telemetry/tests/redact.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry/tests/redact.spec.ts) — A test under the owning area exercises or imports `SessionTelemetrySink`. A test under the owning area exercises or imports `SessionTelemetryCoordinator`.
- [`packages/session/session-telemetry/tests/telemetry.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry/tests/telemetry.spec.ts) — A test under the owning area exercises or imports `SessionTelemetrySink`. A test under the owning area exercises or imports `SessionTelemetryCoordinator`.
- [`packages/session/session-telemetry-otel/tests/otel.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/tests/otel.spec.ts) — A test under the owning area exercises or imports `BatchLogRecordProcessor`. A test under the owning area exercises or imports `exporter`.
- [`packages/session/session-telemetry-otel/tests/loader-composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/tests/loader-composition.e2e.ts) — A test under the owning area exercises or imports `DISABLED`. A test under the owning area exercises or imports `FEEDBACK_ONLY`.
- [`packages/core/agent-loop/tests/scope-lifecycle.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/scope-lifecycle.spec.ts) — Contains the exact code literal `agent/error` named by the note.

## How to read the implementation

1. Start with [`packages/session/session-telemetry/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/trust`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `shutdown`, `url`, `SessionTelemetryCoordinator`, `SessionTelemetrySink`, `SessionTelemetryBackend`, `emit`, `flush`, `next`, `session-telemetry-otlp-rfc`, `packages/session/`, `telemetry/`, `@deepseek-ai/dsh-session-telemetry`, `structuredClone`, `agent/error`
- Regex: `(?i)(shutdown|SessionTelemetryCoordinator|SessionTelemetrySink|SessionTelemetryBackend|emit|flush|next|session\-telemetry\-otlp\-rfc)`

```bash
rg -n --pcre2 "(?i)(shutdown|SessionTelemetryCoordinator|SessionTelemetrySink|SessionTelemetryBackend|emit|flush|next|session\\-telemetry\\-otlp\\-rfc)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0258. Feedback-gated session telemetry](0258-feedback-gated-session-telemetry.md): The source note links to this decision directly.
- **`source-link`** — [0286. SessionTelemetryBackend requires explicit opt-in](0286-sessiontelemetrybackend-requires-explicit-opt-in.md): The source note links to this decision directly.
- **`source-link`** — [0476. Buffer-free feedback telemetry](0476-buffer-free-feedback-telemetry.md): The source note links to this decision directly.
- **`shares-code-with`** — [0273. Feedback acknowledgement sharing disclosure](0273-feedback-acknowledgement-sharing-disclosure.md): Shares source implementation: `packages/session/session-telemetry-otel/src/index.ts`, `packages/session/session-telemetry-otel/src/invariant.ts`.
- **`shares-code-with`** — [0233. SessionTelemetryBackend anonymous user id ($DSH_HOME/.anonymous-user-id) and the OTel Resource user.id](0233-sessiontelemetrybackend-anonymous-user-id-dsh-home-anonymous-user-id-and.md): Shares source implementation: `packages/session/session-telemetry-otel/src/index.ts`, `packages/session/session-telemetry-otel/src/invariant.ts`.
- **`shares-code-with`** — [0236. Default session-telemetry mount (OTel reporting) in the dsh web composition](0236-default-session-telemetry-mount-otel-reporting-in-the-dsh-web-compositio.md): Shares source implementation: `packages/session/session-telemetry-otel/src/index.ts`, `packages/session/session-telemetry-otel/src/invariant.ts`.
- **`shares-code-with`** — [0338. Bounded, escalating signal shutdown for Web and headless](0338-bounded-escalating-signal-shutdown-for-web-and-headless.md): Shares source implementation: `packages/session/session-telemetry-otel/src/index.ts`.
- **`same-design-pressure`** — [0040. LSP capability seam and model-facing query tool](0040-lsp-capability-seam-and-model-facing-query-tool.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0176-session-telemetry-seam-with-mandatory-redaction-and-the-otel-backend.md`.
