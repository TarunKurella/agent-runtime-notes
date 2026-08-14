---
id: "dsh-note-0291"
title: "DeepSeek request user and session identity headers"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-11-deepseek-request-user-id-header.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/performance"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/generation"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "sessionId"
  - "user"
  - "metadata"
  - "getOrCreateAnonymousUserId"
  - "resolveUserId"
  - "attributionHeaders"
  - "x-deepseek-harness-session-id"
  - "GenerateOptions.sessionId"
  - "baseURL"
  - "dsh-llm-deepseek"
  - "x-deepseek-harness-user-id"
  - "@deepseek-ai/dsh-anonymous-user-id"
  - "user.id"
  - "/feedback"
search_regex: "(?i)(sessionId|user|metadata|getOrCreateAnonymousUserId|resolveUserId|attributionHeaders|x\\-deepseek\\-harness\\-session\\-id|GenerateOptions\\.sessionId)"
---

# 0291. DeepSeek request user and session identity headers — implementation context

## Open this when

Direct DeepSeek requests already carried x-deepseek-harness-session-id when the caller supplied GenerateOptions.sessionId, which lets provider-side support and diagnostics correlate turns within one conversation. They lacked a stable identity across sessions even though the harness already persists an anonymous user id for telemetry and feedback. A separate id would break correlation, while putting it in the provider-neutral attribution helper would send a stable per-user identifier through every HTTP adapter. The user id is transport metadata, not model input.

## Source decision

dsh-llm-deepseek sends x-deepseek-harness-user-id on every provider request sent after successful credential resolution. The value comes from @deepseek-ai/dsh-anonymous-user-id and therefore matches the OpenTelemetry Resource user.id and /feedback acknowledgement for the same $DSH_HOME. The adapter continues to send x-deepseek-harness-session-id only when GenerateOptions.sessionId is present; the agent loop supplies the current durable Session.id for ordinary agent, title-generation, and compaction requests.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-11-deepseek-request-user-id-header.md](../02-notes/implemented/feature/2026-08-11-deepseek-request-user-id-header.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-11-deepseek-request-user-id-header.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-11-deepseek-request-user-id-header.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm-deepseek/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-deepseek`. Defines `resolveUserId`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-deepseek/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/adapter.ts) | provider/backend adapter | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/identity/anonymous-user-id/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/identity/anonymous-user-id/src/index.ts) | package entry point | Core file in the package named by the note: `packages/identity/anonymous-user-id`. Defines `getOrCreateAnonymousUserId`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/identity/anonymous-user-id/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/identity/anonymous-user-id/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/identity/anonymous-user-id`. | `named-package-member` |
| [`packages/llm/llm-deepseek`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/identity/anonymous-user-id`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/identity/anonymous-user-id) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `sessionId`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/attribution.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/attribution.ts) | runtime implementation | Defines `attributionHeaders`, a construct named by the note. | `symbol-definition` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Defines `user`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `metadata`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `sessionId` | `const` | [`packages/acp/acp/src/index.ts:254`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L254) | `const sessionId = SessionId(randomUUID())` |
| `user` | `const` | [`packages/boot/app-boot/src/index.ts:185`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L185) | `const user = home === resolve(cwd) ? undefined : readEnvLayer(binName, home, warn)` |
| `metadata` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:555`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L555) | `const metadata = sessionListMetadata(session.events)` |
| `getOrCreateAnonymousUserId` | `function` | [`packages/identity/anonymous-user-id/src/index.ts:68`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/identity/anonymous-user-id/src/index.ts#L68) | `export function getOrCreateAnonymousUserId(options: AnonymousUserIdOptions = {}): AnonymousUserId {` |
| `resolveUserId` | `const` | [`packages/llm/llm-deepseek/src/index.ts:249`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts#L249) | `const resolveUserId = (): AnonymousUserId => userId ??= getOrCreateAnonymousUserId()` |
| `attributionHeaders` | `function` | [`packages/llm/llm/src/attribution.ts:64`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/attribution.ts#L64) | `export function attributionHeaders(` |

### Tests and executable evidence

- [`packages/llm/llm/tests/attribution.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/attribution.spec.ts) — A test under the owning area exercises or imports `attributionHeaders`.
- [`packages/llm/llm-deepseek/tests/adapter.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/tests/adapter.e2e.ts) — A test under the owning area exercises or imports `dsh-llm-deepseek`.
- [`packages/llm/llm-deepseek/tests/adapter.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/tests/adapter.spec.ts) — A test under the owning area exercises or imports `x-deepseek-harness-session-id`. A test under the owning area exercises or imports `dsh-llm-deepseek`.
- [`packages/llm/llm-deepseek/tests/dynamic-config.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/tests/dynamic-config.spec.ts) — A test under the owning area exercises or imports `dsh-llm-deepseek`.
- [`packages/llm/llm-deepseek/tests/loader-composition.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/tests/loader-composition.spec.ts) — A test under the owning area exercises or imports `dsh-llm-deepseek`. A test under the owning area exercises or imports `x-deepseek-harness-user-id`.
- [`packages/identity/anonymous-user-id/tests/anonymous-user-id.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/identity/anonymous-user-id/tests/anonymous-user-id.spec.ts) — A test under the owning area exercises or imports `getOrCreateAnonymousUserId`.
- Source verification intent: The mock provider asserts that an authorized request carries the same user id returned by getOrCreateAnonymousUserId() and omits the session header when no session id is supplied. The session-identity wire test asserts both headers and preserves the exact supplied session id. A direct-adapter test asserts that user-id resolution happens once per stream, while the keyless configuration test proves a credential failure does not create .anonymous-user-id. The real Loader composition test asserts that the assembled plugin uses the shared user-id package rather than a test-only value.

## How to read the implementation

1. Start with [`packages/llm/llm-deepseek/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
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

- Tags: `class/feature`, `concern/boundary`, `concern/evidence`, `concern/performance`, `concern/schema-types`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/generation`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `sessionId`, `user`, `metadata`, `getOrCreateAnonymousUserId`, `resolveUserId`, `attributionHeaders`, `x-deepseek-harness-session-id`, `GenerateOptions.sessionId`, `baseURL`, `dsh-llm-deepseek`, `x-deepseek-harness-user-id`, `@deepseek-ai/dsh-anonymous-user-id`, `user.id`, `/feedback`
- Regex: `(?i)(sessionId|user|metadata|getOrCreateAnonymousUserId|resolveUserId|attributionHeaders|x\-deepseek\-harness\-session\-id|GenerateOptions\.sessionId)`

```bash
rg -n --pcre2 "(?i)(sessionId|user|metadata|getOrCreateAnonymousUserId|resolveUserId|attributionHeaders|x\\-deepseek\\-harness\\-session\\-id|GenerateOptions\\.sessionId)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0371. First-run readiness reads every provider, and the setup card closes](0371-first-run-readiness-reads-every-provider-and-the-setup-card-closes.md): Shares source implementation: `packages/llm/llm-deepseek`, `packages/llm/llm-deepseek/src/adapter.ts`.
- **`shares-code-with`** — [0213. official DeepSeek first-run credential setup](0213-official-deepseek-first-run-credential-setup.md): Shares source implementation: `packages/llm/llm-deepseek`, `packages/llm/llm-deepseek/src/index.ts`.
- **`shares-code-with`** — [0017. Mandatory `User-Agent` attribution for provider requests](0017-mandatory-user-agent-attribution-for-provider-requests.md): Shares source implementation: `packages/llm/llm-deepseek/src/adapter.ts`, `packages/llm/llm-deepseek/src/index.ts`.
- **`shares-code-with`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/invariant.ts`.
- **`shares-code-with`** — [0094. pi-ai routes are declared providers, not catalog lookups](0094-pi-ai-routes-are-declared-providers-not-catalog-lookups.md): Shares source implementation: `packages/llm/llm-deepseek/src/adapter.ts`, `packages/llm/llm-deepseek/src/index.ts`.
- **`shares-code-with`** — [0501. Scriptable LLM wire fault server](0501-scriptable-llm-wire-fault-server.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/invariant.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/types.ts`.
- **`shares-code-with`** — [0109. Shared anonymous user id across telemetry, feedback, and DeepSeek requests](0109-shared-anonymous-user-id-across-telemetry-feedback-and-deepseek-requests.md): Shares source implementation: `packages/identity/anonymous-user-id`, `packages/identity/anonymous-user-id/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0291-deepseek-request-user-and-session-identity-headers.md`.
