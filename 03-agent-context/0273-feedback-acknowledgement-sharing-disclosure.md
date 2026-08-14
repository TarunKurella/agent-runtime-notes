---
id: "dsh-note-0273"
title: "Feedback acknowledgement sharing disclosure"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-07-feedback-acknowledgement-sharing-disclosure.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/recovery"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "CommandNode"
  - "disabled"
  - "telemetry"
  - "SessionTelemetryMode"
  - "SessionTelemetrySharingStatus"
  - "SessionTelemetryBackend"
  - "full"
  - "/feedback"
  - "feedback/record"
  - "FULL"
  - "FEEDBACK_ONLY"
  - "DISABLED"
  - "@deepseek-ai/dsh-session-telemetry"
  - "feedback-only"
search_regex: "(?i)(CommandNode|disabled|telemetry|SessionTelemetryMode|SessionTelemetrySharingStatus|SessionTelemetryBackend|full|/feedback)"
---

# 0273. Feedback acknowledgement sharing disclosure — implementation context

## Open this when

The /feedback command records a log-only feedback/record event and acknowledges the user, but the acknowledgement carried no durable context about what happened to the session: deployments that mount session telemetry (FULL, FEEDBACK_ONLY, or DISABLED) had no way to tell the user whether their feedback and session left the process, and the receiving session id was not echoed. The command plugin could not read the sharing policy because the telemetry seam exposed capture only, and the OTel mode enum lived in the optional backend package.

## Source decision

The disclosure states the current sharing policy only; it never promises delivery or retention. Handoff is the backend's non-blocking enqueue and batching, retry, and loss policy stay the backend SDK's, and a later reconfiguration can change what was shared, so the sentences claim nothing about what reached a collector or about future retention. The disclosure adds no session event and never reaches the model surface; the web client renders it through the existing command row (CommandNode outcome text) with no client change.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-07-feedback-acknowledgement-sharing-disclosure.md](../02-notes/implemented/feature/2026-08-07-feedback-acknowledgement-sharing-disclosure.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-07-feedback-acknowledgement-sharing-disclosure.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-07-feedback-acknowledgement-sharing-disclosure.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/feedback/command-feedback/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/command-feedback/src/index.ts) | package entry point | Core file in the package named by the note: `packages/feedback/command-feedback`. Defines `telemetry`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session/session-telemetry/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session/session-telemetry`. Defines `SessionTelemetrySharingStatus`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/feedback/command-feedback/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/command-feedback/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/feedback/command-feedback`. | `named-package-member` |
| [`packages/session/session-telemetry/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-telemetry`. | `named-package-member` |
| [`packages/session/session-telemetry-otel/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session/session-telemetry-otel`. Defines `SessionTelemetryMode`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session/session-telemetry-otel/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-telemetry-otel`. | `named-package-member` |
| [`packages/feedback/command-feedback`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/command-feedback) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session/session-telemetry`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session/session-telemetry-otel`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/web/tool-web/src/fetch.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts) | runtime implementation | Defines `full`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-settings-plugins/src/client/BashCard.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/client/BashCard.tsx) | runtime implementation | Defines `disabled`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/sessions/conversation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts) | runtime implementation | Defines `CommandNode`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `CommandNode` | `interface` | [`packages/client/runtime/src/client/sessions/conversation.ts:256`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts#L256) | `export interface CommandNode {` |
| `disabled` | `const` | [`packages/client/ui-settings-plugins/src/client/BashCard.tsx:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/client/BashCard.tsx#L23) | `const disabled = !state.writable` |
| `telemetry` | `const` | [`packages/feedback/command-feedback/src/index.ts:92`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/command-feedback/src/index.ts#L92) | `const telemetry = ctx.get('sessionTelemetry')` |
| `SessionTelemetryMode` | `enum` | [`packages/session/session-telemetry-otel/src/index.ts:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/index.ts#L44) | `export enum SessionTelemetryMode {` |
| `SessionTelemetrySharingStatus` | `type` | [`packages/session/session-telemetry/src/index.ts:140`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry/src/index.ts#L140) | `export type SessionTelemetrySharingStatus = 'full' \| 'feedback-only' \| 'disabled'` |
| `SessionTelemetryBackend` | `class` | [`packages/session/session-telemetry/src/index.ts:148`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry/src/index.ts#L148) | `export abstract class SessionTelemetryBackend extends Service implements SessionTelemetrySink {` |
| `full` | `const` | [`packages/web/tool-web/src/fetch.ts:315`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts#L315) | `const full = \`${prefix}${truncated ? TRUNCATION_FOOTER : ''}\`` |

### Tests and executable evidence

- [`packages/session/session-telemetry-otel/tests/otel.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/tests/otel.spec.ts) — A test under the owning area exercises or imports `FULL`. A test under the owning area exercises or imports `FEEDBACK_ONLY`.
- [`packages/feedback/command-feedback/tests/command-feedback.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/command-feedback/tests/command-feedback.spec.ts) — A test under the owning area exercises or imports `SessionTelemetrySharingStatus`. A test under the owning area exercises or imports `feedback-only`.
- [`packages/feedback/command-feedback/tests/loader-composition.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/command-feedback/tests/loader-composition.spec.ts) — A test under the owning area exercises or imports `sharing`.
- [`packages/session/session-telemetry-otel/tests/loader-composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/tests/loader-composition.e2e.ts) — A test under the owning area exercises or imports `FEEDBACK_ONLY`. A test under the owning area exercises or imports `DISABLED`.
- [`apps/web/tests/feedback-command.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/feedback-command.e2e.ts) — Contains the exact code literal `feedback/record` named by the note.

## How to read the implementation

1. Start with [`packages/feedback/command-feedback/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/command-feedback/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/recovery`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/storage`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `CommandNode`, `disabled`, `telemetry`, `SessionTelemetryMode`, `SessionTelemetrySharingStatus`, `SessionTelemetryBackend`, `full`, `/feedback`, `feedback/record`, `FULL`, `FEEDBACK_ONLY`, `DISABLED`, `@deepseek-ai/dsh-session-telemetry`, `feedback-only`
- Regex: `(?i)(CommandNode|disabled|telemetry|SessionTelemetryMode|SessionTelemetrySharingStatus|SessionTelemetryBackend|full|/feedback)`

```bash
rg -n --pcre2 "(?i)(CommandNode|disabled|telemetry|SessionTelemetryMode|SessionTelemetrySharingStatus|SessionTelemetryBackend|full|/feedback)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0258. Feedback-gated session telemetry](0258-feedback-gated-session-telemetry.md): The source note links to this decision directly.
- **`shares-code-with`** — [0176. Session telemetry seam with mandatory redaction and the OTel backend](0176-session-telemetry-seam-with-mandatory-redaction-and-the-otel-backend.md): Shares source implementation: `packages/session/session-telemetry-otel/src/index.ts`, `packages/session/session-telemetry-otel/src/invariant.ts`.
- **`shares-code-with`** — [0109. Shared anonymous user id across telemetry, feedback, and DeepSeek requests](0109-shared-anonymous-user-id-across-telemetry-feedback-and-deepseek-requests.md): Shares source implementation: `packages/session/session-telemetry-otel`, `packages/session/session-telemetry-otel/src/index.ts`.
- **`shares-code-with`** — [0286. SessionTelemetryBackend requires explicit opt-in](0286-sessiontelemetrybackend-requires-explicit-opt-in.md): Shares source implementation: `packages/session/session-telemetry-otel`, `packages/session/session-telemetry-otel/src/index.ts`.
- **`shares-code-with`** — [0236. Default session-telemetry mount (OTel reporting) in the dsh web composition](0236-default-session-telemetry-mount-otel-reporting-in-the-dsh-web-compositio.md): Shares source implementation: `packages/session/session-telemetry-otel`, `packages/session/session-telemetry-otel/src/index.ts`.
- **`shares-code-with`** — [0233. SessionTelemetryBackend anonymous user id ($DSH_HOME/.anonymous-user-id) and the OTel Resource user.id](0233-sessiontelemetrybackend-anonymous-user-id-dsh-home-anonymous-user-id-and.md): Shares source implementation: `packages/session/session-telemetry-otel`, `packages/session/session-telemetry-otel/src/index.ts`.
- **`shares-code-with`** — [0202. `/feedback` command](0202-feedback-command.md): Shares source implementation: `packages/feedback/command-feedback/src/index.ts`, `packages/feedback/command-feedback/src/invariant.ts`.
- **`shares-code-with`** — [0032. Tool output spill policy](0032-tool-output-spill-policy.md): Shares source implementation: `packages/web/tool-web/src/fetch.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0273-feedback-acknowledgement-sharing-disclosure.md`.
