---
id: "dsh-note-0233"
title: "SessionTelemetryBackend anonymous user id ($DSH_HOME/.anonymous-user-id) and the OTel Resource user.id"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-31-telemetry-anonymous-user-id.md"
implementation_evidence: "medium"
target_anchor: "append-only event and session store"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/projection"
aliases:
  - "version"
  - "getOrCreateAnonymousUserId"
  - "OpenTelemetrySessionBackend"
  - "resolveDshHome"
  - "service.name"
  - "service.version"
  - "$DSH_HOME/.anonymous-user-id"
  - "$DSH_HOME"
  - "~/.dsh"
  - "user.id"
  - "session-telemetry-otel"
  - "/feedback"
  - "@deepseek-ai/dsh-anonymous-user-id"
  - ".anonymous-user-id"
search_regex: "(?i)(version|getOrCreateAnonymousUserId|OpenTelemetrySessionBackend|resolveDshHome|service\\.name|service\\.version|\\$DSH_HOME/\\.anonymous\\-user\\-id|\\$DSH_HOME)"
---

# 0233. SessionTelemetryBackend anonymous user id ($DSH_HOME/.anonymous-user-id) and the OTel Resource user.id — implementation context

## Open this when

Session telemetry is mounted by default (default-mount Note), but the OTel Resource carried only service.name/service.version --- no user-level identity at all, so the collector could neither aggregate per user nor count active users. The only prior ruling on point was an unimplemented one to derive a user id by hashing the hostname/local IP. The OTel feed needed an anonymous user identity with clean semantics.

## Source decision

getOrCreateAnonymousUserId() returns the bare UUID line in $DSH_HOME/.anonymous-user-id (resolved by resolveDshHome, $DSH_HOME > ~/.dsh), minting and persisting a random UUID v4 on first use; the backend constructor carries it as the Resource's user.id (the OTel semconv user attribute), once per export batch. The original implementation lived inside session-telemetry-otel because no second real consumer existed.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-31-telemetry-anonymous-user-id.md](../02-notes/implemented/feature/2026-07-31-telemetry-anonymous-user-id.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-31-telemetry-anonymous-user-id.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-31-telemetry-anonymous-user-id.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/util/home-paths/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/index.ts) | package entry point | Core file in the package named by the note: `packages/util/home-paths`. Defines `resolveDshHome`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/util/home-paths/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/util/home-paths`. | `named-package-member` |
| [`packages/identity/anonymous-user-id/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/identity/anonymous-user-id/src/index.ts) | package entry point | Core file in the package named by the note: `packages/identity/anonymous-user-id`. Defines `getOrCreateAnonymousUserId`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/identity/anonymous-user-id/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/identity/anonymous-user-id/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/identity/anonymous-user-id`. | `named-package-member` |
| [`packages/session/session-telemetry-otel/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session/session-telemetry-otel`. Defines `OpenTelemetrySessionBackend`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session/session-telemetry-otel/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-telemetry-otel`. | `named-package-member` |
| [`packages/util/home-paths`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/identity/anonymous-user-id`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/identity/anonymous-user-id) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session/session-telemetry-otel`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/e2b/fs-e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts) | package entry point | Defines `version`, a construct named by the note. | `symbol-definition` |
| [`packages/util/home-paths/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/README.md) | package contract and examples | Core file in the package named by the note: `packages/util/home-paths`. | `named-package-member` |
| [`packages/util/home-paths/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/package.json) | composition and configuration | Core file in the package named by the note: `packages/util/home-paths`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `version` | `const` | [`packages/e2b/fs-e2b/src/index.ts:389`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L389) | `const version = await this.writeAtomic(` |
| `getOrCreateAnonymousUserId` | `function` | [`packages/identity/anonymous-user-id/src/index.ts:68`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/identity/anonymous-user-id/src/index.ts#L68) | `export function getOrCreateAnonymousUserId(options: AnonymousUserIdOptions = {}): AnonymousUserId {` |
| `OpenTelemetrySessionBackend` | `class` | [`packages/session/session-telemetry-otel/src/index.ts:147`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/src/index.ts#L147) | `export class OpenTelemetrySessionBackend extends SessionTelemetryBackend {` |
| `resolveDshHome` | `function` | [`packages/util/home-paths/src/index.ts:87`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/index.ts#L87) | `export function resolveDshHome(configured?: string, env: Record<string, string \| undefined> = process.env): string {` |

### Tests and executable evidence

- [`packages/util/home-paths/tests/home-paths.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/tests/home-paths.spec.ts) — A test under the owning area exercises or imports `resolveDshHome`. A test under the owning area exercises or imports `$DSH_HOME`.
- [`packages/session/session-telemetry-otel/tests/otel.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/tests/otel.spec.ts) — A test under the owning area exercises or imports `getOrCreateAnonymousUserId`. A test under the owning area exercises or imports `OpenTelemetrySessionBackend`.
- [`packages/identity/anonymous-user-id/tests/anonymous-user-id.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/identity/anonymous-user-id/tests/anonymous-user-id.spec.ts) — A test under the owning area exercises or imports `getOrCreateAnonymousUserId`.
- [`packages/session/session-telemetry-otel/tests/loader-composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry-otel/tests/loader-composition.e2e.ts) — A test under the owning area exercises or imports `session-telemetry-otel`.

## How to read the implementation

1. Start with [`packages/util/home-paths/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/observability`, `domain/session-state`, `domain/storage`, `lifecycle/implemented`, `mechanism/projection`
- Aliases: `version`, `getOrCreateAnonymousUserId`, `OpenTelemetrySessionBackend`, `resolveDshHome`, `service.name`, `service.version`, `$DSH_HOME/.anonymous-user-id`, `$DSH_HOME`, `~/.dsh`, `user.id`, `session-telemetry-otel`, `/feedback`, `@deepseek-ai/dsh-anonymous-user-id`, `.anonymous-user-id`
- Regex: `(?i)(version|getOrCreateAnonymousUserId|OpenTelemetrySessionBackend|resolveDshHome|service\.name|service\.version|\$DSH_HOME/\.anonymous\-user\-id|\$DSH_HOME)`

```bash
rg -n --pcre2 "(?i)(version|getOrCreateAnonymousUserId|OpenTelemetrySessionBackend|resolveDshHome|service\\.name|service\\.version|\\$DSH_HOME/\\.anonymous\\-user\\-id|\\$DSH_HOME)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0109. Shared anonymous user id across telemetry, feedback, and DeepSeek requests](0109-shared-anonymous-user-id-across-telemetry-feedback-and-deepseek-requests.md): The source note links to this decision directly.
- **`source-link`** — [0236. Default session-telemetry mount (OTel reporting) in the dsh web composition](0236-default-session-telemetry-mount-otel-reporting-in-the-dsh-web-compositio.md): The source note links to this decision directly.
- **`source-link`** — [0291. DeepSeek request user and session identity headers](0291-deepseek-request-user-and-session-identity-headers.md): The source note links to this decision directly.
- **`shares-code-with`** — [0512. Storage root placement and derived-medium recovery](0512-storage-root-placement-and-derived-medium-recovery.md): Shares source implementation: `packages/util/home-paths`, `packages/util/home-paths/src/index.ts`.
- **`shares-code-with`** — [0286. SessionTelemetryBackend requires explicit opt-in](0286-sessiontelemetrybackend-requires-explicit-opt-in.md): Shares source implementation: `packages/session/session-telemetry-otel`, `packages/session/session-telemetry-otel/src/index.ts`.
- **`shares-code-with`** — [0258. Feedback-gated session telemetry](0258-feedback-gated-session-telemetry.md): Shares source implementation: `packages/session/session-telemetry-otel`, `packages/session/session-telemetry-otel/src/index.ts`.
- **`shares-code-with`** — [0176. Session telemetry seam with mandatory redaction and the OTel backend](0176-session-telemetry-seam-with-mandatory-redaction-and-the-otel-backend.md): Shares source implementation: `packages/session/session-telemetry-otel/src/index.ts`, `packages/session/session-telemetry-otel/src/invariant.ts`.
- **`shares-code-with`** — [0136. Workspace context instruction files](0136-workspace-context-instruction-files.md): Shares source implementation: `packages/util/home-paths/src/index.ts`, `packages/util/home-paths/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0233-sessiontelemetrybackend-anonymous-user-id-dsh-home-anonymous-user-id-and.md`.
