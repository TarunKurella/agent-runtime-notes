---
id: "dsh-note-0236"
title: "Default session-telemetry mount (OTel reporting) in the dsh web composition"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-31-web-telemetry-default-mount.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/security"
  - "domain/session-state"
  - "domain/testing"
  - "lifecycle/implemented"
aliases:
  - "cwd"
  - "url"
  - "packages/bundle/base/cordis.patch.yml"
  - "session-telemetry-otel"
  - "DISABLED"
  - "FULL"
  - "FEEDBACK_ONLY"
  - "DSH_TELEMETRY_MODE"
  - "DSH_TELEMETRY_OTLP_URL"
  - "https://harness-telemetry.deepseeksvc.com/v1/logs"
  - "DSH_TELEMETRY_DISABLED"
  - "processor.scheduledDelayMillis: 10000"
  - "exporter.timeoutMillis: 1000"
  - "maxExportBatchSize: 2048"
search_regex: "(?i)(packages/bundle/base/cordis\\.patch\\.yml|session\\-telemetry\\-otel|DISABLED|FULL|FEEDBACK_ONLY|DSH_TELEMETRY_MODE|DSH_TELEMETRY_OTLP_URL|https://harness\\-telemetry\\.deepseeksvc\\.com/v1/logs)"
---

# 0236. Default session-telemetry mount (OTel reporting) in the dsh web composition — implementation context

## Open this when

The telemetry seam and OTel backend (revival Note) had never been wired into any deployment composition since completion: no roster row, no switch, no cadence ruling, and zero observability over user sessions for the internal deployment. A deployment decision was needed: which surfaces report, to where, on what cadence, how to opt out, and how CI stays isolated.

## Source decision

The shared dsh base bundle (packages/bundle/base/cordis.patch.yml) mounts the session-telemetry-otel row with a baked-in production endpoint, so every profile has one consistent telemetry capability. The default-off decision keeps that row in DISABLED mode unless a deployment explicitly selects FULL or FEEDBACK_ONLY; the endpoint alone does not authorize reporting. Web and headless use the bounded, escalating process-shutdown controller on SIGINT/SIGTERM, giving an enabled backend's three-second shutdown deadline time to drain before the five-second launcher bound.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-31-web-telemetry-default-mount.md](../02-notes/implemented/feature/2026-07-31-web-telemetry-default-mount.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-31-web-telemetry-default-mount.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-31-web-telemetry-default-mount.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/bundle/base/cordis.patch.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/cordis.patch.yml) | composition and configuration | The source note names this file directly. Contains the exact code literal `session-telemetry/record` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/session/session-telemetry-otel/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session/session-telemetry-otel`. Defines `url`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session/session-telemetry-otel/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-telemetry-otel`. | `named-package-member` |
| [`packages/session/session-telemetry-otel`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/fs/tool-fs/src/session-cwd.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts) | runtime implementation | Defines `cwd`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-telemetry-otel/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/README.md) | package contract and examples | Core file in the package named by the note: `packages/session/session-telemetry-otel`. | `named-package-member` |
| [`packages/session/session-telemetry-otel/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/package.json) | composition and configuration | Core file in the package named by the note: `packages/session/session-telemetry-otel`. | `named-package-member` |
| [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) | package contract and examples | Contains the exact code literal `packages/bundle/base/cordis.patch.yml` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.zh.md) | package contract and examples | Contains the exact code literal `packages/bundle/base/cordis.patch.yml` named by the note. | `exact-code-occurrence` |
| [`apps/cli/composition.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/composition.md) | package contract and examples | Contains the exact code literal `packages/bundle/base/cordis.patch.yml` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Contains the exact code literal `packages/bundle/base/cordis.patch.yml` named by the note. | `exact-code-occurrence` |
| [`docs/subsystems/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/README.md) | package contract and examples | Contains the exact code literal `session-telemetry/record` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `cwd` | `const` | [`packages/fs/tool-fs/src/session-cwd.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts#L24) | `const cwd = exec.agent?.session.header.cwd` |
| `url` | `const` | [`packages/session/session-telemetry-otel/src/index.ts:170`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/index.ts#L170) | `const url = config.exporter?.url` |

### Tests and executable evidence

- [`packages/session/session-telemetry-otel/tests/otel.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/tests/otel.spec.ts) — A test under the owning area exercises or imports `DISABLED`. A test under the owning area exercises or imports `FULL`.
- [`packages/session/session-telemetry-otel/tests/loader-composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/tests/loader-composition.e2e.ts) — A test under the owning area exercises or imports `session-telemetry-otel`. A test under the owning area exercises or imports `DISABLED`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — Contains the exact code literal `packages/bundle/base/cordis.patch.yml` named by the note.
- [`scripts/verify-config-source-ownership.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-config-source-ownership.spec.ts) — Contains the exact code literal `packages/bundle/base/cordis.patch.yml` named by the note.

## How to read the implementation

1. Start with [`packages/bundle/base/cordis.patch.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/cordis.patch.yml) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/security`, `domain/session-state`, `domain/testing`, `lifecycle/implemented`
- Aliases: `cwd`, `url`, `packages/bundle/base/cordis.patch.yml`, `session-telemetry-otel`, `DISABLED`, `FULL`, `FEEDBACK_ONLY`, `DSH_TELEMETRY_MODE`, `DSH_TELEMETRY_OTLP_URL`, `https://harness-telemetry.deepseeksvc.com/v1/logs`, `DSH_TELEMETRY_DISABLED`, `processor.scheduledDelayMillis: 10000`, `exporter.timeoutMillis: 1000`, `maxExportBatchSize: 2048`
- Regex: `(?i)(packages/bundle/base/cordis\.patch\.yml|session\-telemetry\-otel|DISABLED|FULL|FEEDBACK_ONLY|DSH_TELEMETRY_MODE|DSH_TELEMETRY_OTLP_URL|https://harness\-telemetry\.deepseeksvc\.com/v1/logs)`

```bash
rg -n --pcre2 "(?i)(packages/bundle/base/cordis\\.patch\\.yml|session\\-telemetry\\-otel|DISABLED|FULL|FEEDBACK_ONLY|DSH_TELEMETRY_MODE|DSH_TELEMETRY_OTLP_URL|https://harness\\-telemetry\\.deepseeksvc\\.com/v1/logs)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0338. Bounded, escalating signal shutdown for Web and headless](0338-bounded-escalating-signal-shutdown-for-web-and-headless.md): The source note links to this decision directly.
- **`source-link`** — [0176. Session telemetry seam with mandatory redaction and the OTel backend](0176-session-telemetry-seam-with-mandatory-redaction-and-the-otel-backend.md): The source note links to this decision directly.
- **`source-link`** — [0233. SessionTelemetryBackend anonymous user id ($DSH_HOME/.anonymous-user-id) and the OTel Resource user.id](0233-sessiontelemetrybackend-anonymous-user-id-dsh-home-anonymous-user-id-and.md): The source note links to this decision directly.
- **`source-link`** — [0286. SessionTelemetryBackend requires explicit opt-in](0286-sessiontelemetrybackend-requires-explicit-opt-in.md): The source note links to this decision directly.
- **`shares-code-with`** — [0258. Feedback-gated session telemetry](0258-feedback-gated-session-telemetry.md): Shares source implementation: `packages/session/session-telemetry-otel`, `packages/session/session-telemetry-otel/README.md`.
- **`shares-code-with`** — [0109. Shared anonymous user id across telemetry, feedback, and DeepSeek requests](0109-shared-anonymous-user-id-across-telemetry-feedback-and-deepseek-requests.md): Shares source implementation: `packages/session/session-telemetry-otel`, `packages/session/session-telemetry-otel/README.md`.
- **`shares-code-with`** — [0273. Feedback acknowledgement sharing disclosure](0273-feedback-acknowledgement-sharing-disclosure.md): Shares source implementation: `packages/session/session-telemetry-otel`, `packages/session/session-telemetry-otel/src/index.ts`.
- **`shares-code-with`** — [0130. Multiplex concurrent ACP sessions over one connection](0130-multiplex-concurrent-acp-sessions-over-one-connection.md): Shares source implementation: `packages/fs/tool-fs/src/session-cwd.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0236-default-session-telemetry-mount-otel-reporting-in-the-dsh-web-compositio.md`.
