---
id: "dsh-note-0338"
title: "Bounded, escalating signal shutdown for Web and headless"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-03-cli-signal-shutdown-escalation.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
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
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "exitCode"
  - "createProcessShutdown"
  - "shutdown"
  - "shutdownTimeoutMillis"
  - "dsh --profile headless"
  - "ctx.fiber.dispose"
  - "Ctrl+C"
  - "DSH_TELEMETRY_DISABLED=1"
  - "BatchLogRecordProcessor.shutdown"
  - "exporter.forceFlush"
  - "exportTimeoutMillis"
  - "forceFlush"
  - "process.exitCode"
  - "process.exit"
search_regex: "(?i)(exitCode|createProcessShutdown|shutdown|shutdownTimeoutMillis|dsh[- ]\\-\\-profile[- ]headless|ctx\\.fiber\\.dispose|Ctrl\\+C|DSH_TELEMETRY_DISABLED=1)"
---

# 0338. Bounded, escalating signal shutdown for Web and headless — implementation context

## Open this when

The default telemetry mount added SIGINT/SIGTERM handlers to dsh web and the headless command (now dsh --profile headless) so process exit could drain the Cordis tree instead of dropping queued telemetry. Each handler used a one-way boolean latch and exited only after ctx.fiber.dispose() settled. Headless normal completion also awaited that disposal without a bound. A user then reproduced the headless command hanging immediately after the observation URL and ignoring repeated Ctrl+C; DSH_TELEMETRY_DISABLED=1 removed the hang, while a standalone Node handler in the same Linux sandbox received SIGINT.

## Source decision

The fix has two ownership layers. The OTel backend adds shutdownTimeoutMillis (default and shipped value: three seconds) around the SDK provider's complete shutdown Promise. Crossing it rejects into the telemetry coordinator's existing contained-failure path, allowing the Cordis tree to finish disposal; pending records may be lost because OTel exposes no cancellation for the transport Promise. Web and headless share createProcessShutdown, one process-level controller around root disposal: Normal shutdown calls coalesce onto one disposal and retain the first requested exit code; they never escalate one another.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-03-cli-signal-shutdown-escalation.md](../02-notes/implemented/bug-fix/2026-08-03-cli-signal-shutdown-escalation.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-03-cli-signal-shutdown-escalation.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-03-cli-signal-shutdown-escalation.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/cli/src/plugin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts) | runtime implementation | Defines `exitCode`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/src/profile-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts) | runtime implementation | Defines `shutdown`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/src/process-shutdown.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/process-shutdown.ts) | runtime implementation | Defines `createProcessShutdown`, a construct named by the note. | `symbol-definition` |
| [`packages/e2b/subprocess-e2b/src/process.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/process.ts) | runtime implementation | Defines `exitCode`, a construct named by the note. | `symbol-definition` |
| [`packages/hooks/hook-protocol/src/runner.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/runner.ts) | runtime implementation | Defines `exitCode`, a construct named by the note. | `symbol-definition` |
| [`packages/sandbox/sandbox-windows-acl/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/index.ts) | package entry point | Defines `exitCode`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-telemetry-otel/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/index.ts) | package entry point | Defines `shutdownTimeoutMillis`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `exitCode` | `const` | [`apps/cli/src/plugin.ts:142`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts#L142) | `const exitCode = result.status ?? 1` |
| `createProcessShutdown` | `function` | [`apps/cli/src/process-shutdown.ts:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/process-shutdown.ts#L22) | `export function createProcessShutdown(` |
| `shutdown` | `const` | [`apps/cli/src/profile-boot.ts:210`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L210) | `const shutdown = createProcessShutdown(async () => { await app.current?.fiber.dispose() })` |
| `exitCode` | `const` | [`packages/e2b/subprocess-e2b/src/process.ts:525`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/process.ts#L525) | `const exitCode = Number(rawStatus)` |
| `exitCode` | `const` | [`packages/hooks/hook-protocol/src/runner.ts:91`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/runner.ts#L91) | `const exitCode = result.exitCode ?? undefined` |
| `exitCode` | `const` | [`packages/sandbox/sandbox-windows-acl/src/index.ts:365`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/index.ts#L365) | `const exitCode = await exitCodePromise` |
| `shutdownTimeoutMillis` | `const` | [`packages/session/session-telemetry-otel/src/index.ts:192`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/index.ts#L192) | `const shutdownTimeoutMillis = config.shutdownTimeoutMillis ?? DEFAULT_SHUTDOWN_TIMEOUT_MILLIS` |

### Tests and executable evidence

- [`apps/cli/tests/headless-shutdown.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/headless-shutdown.e2e.ts) — The source note names this file directly. A test under the owning area exercises or imports `exitCode`.
- [`apps/cli/tests/process-shutdown.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/process-shutdown.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `createProcessShutdown`.
- [`packages/session/session-telemetry-otel/tests/otel.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/tests/otel.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `exportTimeoutMillis`.
- [`apps/cli/tests/args.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/args.spec.ts) — A test under the owning area exercises or imports `exitCode`.
- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — A test under the owning area exercises or imports `exitCode`.
- [`apps/cli/tests/fixtures/never-dispose.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/never-dispose.mjs) — A test under the owning area exercises or imports `shutdown`.
- [`apps/cli/tests/source-launch.compat.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/source-launch.compat.spec.ts) — A test under the owning area exercises or imports `exitCode`.
- Source verification intent: apps/cli/tests/process-shutdown.spec.ts pins natural completion after resolved disposal, forced exit after rejected disposal, the five-second backstop, normal-call coalescing, signal-owned disposal, a signal interrupting normal disposal or post-disposal handle draining, and second-signal escalation. apps/cli/tests/headless-shutdown.e2e.ts boots the real shipped Web/headless Loader tree in a PTY with a test-only plugin whose disposer announces entry and never settles. The test sends SIGINT after the observation URL, waits for proof that disposal started, sends SIGINT again, and requires exit 130.

## How to read the implementation

1. Start with [`apps/cli/src/plugin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/trust`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `exitCode`, `createProcessShutdown`, `shutdown`, `shutdownTimeoutMillis`, `dsh --profile headless`, `ctx.fiber.dispose`, `Ctrl+C`, `DSH_TELEMETRY_DISABLED=1`, `BatchLogRecordProcessor.shutdown`, `exporter.forceFlush`, `exportTimeoutMillis`, `forceFlush`, `process.exitCode`, `process.exit`
- Regex: `(?i)(exitCode|createProcessShutdown|shutdown|shutdownTimeoutMillis|dsh[- ]\-\-profile[- ]headless|ctx\.fiber\.dispose|Ctrl\+C|DSH_TELEMETRY_DISABLED=1)`

```bash
rg -n --pcre2 "(?i)(exitCode|createProcessShutdown|shutdown|shutdownTimeoutMillis|dsh[- ]\\-\\-profile[- ]headless|ctx\\.fiber\\.dispose|Ctrl\\+C|DSH_TELEMETRY_DISABLED=1)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0236. Default session-telemetry mount (OTel reporting) in the dsh web composition](0236-default-session-telemetry-mount-otel-reporting-in-the-dsh-web-compositio.md): The source note links to this decision directly.
- **`shares-code-with`** — [0258. Feedback-gated session telemetry](0258-feedback-gated-session-telemetry.md): Shares source implementation: `apps/cli/src/profile-boot.ts`, `apps/cli/tests/process-shutdown.spec.ts`.
- **`shares-code-with`** — [0476. Buffer-free feedback telemetry](0476-buffer-free-feedback-telemetry.md): Shares source implementation: `apps/cli/src/profile-boot.ts`, `apps/cli/tests/headless-shutdown.e2e.ts`.
- **`shares-code-with`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): Shares source implementation: `apps/cli/src/plugin.ts`.
- **`shares-code-with`** — [0176. Session telemetry seam with mandatory redaction and the OTel backend](0176-session-telemetry-seam-with-mandatory-redaction-and-the-otel-backend.md): Shares source implementation: `packages/session/session-telemetry-otel/src/index.ts`.
- **`shares-code-with`** — [0042. Explicit turn cancellation capability](0042-explicit-turn-cancellation-capability.md): Shares source implementation: `apps/cli/src/profile-boot.ts`.
- **`shares-code-with`** — [0286. SessionTelemetryBackend requires explicit opt-in](0286-sessiontelemetrybackend-requires-explicit-opt-in.md): Shares source implementation: `packages/session/session-telemetry-otel/src/index.ts`, `packages/session/session-telemetry-otel/tests/otel.spec.ts`.
- **`shares-code-with`** — [0229. Even out the shipped tool rosters](0229-even-out-the-shipped-tool-rosters.md): Shares source implementation: `apps/cli/src/plugin.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0338-bounded-escalating-signal-shutdown-for-web-and-headless.md`.
