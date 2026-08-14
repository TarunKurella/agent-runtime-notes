---
id: "dsh-note-0565"
title: "Exact session query service"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-10-session-query-service.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/trust"
  - "domain/build-release"
  - "domain/llm"
  - "domain/observability"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/tools"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "readEvent"
  - "SessionStore"
  - "live"
  - "foldSurface"
  - "SurfaceManager"
  - "SessionHeader"
  - "sessionQuery"
  - "persisted"
  - "before"
  - "after"
  - "traceEvent"
  - "traceSession"
  - "SessionRecord"
  - "@deepseek-ai/dsh-session-query"
search_regex: "(?i)(readEvent|SessionStore|live|foldSurface|SurfaceManager|SessionHeader|sessionQuery|persisted)"
---

# 0565. Exact session query service — implementation context

## Open this when

Session history exists in two places: current SessionStore objects and an optional persistence backend. Consumers that need exact inspection would otherwise duplicate live-versus-persisted precedence, persistence lifecycle handling, raw-event surface classification, relationship tracing, and defensive cloning. Durable state can lag the live log between checkpoints, so persistence alone is not a truthful current source. Full-text search is related but materially larger.

## Source decision

@deepseek-ai/dsh-session-query owns the single abstract ctx.sessionQuery service over one logical corpus. It concretely implements listSessions(), provider-independent filterSessions(filters), listEvents(sessionId), filterEvents(sessionId, filters), bounded readEvent(request), traceSession(sessionId), and traceEvent(request), while concrete backends implement its two full-text methods. The unified service decision owns that topology, the SQLite search decision owns search behavior, and the tracing decision owns lineage and event-relationship semantics.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-10-session-query-service.md](../02-notes/archived/feature/2026-07-10-session-query-service.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-10-session-query-service.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-10-session-query-service.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. Defines `SessionStore`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. Defines `SessionHeader`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/surface.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `foldSurface`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/session-query/session-query/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session-query/session-query`. Defines `before`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session-query/session-query/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/session-query/session-query`. Defines `SessionRecord`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session-query/session-query/src/corpus.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/corpus.ts) | runtime implementation | Core file in the package named by the note: `packages/session-query/session-query`. Defines `live`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session-query/session-query/src/tracing.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/tracing.ts) | runtime implementation | Core file in the package named by the note: `packages/session-query/session-query`. Defines `traceSession`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session-query/session-query/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session-query/session-query`. | `named-package-member` |
| [`packages/core/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/session) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session-query/session-query`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query) | package or module directory | The note names this package or capability. | `named-package` |
| [`.github/issue-management/policy.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/issue-management/policy.mjs) | repository automation | Defines `readEvent`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `readEvent` | `function` | [`.github/issue-management/policy.mjs:689`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/issue-management/policy.mjs#L689) | `function readEvent() {` |
| `SessionStore` | `class` | [`packages/core/session/src/index.ts:792`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L792) | `export class SessionStore extends Service {` |
| `live` | `const` | [`packages/core/session/src/index.ts:1147`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L1147) | `const live = this.get(source.id)` |
| `foldSurface` | `function` | [`packages/core/session/src/surface.ts:387`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L387) | `export function foldSurface(events: readonly SessionEvent[]): SurfaceFoldResult {` |
| `SurfaceManager` | `class` | [`packages/core/session/src/surface.ts:398`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L398) | `export class SurfaceManager implements SessionSurface {` |
| `SessionHeader` | `interface` | [`packages/core/session/src/types.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L61) | `export interface SessionHeader {` |
| `sessionQuery` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2043`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2043) | `const sessionQuery = ctx.get('sessionQuery')` |
| `persisted` | `const` | [`packages/session-query/session-query/src/corpus.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/corpus.ts#L61) | `const persisted = persistence === undefined ? [] : await listPersisted(persistence, signal)` |
| `live` | `const` | [`packages/session-query/session-query/src/corpus.ts:90`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/corpus.ts#L90) | `const live = this._ctx.sessions.get(sessionId)` |
| `before` | `const` | [`packages/session-query/session-query/src/index.ts:308`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/index.ts#L308) | `const before = this._readWindow('before', request.before)` |
| `after` | `const` | [`packages/session-query/session-query/src/index.ts:309`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/index.ts#L309) | `const after = this._readWindow('after', request.after)` |
| `traceEvent` | `function` | [`packages/session-query/session-query/src/tracing.ts:65`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/tracing.ts#L65) | `export function traceEvent(` |
| `traceSession` | `function` | [`packages/session-query/session-query/src/tracing.ts:113`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/tracing.ts#L113) | `export function traceSession(` |
| `SessionRecord` | `interface` | [`packages/session-query/session-query/src/types.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/types.ts#L24) | `export interface SessionRecord {` |

### Tests and executable evidence

- [`packages/core/session/tests/surface.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/surface.spec.ts) — A test under the owning area exercises or imports `foldSurface`. A test under the owning area exercises or imports `SurfaceManager`.
- [`packages/host/apiproxy/tests/api-proxy-fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-fork.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/host/apiproxy/tests/session-export.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/session-export.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/host/apiproxy/tests/api-proxy-search.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-search.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/session-query/session-query/tests/tracing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/tests/tracing.spec.ts) — A test under the owning area exercises or imports `SessionStore`. A test under the owning area exercises or imports `sessionQuery`.
- [`packages/session-query/session-query/tests/session-query.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/tests/session-query.spec.ts) — A test under the owning area exercises or imports `SessionStore`. A test under the owning area exercises or imports `sessionQuery`.
- [`packages/session-query/session-query/tests/search-helpers.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/tests/search-helpers.spec.ts) — A test under the owning area exercises or imports `SessionStore`. A test under the owning area exercises or imports `sessionQuery`.
- [`scripts/client-bundle-purity.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-purity.spec.ts) — Contains the exact code literal `dsh-session` named by the note.

## How to read the implementation

1. Start with [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/trust`, `domain/build-release`, `domain/llm`, `domain/observability`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/tools`, `lifecycle/archived`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `readEvent`, `SessionStore`, `live`, `foldSurface`, `SurfaceManager`, `SessionHeader`, `sessionQuery`, `persisted`, `before`, `after`, `traceEvent`, `traceSession`, `SessionRecord`, `@deepseek-ai/dsh-session-query`
- Regex: `(?i)(readEvent|SessionStore|live|foldSurface|SurfaceManager|SessionHeader|sessionQuery|persisted)`

```bash
rg -n --pcre2 "(?i)(readEvent|SessionStore|live|foldSurface|SurfaceManager|SessionHeader|sessionQuery|persisted)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0552. Unified session query service](0552-unified-session-query-service.md): The source note links to this decision directly.
- **`shares-code-with`** — [0154. Session query relationship tracing](0154-session-query-relationship-tracing.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0038. Initiating Agent scope over AsyncLocalStorage](0038-initiating-agent-scope-over-asynclocalstorage.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0460. Collapse live persistence into one flush controller](0460-collapse-live-persistence-into-one-flush-controller.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0061. Web client Agent-scope parity model and the provisioning channel (agents/scope / blank reuse / provide)](0061-web-client-agent-scope-parity-model-and-the-provisioning-channel-agents.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0231. Permission Settings default for new sessions](0231-permission-settings-default-for-new-sessions.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0565-exact-session-query-service.md`.
