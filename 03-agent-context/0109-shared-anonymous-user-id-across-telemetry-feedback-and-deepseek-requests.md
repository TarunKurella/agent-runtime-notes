---
id: "dsh-note-0109"
title: "Shared anonymous user id across telemetry, feedback, and DeepSeek requests"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-07-shared-feedback-telemetry-user-id.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/llm"
  - "domain/observability"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/adapter"
aliases:
  - "getOrCreateAnonymousUserId"
  - "$DSH_HOME/.anonymous-user-id"
  - "/feedback"
  - "session-telemetry-otel"
  - "@deepseek-ai/dsh-anonymous-user-id"
  - "user.id"
  - "Feedback recorded for session {sessionId}"
  - "Anonymous user: {userId}"
  - "x-deepseek-harness-user-id"
  - ".anonymous-user-id"
  - "Shared anonymous user id across telemetry, feedback, and DeepSeek requests"
  - "architecture"
  - "boundary"
  - "concurrency"
search_regex: "(?i)(getOrCreateAnonymousUserId|\\$DSH_HOME/\\.anonymous\\-user\\-id|/feedback|session\\-telemetry\\-otel|@deepseek\\-ai/dsh\\-anonymous\\-user\\-id|user\\.id|Feedback[- ]recorded[- ]for[- ]session[- ]\\{sessionId\\}|Anonymous[- ]user:[- ]\\{userId\\})"
---

# 0109. Shared anonymous user id across telemetry, feedback, and DeepSeek requests — implementation context

## Open this when

The OpenTelemetry backend already persisted one anonymous UUID in $DSH_HOME/.anonymous-user-id. /feedback now needs to report both the receiving session id and a user id so an operator can correlate the acknowledgement with exported records. Duplicating or independently generating that identity would make the reported user meaningless, while importing it from session-telemetry-otel would make a direct command depend on an exporter backend and create a dependency cycle when feedback export is mounted by telemetry.

## Source decision

@deepseek-ai/dsh-anonymous-user-id owns getOrCreateAnonymousUserId() and the $DSH_HOME/.anonymous-user-id storage contract. session-telemetry-otel uses the returned id as OpenTelemetry Resource user.id; the /feedback success acknowledgement reports Feedback recorded for session {sessionId} followed by Anonymous user: {userId} on a second line; and direct DeepSeek requests carry it as x-deepseek-harness-user-id. Invalid feedback is rejected before resolving the id, and the DeepSeek adapter resolves it only after credentials succeed, so neither an empty command nor a credential failure creates .anonymous-user-id.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-07-shared-feedback-telemetry-user-id.md](../02-notes/implemented/architecture/2026-08-07-shared-feedback-telemetry-user-id.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-07-shared-feedback-telemetry-user-id.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-07-shared-feedback-telemetry-user-id.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/identity/anonymous-user-id/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/identity/anonymous-user-id/src/index.ts) | package entry point | Core file in the package named by the note: `packages/identity/anonymous-user-id`. Defines `getOrCreateAnonymousUserId`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/identity/anonymous-user-id/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/identity/anonymous-user-id/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/identity/anonymous-user-id`. | `named-package-member` |
| [`packages/session/session-telemetry-otel/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session/session-telemetry-otel`. | `named-package-member` |
| [`packages/session/session-telemetry-otel/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-telemetry-otel`. | `named-package-member` |
| [`packages/identity/anonymous-user-id`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/identity/anonymous-user-id) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session/session-telemetry-otel`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/identity/anonymous-user-id/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/identity/anonymous-user-id/README.md) | package contract and examples | Core file in the package named by the note: `packages/identity/anonymous-user-id`. | `named-package-member` |
| [`packages/identity/anonymous-user-id/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/identity/anonymous-user-id/package.json) | composition and configuration | Core file in the package named by the note: `packages/identity/anonymous-user-id`. | `named-package-member` |
| [`packages/session/session-telemetry-otel/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/README.md) | package contract and examples | Core file in the package named by the note: `packages/session/session-telemetry-otel`. | `named-package-member` |
| [`packages/session/session-telemetry-otel/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/package.json) | composition and configuration | Core file in the package named by the note: `packages/session/session-telemetry-otel`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `getOrCreateAnonymousUserId` | `function` | [`packages/identity/anonymous-user-id/src/index.ts:68`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/identity/anonymous-user-id/src/index.ts#L68) | `export function getOrCreateAnonymousUserId(options: AnonymousUserIdOptions = {}): AnonymousUserId {` |

### Tests and executable evidence

- [`packages/session/session-telemetry-otel/tests/otel.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/tests/otel.spec.ts) — A test under the owning area exercises or imports `getOrCreateAnonymousUserId`.
- [`packages/identity/anonymous-user-id/tests/anonymous-user-id.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/identity/anonymous-user-id/tests/anonymous-user-id.spec.ts) — A test under the owning area exercises or imports `getOrCreateAnonymousUserId`.
- [`packages/session/session-telemetry-otel/tests/loader-composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/tests/loader-composition.e2e.ts) — A test under the owning area exercises or imports `session-telemetry-otel`.

## How to read the implementation

1. Start with [`packages/identity/anonymous-user-id/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/identity/anonymous-user-id/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/schema-types`, `domain/llm`, `domain/observability`, `domain/security`, `domain/session-state`, `domain/storage`, `lifecycle/implemented`, `mechanism/adapter`
- Aliases: `getOrCreateAnonymousUserId`, `$DSH_HOME/.anonymous-user-id`, `/feedback`, `session-telemetry-otel`, `@deepseek-ai/dsh-anonymous-user-id`, `user.id`, `Feedback recorded for session {sessionId}`, `Anonymous user: {userId}`, `x-deepseek-harness-user-id`, `.anonymous-user-id`, `Shared anonymous user id across telemetry, feedback, and DeepSeek requests`, `architecture`, `boundary`, `concurrency`
- Regex: `(?i)(getOrCreateAnonymousUserId|\$DSH_HOME/\.anonymous\-user\-id|/feedback|session\-telemetry\-otel|@deepseek\-ai/dsh\-anonymous\-user\-id|user\.id|Feedback[- ]recorded[- ]for[- ]session[- ]\{sessionId\}|Anonymous[- ]user:[- ]\{userId\})`

```bash
rg -n --pcre2 "(?i)(getOrCreateAnonymousUserId|\\$DSH_HOME/\\.anonymous\\-user\\-id|/feedback|session\\-telemetry\\-otel|@deepseek\\-ai/dsh\\-anonymous\\-user\\-id|user\\.id|Feedback[- ]recorded[- ]for[- ]session[- ]\\{sessionId\\}|Anonymous[- ]user:[- ]\\{userId\\})" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0233. SessionTelemetryBackend anonymous user id ($DSH_HOME/.anonymous-user-id) and the OTel Resource user.id](0233-sessiontelemetrybackend-anonymous-user-id-dsh-home-anonymous-user-id-and.md): The source note links to this decision directly.
- **`source-link`** — [0291. DeepSeek request user and session identity headers](0291-deepseek-request-user-and-session-identity-headers.md): The source note links to this decision directly.
- **`shares-code-with`** — [0236. Default session-telemetry mount (OTel reporting) in the dsh web composition](0236-default-session-telemetry-mount-otel-reporting-in-the-dsh-web-compositio.md): Shares source implementation: `packages/session/session-telemetry-otel`, `packages/session/session-telemetry-otel/README.md`.
- **`shares-code-with`** — [0258. Feedback-gated session telemetry](0258-feedback-gated-session-telemetry.md): Shares source implementation: `packages/session/session-telemetry-otel`, `packages/session/session-telemetry-otel/README.md`.
- **`shares-code-with`** — [0286. SessionTelemetryBackend requires explicit opt-in](0286-sessiontelemetrybackend-requires-explicit-opt-in.md): Shares source implementation: `packages/session/session-telemetry-otel`, `packages/session/session-telemetry-otel/README.md`.
- **`shares-code-with`** — [0273. Feedback acknowledgement sharing disclosure](0273-feedback-acknowledgement-sharing-disclosure.md): Shares source implementation: `packages/session/session-telemetry-otel`, `packages/session/session-telemetry-otel/src/index.ts`.
- **`shares-code-with`** — [0176. Session telemetry seam with mandatory redaction and the OTel backend](0176-session-telemetry-seam-with-mandatory-redaction-and-the-otel-backend.md): Shares source implementation: `packages/session/session-telemetry-otel/src/index.ts`, `packages/session/session-telemetry-otel/src/invariant.ts`.
- **`shares-code-with`** — [0338. Bounded, escalating signal shutdown for Web and headless](0338-bounded-escalating-signal-shutdown-for-web-and-headless.md): Shares source implementation: `packages/session/session-telemetry-otel/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0109-shared-anonymous-user-id-across-telemetry-feedback-and-deepseek-requests.md`.
